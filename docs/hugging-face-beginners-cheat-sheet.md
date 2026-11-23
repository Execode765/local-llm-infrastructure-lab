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

---

## Addendum A — AI Model Platforms & Alternatives

> These are **not all direct Hugging Face competitors**. Some host model files, some make local models easy to run, and others sell hosted inference. Use this section to recognize what kind of site you are visiting.

| Platform | Main role | How it compares with Hugging Face | Why I might visit |
| --- | --- | --- | --- |
| [Hugging Face](https://huggingface.co/) | Model and dataset hub, demos, developer tools, hosted inference | Broad reference point | Inspect model cards, weights, licenses, datasets |
| [ModelScope](https://modelscope.cn/) | Model/dataset community and hosting platform | One of the closer model-hub alternatives | Discover additional models, datasets, and demos |
| [Kaggle](https://www.kaggle.com/) | Datasets, notebooks, competitions, and model resources | More focused on data science and experimentation | Browse datasets and reproducible notebooks |
| [Ollama Library](https://ollama.com/library) | Catalog of models packaged for Ollama | Narrower: focused on models users can run via Ollama | Find supported model tags for the existing local lab |
| [LM Studio](https://lmstudio.ai/) | Desktop application for finding, downloading, and running local models | Local runtime/tool, not a general-purpose model hub | Compare local inference workflows without changing the lab |
| [GitHub](https://github.com/) | Source-code and release hosting | Hosts implementations and sometimes model assets, but is not primarily a model hub | Check original code, issues, releases, documentation |
| [Replicate](https://replicate.com/) | Hosted model execution and APIs | More focused on running models as a cloud service | Understand managed inference and API deployment |
| [Together AI](https://www.together.ai/) | Cloud AI inference and training services | More focused on hosted compute than model archiving | Learn how cloud model serving differs from local inference |
| [OpenRouter](https://openrouter.ai/) | Unified API gateway to multiple hosted models/providers | A routing/access layer, not a general model-file repository | Compare hosted model availability and API options |

### Quick category decoder

- **Model hub:** A place to find model repositories, weights, metadata, and often datasets. Example: Hugging Face or ModelScope.
- **Local runtime / model catalog:** Software and curated listings for running compatible models on your own machine. Example: Ollama or LM Studio.
- **Cloud inference provider:** Runs models on remote hardware and typically exposes an API. Example: Replicate or Together AI.
- **API aggregator/router:** Gives access to multiple hosted models/providers through one interface. Example: OpenRouter.
- **Code and research community:** Stores implementation code, experiments, and related materials. Example: GitHub or Kaggle.

### Questions to ask when visiting a new AI platform

1. **Am I downloading weights or sending requests to someone else's server?**
2. **Does this platform host the original model, a converted version, or only API access?**
3. **What format and runtime are supported?** GGUF, Safetensors, Ollama, Transformers, etc.
4. **Is this free to browse, free to download, or metered/pay-per-use?** These are different things.
5. **What are the license, privacy, and data-handling terms?**
6. **Is this useful for my current local lab, or just interesting background reading?**

### Personal rule for this repository

**Browsing and learning are allowed; no new installation, purchase, account, subscription, or separate project is implied.** Keep the existing Local LLM Infrastructure Lab priorities intact. Verify any model's actual compatibility and current service pricing before use.

_Platform features, availability, and pricing can change. This is a navigation guide, not an endorsement or a current pricing comparison._
