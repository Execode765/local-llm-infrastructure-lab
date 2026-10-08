Lab 01 — Ollama Deployment and GPU Verification

Status: In Progress

1. Objective

Deploy a locally hosted large language model using Ollama on a Windows computer and verify that inference workloads are being processed by the GPU.

The purpose of this lab is to gain hands-on experience with local AI infrastructure, model deployment, GPU acceleration, and basic performance evaluation.

2. Lab Environment

Component	Configuration
Operating System	Windows
LLM Runtime	Ollama
GPU	To be documented
System RAM	To be documented
CPU	To be documented
Model	To be documented
Model Quantization	To be documented

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
