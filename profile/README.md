<div align="center">

<h1>AI Computing</h1>

<h3>Systems for heterogeneous AI infrastructure: GPUs, NPUs and unified memory under one roof</h3>

<p>
  <a href="https://github.com/ai-computing/ai-rm"><img src="https://img.shields.io/badge/ai--rm-integration%20hub-7c5cff" alt="ai-rm"></a>
  <a href="https://github.com/ai-computing/rm-core/blob/main/PROTOCOL.md"><img src="https://img.shields.io/badge/protocol-v0.1-7c5cff" alt="Protocol v0.1"></a>
  <a href="https://github.com/ai-computing/aicomp"><img src="https://img.shields.io/badge/engine-Optimus%20Prime-107e72" alt="Optimus Prime"></a>
  <a href="https://github.com/ai-computing/onggi"><img src="https://img.shields.io/badge/artifacts-Onggi-c08457" alt="Onggi artifact store"></a>
  <img src="https://img.shields.io/badge/target-Kubernetes%20%2B%20llm--d-326ce5?logo=kubernetes&amp;logoColor=white" alt="Kubernetes + llm-d">
</p>

<p>
  <a href="https://github.com/search?q=org%3Aai-computing+topic%3Aai-rm&amp;type=repositories"><b>All ai-rm repositories</b></a> ·
  <a href="https://github.com/ai-computing/ai-rm/blob/master/docs/status.md"><b>Status</b></a> ·
  <a href="https://github.com/ai-computing/ai-rm/blob/master/docs/e2e.md"><b>End-to-end run</b></a> ·
  <a href="https://github.com/ai-computing/ai-rm/blob/master/docs/workload-traces.md"><b>Workload traces</b></a>
</p>

</div>

We build the software that lets inference and training run across accelerators from different vendors —
NVIDIA GPUs, FuriosaAI RNGD and Rebellions ATOM NPUs, CPUs and GB10 unified memory — on Kubernetes and llm-d.

## 🧩 AI-RM — unified resource management

[**ai-rm**](https://github.com/ai-computing/ai-rm) is the integration hub: five technical layers connected by
[**rm-core**](https://github.com/ai-computing/rm-core) contracts, pinned component versions and reproducible measurements.

| Layer | Repository | What it does |
|:-:|---|---|
| L1 | [rm-accel](https://github.com/ai-computing/rm-accel) | Discovers GPUs and NPUs on each node and publishes them as `AcceleratorProfile`s and node labels |
| L2 | [hetero-npu-p2p](https://github.com/ai-computing/hetero-npu-p2p) | Same-host KV transport between NPUs of different vendors (RNGD → ATOM) |
| L2 | [npu-offload](https://github.com/ai-computing/npu-offload) | MoE expert offloading: hot experts in NPU HBM, cold experts on the host |
| L2 | [onggi](https://github.com/ai-computing/onggi) | Content-addressed weights and compiled artifacts: deduplication, node caches, peers, file delta apply and read-only CSI volumes |
| L3 | [rngd-pd](https://github.com/ai-computing/rngd-pd) | Prefill/decode-disaggregated serving across RNGD and ATOM as llm-d pods |
| L3 | [npu-train](https://github.com/ai-computing/npu-train) | LoRA and full fine-tuning on inference-only NPUs, with A100-matching quality |
| L4 | [hetero-llm-d](https://github.com/ai-computing/hetero-llm-d) | A heterogeneous Kubernetes + llm-d cluster: gateway, EPP, model store, monitoring |
| L4 | [rm-controller](https://github.com/ai-computing/ai-rm/tree/master/packages/rm-controller) | Topology-aware placement of prefill/decode pools |
| L5 | [lmd-top](https://github.com/ai-computing/lmd-top) | Terminal console for observing and operating llm-d clusters |
| L5 | [rm-bench](https://github.com/ai-computing/ai-rm/tree/master/packages/rm-bench) | End-to-end load, real-trace replay and SLO measurement |

## 🏺 Onggi — model and compiled-artifact delivery

[**Onggi (옹기)**](https://github.com/ai-computing/onggi) stores shared bytes once across model checkpoints and
precompiled accelerator variants, then restores the required files through node caches, peers and Kubernetes CSI.
Its P2 implementation includes POSIX/S3 storage, hash verification, garbage collection and delta file restoration.
The compile farm and automatic prefetch from placement/autoscaling signals remain planned.

Onggi delivers files; serving runtimes select compatible prepared graphs and perform supported NPU-memory updates.
A batch or sequence-length change still needs a compatible precompiled variant. This separates artifact storage
from the runtime capabilities of RNGD external weights, ATOM device-input weights and ATOM embedded-weight graphs.

## 🚂 Optimus Prime — one engine

[**aicomp**](https://github.com/ai-computing/aicomp) hosts **Optimus Prime** (`opt_prime`), a PyTorch parallelization framework
(pipeline, data and tensor parallelism, LoRA, an inference engine with explicit prefill/decode) that is being extended into the unified
inference runtime for the engine pieces above, and **FASOP**, a cost-based search for parallelization strategies on
heterogeneous GPU clusters (HPDC '24).

## 🔭 Also here

- [LLMServingSim](https://github.com/ai-computing/LLMServingSim) — a simulator for heterogeneous and disaggregated LLM serving infrastructure
- [LLMServingOptimizer](https://github.com/ai-computing/LLMServingOptimizer) — design-space exploration and planning for LLM serving clusters

<sub>Browse by topic: <code>ai-rm</code>, <code>ai-rm-l1</code> … <code>ai-rm-l5</code>, <code>optimus-prime-source</code>.</sub>
