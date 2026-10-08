Lab 01 — Ollama Deployment and GPU Verification

Status: In Progress

1. Objective

Deploy a locally hosted large language model using Ollama on a Windows computer and verify that inference workloads are being processed by the GPU.

The purpose of this lab is to gain hands-on experience with local AI infrastructure, model deployment, GPU acceleration, and basic performance evaluation.

## 2. Lab Environment

The following hardware was used to deploy and test the Qwen3:4B model.

| Component | Specification |
|---|---|
| CPU | Intel Core i9-13980HX |
| GPU | NVIDIA GeForce RTX 4070 Laptop GPU |
| GPU VRAM | 8 GB |
| System RAM | 64 GB |
| Operating System | Windows 11 Home (64-bit), build 26200 |
| NVIDIA Driver | 566.07 |
| CUDA Version Reported by Driver | 12.7 |
| LLM Runtime | Ollama |
| Model | Qwen3:4B |
| Quantization | Q4_K_M |
| Storage | Pending verification |

### GPU Monitoring Results

The NVIDIA System Management Interface (`nvidia-smi`) was used to inspect GPU activity during local LLM operation.

```powershell
nvidia-smi
```

**Observed results — October 8, 2026**

| Metric | Observed Value |
|---|---|
| GPU utilization | 97% |
| GPU memory usage | 3,185 / 8,188 MiB |
| GPU temperature | 61°C |
| GPU power consumption | 81 W |
| Reported power limit | 113 W |
| GPU compute process | llama-server.exe |

### Analysis

Ollama previously reported 100% GPU allocation for the Qwen3:4B model.

A separate NVIDIA monitoring snapshot reported 97% GPU utilization, demonstrating substantial GPU activity during the observation.

These results represent initial monitoring observations rather than a controlled performance benchmark.

Future testing will measure inference throughput, latency, and GPU resource consumption under repeatable workloads.

3. Pre-Deployment Preparation

Before deploying the local LLM environment, a full system image was created using AOMEI Backupper.

A bootable recovery USB drive was also prepared and tested to confirm that the recovery environment could launch.

This provided a recovery option in the event of configuration errors or system instability.

Note: Booting successfully into recovery media does not by itself verify that a complete system restore will succeed.

4. Ollama Deployment

Ollama was installed and configured to run a language model locally.

The initial deployment involved:

1. Preparing the Windows environment.
2. Installing Ollama.
3. Downloading the selected language model.
4. Running an interactive inference session.
5. Testing model responses.

Exact installation commands and model identifiers will be added after verification.

5. GPU Verification

The following command was used to inspect active model allocation:

ollama ps

During testing, Ollama reported 100% GPU allocation for the running model.

This indicates that Ollama reported the model as fully GPU-resident during the observation.

It does not necessarily mean the GPU was operating at 100% computational utilization.

6. Initial Test Results

Test	Observation
Local model execution	Successful
Text generation	Responsive
GPU model allocation	100% reported
Tokens per second	Not yet measured
GPU memory usage	Not yet documented
CPU utilization	Not yet documented

7. Lessons Learned

* Local LLMs can execute on personally managed hardware without requiring a hosted inference service.
* Ollama provides command-line tools for managing and inspecting local models.
* GPU allocation and GPU utilization are different measurements.
* Performance observations should be supported by repeatable benchmarks.
* System backups and recovery planning are valuable preparation steps before infrastructure changes.

8. Next Steps

* Record exact hardware specifications.
* Identify the installed model and quantization.
* Add screenshots of successful inference and GPU allocation.
* Establish repeatable performance benchmarks.
* Compare future model configurations.


## Model Configuration and Verification

The deployed model was inspected using the following PowerShell commands:

```powershell
ollama list
ollama ps
ollama show qwen3:4b
```

### Verified Configuration

| Property | Value |
|---|---|
| Model | qwen3:4b |
| Model ID | 359d7dd4bcda |
| Architecture | qwen3 |
| Parameters | 4.0 billion |
| Quantization | Q4_K_M |
| Downloaded model size | 2.5 GB |
| Loaded model size | 3.2 GB |
| GPU allocation | 100% |
| Model maximum context | 262,144 tokens |
| Active inference context | 4,096 tokens |
| Embedding length | 2,560 |
| Inference backend | llama.cpp |

### Observations

The Qwen3:4B model was successfully deployed through Ollama on Windows.

The `ollama ps` command reported 100% GPU allocation, confirming that Ollama placed the loaded model entirely on the GPU at the time of observation.

The model uses Q4_K_M quantization, reducing its memory footprint compared with higher-precision representations.

Although the model supports a maximum context length of 262,144 tokens, the active Ollama session was configured with 4,096 tokens.

### Verification Status

- [x] Model installed
- [x] Model configuration inspected
- [x] Quantization identified
- [x] GPU allocation verified
- [ ] GPU utilization measured
- [ ] Inference throughput benchmarked
- [ ] Performance compared across models
