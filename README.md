# Awesome-ML-Feature-Stores

## Top ML Inference Engines Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Hardware-Specific Compilation, Cross-Platform Execution Providers & Edge Deployment*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **ML Inference Engines**. These tools execute trained models on target hardware — applying kernel fusion, quantization, and hardware-specific optimizations to turn a framework checkpoint into fast, efficient predictions on GPUs, CPUs, NPUs, and mobile silicon.

**Examples** include ONNX Runtime, TensorRT, OpenVINO, Apache TVM, Core ML, and Vitis AI (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, execution provider flexibility, and transparent hardware acceleration — ideal for ML engineers deploying models across heterogeneous hardware without rewriting inference code per target.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **Apple Core ML** — **Apple's on-device inference framework for Apple silicon.** Runs models entirely on-device across CPU, GPU, and **Neural Engine (ANE)**, with typical **2–5x speedups via ANE** compared to unoptimized execution [citation:7]. **Core ML Tools** converts PyTorch and TensorFlow models, with support for weight compression and stateful Transformer execution . **Best for iOS and macOS applications** wanting native Apple hardware acceleration.

## Open-Source GitHub Projects

- **[ONNX Runtime](https://github.com/microsoft/onnxruntime)** — **The cross-platform inference accelerator with the broadest hardware coverage through Execution Providers (EPs).** MIT licensed. **EPs are the interface between ONNX Runtime and hardware acceleration libraries** — you select the provider for your target, and the same ONNX model runs on NVIDIA GPUs (CUDA/TensorRT), Intel hardware (OpenVINO), Qualcomm Snapdragon (QNN), Apple devices (CoreML), and browsers (WebGPU/WebAssembly) [citation:2]. **v1.24.2** upgraded TensorRT EP to TensorRT 10.14, QNN to SDK 2.42.0, and OpenVINO to 2025.4.1, with Flash Attention and GQA kernel fusion for CUDA [citation:4]. **The decision rule**: ONNX is the portability choice, but **vendor-specific runtimes win on raw performance** — TensorRT is 2–4x faster than PyTorch on NVIDIA with FP16, and OpenVINO delivers 1.5–3x on Intel CPUs [citation:7]. **Best for teams wanting one runtime across heterogeneous hardware** without maintaining separate inference stacks per vendor.

- **[TensorRT](https://github.com/NVIDIA/TensorRT)** — **The peak-performance inference optimizer for NVIDIA GPUs.** **Delivers 2–4x speedup with FP16 and 4–8x with INT8** compared to unoptimized PyTorch execution on NVIDIA hardware [citation:7]. **Engine compilation fuses layers, auto-tunes kernels, and calibrates precision** — the engine is device-specific and must be rebuilt for different GPU architectures (an A100 engine won't run on a Jetson Orin) [citation:7]. **Engine builds take 10–60 seconds** on edge devices — bake engines into the device image to avoid first-boot delays [citation:7]. **Best for any model that will run on NVIDIA hardware** where maximum throughput justifies the compilation step.

- **[OpenVINO](https://github.com/openvinotoolkit/openvino)** — **Intel's inference toolkit for CPUs, integrated GPUs, and NPUs.** Apache 2.0 licensed. **Delivers 1.5–3x speedup on Intel CPUs** compared to unoptimized execution [citation:7]. **OpenVINO 2026.4** requires **AVX2 instruction set minimum** (SSE no longer supported) and dropped Debian 10 and Ubuntu 20.04 support [citation:6]. **The actively maintained CPU acceleration path** for Intel hardware, with NNCF quantization and pruning tools. **Best for self-hosted Intel CPU/iGPU/NPU deployments.**

- **[Apache TVM](https://github.com/apache/tvm)** — **The open-source compiler stack for diverse hardware.** Apache 2.0 licensed. **Compiles models from ONNX, TensorFlow, and PyTorch to optimized code** for CPUs, GPUs, and custom accelerators. **TVM 0.27.0** added TensorRT engine embedding during code generation and expanded Relax frontend coverage for PyTorch, ONNX, and TFLite [citation:5]. **Performance varies by architecture**: TVM delivers substantial gains for **convolutional models** but **performance drops significantly for Transformers** compared to vendor-specific runtimes — a search-based optimizer navigating a complex space [citation:15]. **Best for teams targeting non-mainstream hardware** or building custom compilation pipelines.

- **[Vitis AI](https://github.com/Xilinx/Vitis-AI)** — **AMD/Xilinx's inference development platform for FPGAs and adaptive SoCs.** **One of the most active toolchains by contributor count (408)** alongside OpenVINO [citation:8]. **Supports deployment to Zynq UltraScale+ FPGAs** with quantization-aware compilation. **Best for FPGA-based edge inference** in embedded and industrial applications.

### Additional Strong Open-Source Options

- **Qualcomm QNN (AI Engine Direct)** — Snapdragon NPU, GPU, and CPU acceleration. Accessible through ONNX Runtime's QNN Execution Provider [citation:2]. **Best for Android and Windows on ARM deployments** targeting Snapdragon hardware.
- **AWS Neuron** — Inference acceleration for AWS Inferentia and Trainium silicon. **Best for AWS-native teams** wanting cost-optimized inference on AWS custom silicon.
- **NVIDIA Triton Inference Server** — Multi-framework serving platform that can run TensorRT, PyTorch, ONNX, and OpenVINO backends. **Best for enterprises serving multiple model types** from one server.
- **Apache TVM Unity** — The next-generation TVM architecture with Relax IR for end-to-end model optimization. Part of the active TVM 0.27 release line [citation:5].

**Frameworks for building custom systems**: Combine **ONNX Runtime** for portable cross-hardware execution with selectable EPs, **TensorRT** for maximum NVIDIA GPU throughput, **OpenVINO** for Intel CPU/NPU acceleration, **Core ML** for Apple Neural Engine deployment, and **Apache TVM** for non-mainstream hardware targets — to build an inference stack tailored to your target hardware mix, latency requirements, and tolerance for device-specific engine compilation.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Inference engines differ significantly in hardware target, compilation complexity, and performance profile. **Vendor-specific runtimes consistently outperform generic compilers** — TensorRT on NVIDIA and OpenVINO on Intel maintain advantages that generic tools like TVM struggle to match, especially for Transformers [citation:15]. **TensorRT engines are device-specific** and must be rebuilt for different GPU architectures [citation:7]. **ONNX Runtime's EP model provides portability** but often at the cost of peak performance compared to native vendor runtimes [citation:2]. **OpenVINO 2026.4 requires AVX2** and dropped older OS support [citation:6].
- Self-hosted open-source solutions require proper conversion environment isolation, validation datasets for accuracy comparison after quantization, and awareness that engine compilation adds deployment-time complexity.

---

**Made for ML engineers deploying models across heterogeneous hardware with performance-critical inference.**
Let's make ML inference engines more open, hardware-aware, and deployment-transparent.
