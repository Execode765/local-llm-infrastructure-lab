# Why Do Different AI Models Give Different Answers?

*Local LLM Infrastructure Lab — Concepts Explained, Lesson 02*

## 1. The Big Question

Have you ever asked two people the exact same question and received two completely different answers?

One person might answer immediately. Another might spend several minutes thinking, explaining every possibility, and eventually reach a different conclusion.

Artificial intelligence can behave similarly.

Different AI models may respond to the same question with different explanations, levels of detail, speeds, and degrees of accuracy.

But why?

In this experiment, we explored how two locally installed AI models handled the same questions, what happened when we changed their settings, and whether spending more time generating an answer actually made that answer better.

## 2. Think of It Like Two Students Taking the Same Test

Imagine two students sitting in a classroom.

Both receive the same math problem.

The first student writes several pages of calculations, considers multiple approaches, and takes ten minutes to finish.

The second student works through the problem more briefly and finishes in two minutes.

Which student gave the better answer?

You can't know until you check their work.

The first student might have made a mistake despite spending more time. The second might have answered correctly, or might have overlooked something important.

AI models present a similar challenge.

A longer explanation doesn't necessarily mean better reasoning, and a faster answer doesn't automatically mean less accuracy.

**The important question isn't simply how long the AI takes. It's whether the answer is correct, useful, and appropriate for the task.**

## 3. What Makes AI Models Behave Differently?

Several factors influence how an AI model responds.

**Training:** Models learn statistical patterns from large amounts of data during development. Different training data and methods can lead to different capabilities and behaviors.

**Model architecture:** This describes how the model is designed to process information. Different architectures may have different strengths and limitations.

**Model size:** Models contain numerical values called parameters. These help determine how the model processes information. More parameters can increase capability, but a larger model isn't automatically better at every task.

**Instructions and settings:** The way a question is written, how much information the model can consider, and the software settings used can all influence its response.

**Generation behavior:** Some models can produce lengthy reasoning-style output before presenting a final answer. This may help with certain problems, but it also consumes time and computing resources.

One especially important term is the *context window*.

Think of it as the amount of working material an AI model can keep available during a particular interaction.

If the model reaches that limit, it may be unable to continue its response normally.

## 4. What Did We Actually Test?

For Lab 02, we used two AI models running locally on a Windows gaming laptop:

- Qwen3 4B
- Qwen3.5 4B

Both were tested using Ollama and an NVIDIA RTX 4070 Laptop GPU with 8 GB of dedicated video memory.

Our first question sounded simple:

**"How many Fridays are in a leap year? Does it have more or less than a standard year?"**

Here's why that question is more interesting than it looks.

A standard year contains 365 days, or 52 complete weeks plus one extra day.

A leap year contains 366 days, or 52 complete weeks plus two extra days.

That means both types of years can have either 52 or 53 Fridays, depending on which weekday the year begins.

A leap year doesn't automatically have more Fridays than a standard year.

We already knew the correct mathematical rules, so we could compare the models' answers against a reliable answer key.

### What happened?

Some Qwen3 responses correctly identified the basic rules but made incorrect or contradictory claims when comparing the years.

Qwen3.5 sometimes generated so much text that its response ended in the middle of a sentence.

When we increased the available context in a separate test, Qwen3.5 completed its answer, but still included a mathematical error in an additional claim.

One recorded response took approximately 109 seconds and generated 6,965 tokens.

That is a substantial amount of processing for a relatively simple calendar question.

**Our discovery: Producing more text and spending more time on a problem did not guarantee a completely correct answer.**

These observations came from a small number of tests. They do not establish which model is generally more accurate or capable.

## 5. What Happened When We Asked a Practical IT Question?

Next, we tried something closer to a real-world technical support situation.

We described a Windows computer that had assigned itself an IP address beginning with `169.254` and had no default gateway.

In everyday language, the computer had a networking problem.

It likely couldn't obtain a normal network address from the service responsible for assigning addresses automatically.

We asked Qwen3.5 to explain the likely problem, recommend an initial check, and identify a Windows command that could help investigate.

This time, the model produced a short three-bullet response in approximately 8.3 seconds.

It correctly identified a likely DHCP problem and suggested the useful command `ipconfig /all`.

However, it skipped an important first step: checking whether the computer was physically or wirelessly connected to the network.

It also included some inaccurate technical guidance.

So the response was helpful, but not entirely correct.

### Why does that matter?

Imagine using AI to help troubleshoot your home internet connection.

It might suggest a useful command or point you toward the likely cause.

But if you follow every suggestion without checking it, you could waste time or make unnecessary changes.

AI can be a valuable troubleshooting assistant, but its recommendations still need verification.

## 6. Why Would Anyone Want to Understand This?

This experiment offers several practical lessons.

**Choosing an AI assistant:** Different models may be better suited to different tasks. Testing them with questions relevant to your needs can help you make an informed choice.

**Saving time:** A concise, accurate answer can be more useful than a long explanation that takes several minutes to generate.

**Avoiding mistakes:** Confident-sounding answers aren't proof of correctness. Important information should be checked against reliable sources.

**Understanding hardware:** Longer responses generally require additional processing. When running AI locally, this affects response time and computing-resource usage.

**Using AI at work:** Whether you're troubleshooting computers, reviewing documents, or learning something new, AI works best when its output can be evaluated rather than blindly accepted.

There are limitations to what our experiment proves.

We tested only a small number of responses using particular model builds, prompts, and settings. We also encountered unexpected behavior when attempting to disable reasoning-style output.

We therefore cannot conclude that one model is universally faster, smarter, or more reliable than the other.

## 7. What Did We Learn?

Our biggest lesson was that AI performance has more than one dimension.

Speed matters, but so does accuracy.

A response can be technically impressive and still contain mistakes.

A short response can be useful without being complete.

And a model that performs well on one question may struggle with another.

We also learned why controlled testing matters.

If we change the question, software settings, and available context at the same time, it becomes difficult to determine exactly which change caused a difference in performance.

That's an important principle in computer science, engineering, and everyday problem-solving.

**To understand whether something works better, we need to know what we're measuring and keep the comparison fair.**

## 8. Three Things to Remember

1. **Different AI models can give different answers to the same question.** Their training, design, settings, and generation behavior influence the results.
2. **More thinking-style text doesn't automatically mean more accuracy.** Long explanations can still contain mistakes.
3. **AI should be tested and verified, especially when the answer matters.** A useful assistant isn't necessarily an infallible one.

---

## Explore the Actual Experiment

This article is the beginner-friendly companion to [Lab 02 — Qwen Inference Testing](../lab-02-qwen-inference.md).

The technical documentation contains the original prompts, model configurations, timing measurements, token counts, troubleshooting observations, answer evaluations, and limitations.

Readers interested in reproducing the experiment can follow the documented PowerShell commands and Ollama API examples.

## AI-Assisted Educational Documentation

This guide was developed with assistance from ChatGPT by OpenAI, using the repository owner's documented hands-on experiments as the primary source.

The repository owner performed the actual model testing, collected the observations, and documented the results. AI assistance was used to help explain technical concepts, interpret findings, and prepare accessible educational material.

The explanations distinguish between verified experimental observations and broader educational concepts.

**Project philosophy: Build → Test → Understand → Document.**
