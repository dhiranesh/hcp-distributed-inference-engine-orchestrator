# HCP Distributed Inference Engine & Orchestrator

A high-performance distributed inference engine and orchestrator designed for heterogeneous computing clusters (GPUs, TPUs, NPUs, and CPUs).

---

## 🎯 Purpose & Scope

This repository hosts the **core implementation codebase** for the HCP distributed inference runtime and orchestrator. It is engineered to maximize throughput and minimize latency when executing large-scale machine learning models across heterogeneous, asymmetric hardware accelerators.

- **Implementation Codebase:** All production-ready engine code, RPC services, kernel bindings, and scheduling logic reside in this repository.
- **Companion Research Repository:** Theoretical models, hardware profiling notes, experimental benchmarks, and architecture papers are maintained privately in the companion research module (`research/`).

---

## 🏗️ Architecture Overview

The platform is divided into three primary layers:

```
┌────────────────────────────────────────────────────────┐
│               HCP Orchestrator & Gateway               │
│   (Request Admission, SLO Queueing, Dynamic Batching)   │
└──────────────────────────┬─────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────┐
│           Heterogeneous Workload Scheduler             │
│ (Asymmetric Stage Partitioning, Pipeline/Tensor Routing)│
└──────────────────────────┬─────────────────────────────┘
                           │
         ┌─────────────────┼─────────────────┐
         ▼                 ▼                 ▼
   ┌───────────┐     ┌───────────┐     ┌───────────┐
   │ GPU Node  │     │ NPU Node  │     │ CPU Node  │
   │ (Worker)  │     │ (Worker)  │     │ (Worker)  │
   └───────────┘     └───────────┘     └───────────┘
```

1. **Orchestrator & API Gateway:**
   - Unified OpenAI-compatible streaming API.
   - Dynamic request batching and priority scheduling with Time-to-First-Token (TTFT) and Inter-Token Latency (ITL) guarantees.

2. **Heterogeneous Scheduler:**
   - **Hardware-Aware Partitioning:** Automatically splits models across nodes with unequal VRAM and compute capacity.
   - **Dynamic KV-Cache Pooling:** Unified memory addressing across local accelerator memory and host RAM.

3. **Inference Workers:**
   - Low-latency inter-node communication over gRPC and RDMA/RoCE.
   - Optimized execution kernels with custom device backends.

---

## 📁 Repository Structure

```
.
├── src/
│   ├── engine/          # Distributed inference execution kernels and worker runtimes
│   ├── orchestrator/    # Request router, batch manager, and load balancer
│   ├── scheduler/       # Heterogeneous device profiling and stage partitioning
│   └── proto/           # Protobuf definitions for inter-node communication
├── tests/               # Unit, integration, and distributed stress tests
├── research/            # [Submodule] Internal research & architectural designs (private)
├── .gitignore
├── .gitmodules
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- Git 2.30+
- CUDA Toolkit / ROCm / OpenVINO (optional, depending on target accelerator)

### Cloning the Repository

To clone the public codebase:
```bash
git clone https://github.com/dhiranesh/hcp-distributed-inference-engine-orchestrator.git
cd hcp-distributed-inference-engine-orchestrator
```

If you have authorized access to the private research submodule:
```bash
git submodule update --init --recursive
```

---

## 📜 License

This project is licensed under the Apache 2.0 / MIT License.
