# Hugging Face for Beginners — Model Browser Cheat Sheet

> Personal quick reference for browsing Hugging Face. Not a new lab or a model-installation guide.

## What am I looking at?

| Term | Plain-English meaning | Why it matters |
| --- | --- | --- |
| **Model Hub** | A library of model repositories. | Find and compare AI models. |
| **Model repository** | A page containing model files, metadata, and documentation. | Similar to a GitHub repo, but often for model weights. |
| **Model card** | The model's README: intended uses, limitations, training/evaluation notes, and license (when supplied). | Read this before trusting or downloading anything. |
| **Architecture** | The neural-network design (e.g., transformer). | Determines how the model works and which runtimes support it. |
| **Base model** | A pretrained foundation model, not necessarily tuned to follow chat instructions. | Usually less convenient for conversational use. |
| **Instruct / Chat** | A model tuned to respond to instructions or dialogue. | Usually the starting point for an assistant. |
| **Parameters (B)** | Number of learned weights: 4B ≈ 4 billion, 8B ≈ 8 billion, 14B ≈ 14 billion. | More parameters often mean more memory and compute, but not automatically better answers. |
| **Quantization** | Storing weights at lower numerical precision, such as 4-bit or 8-bit. | Can reduce memory needs, sometimes with quality trade-offs. |
| **Q4 / Q8** | Common labels for roughly 4-bit / 8-bit quantization. | Useful when comparing local model sizes; exact schemes differ. |
| **GGUF** | A model-file format commonly used with llama.cpp-based runtimes. | Often relevant to Ollama and local CPU/GPU inference. |
| **Safetensors** | A common format for storing model tensors safely without pickle execution. | Often used by Transformers; not automatically Ollama-ready. |
| **Weights** | The learned numerical values inside a trained model. | The main files you download to run a model locally. |
| **Tokenizer** | Converts text into token IDs and back. | Must be compatible with the model. |
| **Context window** | How many tokens a model can process in one context, subject to runtime limits. | Longer prompts/conversations use more memory and processing. |
| **Inference** | Running a trained model to generate an output. | This is what your Ollama lab benchmarks. |
| **Fine-tuning** | Further training an existing model for particular tasks or behaviors. | Different from simply prompting or running it. |
| **Dataset** | A collection of examples used for training, testing, or evaluation. | Helps explain how models are built and assessed. |
| **Benchmark** | A test or score for a model on defined tasks. | Useful comparison, but not a guarantee of real-world performance. |
| **License** | Rules governing use, modification, and redistribution. | A public download is not automatically unrestricted or open source. |
| **Gated model** | A repository requiring access approval or acceptance of terms. | May require login and permission to download. |
| **Spaces** | Hosted interactive AI demos/apps. | Try examples in a browser; availability and costs vary. |
| **Inference Providers / Endpoints** | Ways to run models on hosted compute via an API. | Cloud execution, not your local GPU; charges may apply. |
| **Downloads / Likes** | Popularity/activity indicators. | Not proof of accuracy, safety, or quality. |
| **Revision / Commit** | A specific version of repository files. | Helps make experiments reproducible. |

## How to read a model page (30-second check)

1. **Who published it?** Check the organization/account and whether it is the intended source.
2. **What does it do?** Read the model card, especially intended use and limitations.
3. **Which variant?** Base vs Instruct; parameter count; architecture.
4. **Can I run it?** Check format, quantization, runtime compatibility, and actual RAM/VRAM requirements.
5. **Can I use it legally?** Read the license and any gated access terms.
6. **Can I trust the claims?** Look for reproducible evaluations, not only likes or downloads.

## Applying this to my local setup

- **Laptop:** NVIDIA RTX 4070 Laptop GPU with **8 GB dedicated VRAM**.
- **Runtime:** Ollama, with small quantized Qwen models already used in the infrastructure lab.
- **4B Q4** may be a reasonable *candidate* to investigate; it is **not** a guarantee of fitting entirely in VRAM.
- **8B Q4** may fit in some configurations, but context length, KV cache, runtime overhead, and other GPU usage matter.
- **14B Q4** commonly exceeds practical 8 GB VRAM budgets for full GPU residency; CPU/RAM offload may be possible but slower.
- Model file size **is not** total runtime memory use.
- A Hugging Face listing **does not** guarantee Ollama compatibility; verify the architecture, format, and import/run instructions.
- Browsing model pages requires **no installation, purchase, or new project**.

## Quick mental model

**Hugging Face = discover and inspect models.**  
**Ollama = manage and run compatible models locally.**  
**GitHub = version and document my infrastructure experiments.**

## Reference links

- [Hugging Face Models](https://huggingface.co/models)
- [Hugging Face documentation](https://huggingface.co/docs)
- [Hugging Face Spaces](https://huggingface.co/spaces)
- [Hugging Face Datasets](https://huggingface.co/datasets)

_Last reviewed: November 2026. A personal learning reference, not a hardware compatibility certification._
