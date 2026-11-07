# Lab 02 — Qwen inference testing

Can Ollama's thinking setting reduce the time and tokens needed for a correct answer to a simple calendar question?

## Setup

- ASUS ROG Strix G814JI with an NVIDIA RTX 4070 Laptop GPU and 8 GB of dedicated VRAM
- Windows PowerShell 5.1 and Ollama 0.40.15, with matching CLI and server versions
- Models stored on the external SSD at `E:\.ollama\models`
- Q4_K_M builds of `qwen3:4b` and `qwen3.5:4b`. The tested Qwen3.5 build had model ID `d8b0f5e9760c` and used the `llamacpp` runner.

## Prompt and answer key

> How many Fridays are in a leap year? Does it have more or less than a standard year?

Both types of year have 52 full weeks. A standard year has one remaining day and has 53 Fridays only if January 1 is Friday. A leap year has two remaining days and has 53 Fridays if January 1 is Thursday or Friday. When comparing two specific years, a leap year can have fewer, the same number, or more Fridays.

## Results

Times are Ollama's reported durations in seconds, rounded to three decimals. Generated tokens use Ollama's `eval_count`. Each row represents one observed run.

| Model | Requested setting | Interface | Active context | Total (s) | Load (s) | Generation (s) | Generated tokens | Answer outcome |
|---|---|---|---:|---:|---:|---:|---:|---|
| Qwen3 4B | On | CLI | Unrecorded | 68.880 | 5.224 | 63.604 | 4,659 | Partial. Correct count rules, incorrect claim that a leap year can never have fewer Fridays. |
| Qwen3 4B | Off | CLI | Unrecorded | 62.600 | 0.004 | 62.049 | 4,549 | Partial. Correct range, contradictory comparison and incorrect calendar examples. |
| Qwen3 4B | Off with `/no_think` appended | CLI | Unrecorded | 59.589 | 5.218 | 54.326 | 4,030 | Partial. Correct count rules, overly broad conclusion of “more.” |
| Qwen3.5 4B | On | CLI | 4,096 | 63.295 | 0.003 | 62.873 | 4,066 | Ended mid-sentence before a final answer. |
| Qwen3.5 4B | Off | CLI | 4,096 | 63.103 | 0.004 | 62.667 | 4,064 | Ended mid-sentence while drafting the answer. |
| Qwen3.5 4B | Off | Chat API | 8,192 | 109.220 | 0.006 | 108.705 | 6,965 | Finished with `done_reason: "stop"`. Correct count rules, incorrect claim about the span needed for 54 Fridays. |

## What the diagnostics showed

The installed Qwen3 model reported only `true` under thinking levels, and its template opened a thinking block unconditionally. All three CLI runs produced lengthy reasoning text. About 5.22 seconds of the 6.28-second difference between the first two runs came from model load time.

The installed Qwen3.5 model advertised both `false` and `true`. Its CLI runs used exactly 4,096 tokens each when prompt and generated tokens were added together. Both ended mid-sentence, which strongly suggests they reached the active context limit.

At 8,192 tokens, the API run finished and `ollama ps` still showed `100% GPU`. The reported loaded size increased from 3.1 to 3.3 GB. The response took 109.220 seconds and included a long reasoning passage and a literal `</think>` in `message.content`, despite the request setting `think=false`. The final answer correctly gave the 52/53 rules, but the minimum span for 54 Fridays is 372 days, counting the first Friday as day one.

The 8,192-token run changed both the interface and context, so the data support a context-limit explanation without isolating context as the cause. With one run per condition and reasoning-like output present when thinking was requested off, this comparison leaves the effect of the thinking toggle unresolved.

## Reproduce the tests

Run the following in PowerShell with Ollama running and the models available locally. Use `ollama ps` to record active context and GPU allocation. Use `ollama show MODEL` to record the installed build's capabilities.

```powershell
$prompt = "How many Fridays are in a leap year? Does it have more or less than a standard year?"

ollama run qwen3:4b --think=true --verbose $prompt
ollama run qwen3:4b --think=false --verbose $prompt
ollama run qwen3:4b --think=false --verbose "$prompt /no_think"

ollama run qwen3.5:4b --think=false --keepalive 15m "Reply with only the word ready."
ollama run qwen3.5:4b --think=true --verbose --keepalive 15m $prompt
ollama run qwen3.5:4b --think=false --verbose --keepalive 15m $prompt
ollama ps
```

For the 8,192-token diagnostic, use the same `$prompt`.

```powershell
$body = @{
    model = "qwen3.5:4b"
    messages = @(@{ role = "user"; content = $prompt })
    think = $false
    stream = $false
    keep_alive = "15m"
    options = @{ num_ctx = 8192 }
} | ConvertTo-Json -Depth 5

$result = Invoke-RestMethod -Uri "http://localhost:11434/api/chat" `
    -Method Post -ContentType "application/json" -Body $body

$result | ConvertTo-Json -Depth 10
ollama ps
```

## Practical test — Windows DHCP troubleshooting

This test used the same Qwen3.5 4B build and Chat API settings as the 8,192-token Friday diagnostic, with `think=false`. To reproduce it, substitute this prompt for `$prompt` in the API example above.

> A Windows workstation has an IPv4 address of 169.254.23.10 and no default gateway. In three bullets, tell me what likely happened, what to check first, and which command shows the adapter's DHCP configuration.

| Total (s) | Load (s) | Generation (s) | Generated tokens | Active context | GPU allocation | Stop reason |
|---:|---:|---:|---:|---:|---:|---|
| 8.314 | 6.083 | 2.136 | 142 | 8,192 | 100% | `stop` |

**Partially correct.** The model followed the three-bullet format, identified the likely DHCP lease failure and IPv4 link-local fallback, and named `ipconfig /all`. The first check should be whether Ethernet has link or Wi-Fi is connected. The answer jumped to restarting the network stack or rebooting. It also suggested `Get-DhcpServerv4Lease`, which queries server-side leases, and incorrectly claimed `ipconfig /all` would display “No server responds.”

Microsoft references — [ipconfig](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ipconfig) and [Get-DhcpServerv4Lease](https://learn.microsoft.com/powershell/module/dhcpserver/get-dhcpserverv4lease).

Most of the 8.314-second wait was model loading. Generation took 2.136 seconds, compared with 108.705 seconds and 6,965 tokens for the Friday API run. The task and requested format both changed, and each was run once, so the difference is an observation across two prompts. This run returned the three bullets without a separate thinking trace. The broader effect of the thinking setting remains unresolved.
