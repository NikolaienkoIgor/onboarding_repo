# Corrector

Corrector repairs invalid Peppol BIS / UBL invoice XML (and related EN 16931 paths) by looping **LLM correction → Schematron/XSD validation** until the document passes or the attempt budget is exhausted.

Typical cloud configuration today uses **Gemini 3.5 Flash** with **`max_attempts = 3`** (up to three LLM correction rounds per request). Self-hosted deployments commonly target **Qwen3 8B** (Ollama / llama.cpp, quantized GGUF).

---

## How a request spends LLM budget

For a typical Peppol UBL Corrector job with **3 LLM checks**:

| Step | What happens | Rough token shape (order of magnitude) |
|------|----------------|----------------------------------------|
| Attempt 1–3 | LLM rewrites XML from validation errors | ~1.5–3k prompt + ~1–2k completion **per attempt** |
| Optional compare | Structural/XML compare after a successful artifact | One extra LLM call |

Worst case ≈ **3 full correction generations** (+ compare). Wall-clock time is dominated by **decode tokens/sec** of the local model, not by Schematron.

!!! note "Cloud reference (Gemini 3.5 Flash)"
    On cloud, a Corrector pass often completes in roughly **tens of seconds** depending on invoice size and how many attempts are needed. That is the **speed** benchmark for sizing local hardware. **Accuracy** of Gemini 3.5 is a separate bar — see below.

---

## Hardware requirements

### Scope of this chapter

Sizing for **Corrector on Qwen3 8B** with **3 LLM checks**, quantized **Q4_K_M / Q5_K_M** GGUF via Ollama or llama.cpp.

| Goal | What hardware can do |
|------|----------------------|
| **Speed** vs Gemini 3.5 Flash | Approach similar *interactive* latency with a mid/high GPU |
| **Accuracy** vs Gemini 3.5 Flash | **Not matched by Qwen3 8B** — 8B is a practical on-prem tradeoff, not a quality twin of Gemini 3.5 |

!!! warning "Quality vs cloud"
    Matching Gemini 3.5 **accuracy** is not a GPU-size problem you can solve with Qwen3 8B. Larger open models (e.g. 32B+) or staying on Gemini remain the path for cloud-parity quality. Hardware below is for running **Qwen3 8B Corrector** well locally.

### Model footprint (Qwen3 8B)

| Quantization | Disk / weights | Runtime RAM or VRAM (≈4k–8k context) |
|--------------|----------------|--------------------------------------|
| Q4_K_M | ~4.7–5.2 GB | ~6–8 GB (weights + KV + runtime) |
| Q5_K_M | ~5.5–6.5 GB | ~7–10 GB |

Recommend **≥16 GB system RAM** even with a GPU (OS, Docker, API, KV headroom). Prefer **≥8k context** for Corrector prompts; default 4k can truncate large invoices.

### Recommended tiers

#### 1. CPU-only Linux server (no GPU)

Usable for **internal / low concurrency** tooling — not for high-traffic interactive UX.

| Spec | Minimum | Comfortable |
|------|---------|-------------|
| CPU | 8+ cores modern Xeon / EPYC / Ryzen | 16+ cores, high memory bandwidth |
| RAM | 16 GB | 32 GB |
| Disk | 20 GB free (images + model) | SSD/NVMe |
| Expected decode | ~**8–20 tok/s** (Q4/Q5, good server CPU) | — |
| Corrector (3 attempts) | Often **several minutes** end-to-end | Acceptable for batch / async |

!!! tip "What “Linux server” means"
    A machine that **runs Linux natively** (cloud VM, bare metal, or office server you SSH into) — **not** a Linux container on Apple Silicon Docker. Mac Docker Ollama is CPU-in-a-VM and can be ~0.3 tok/s; that is not representative of Xeon/EPYC host performance.

#### 2. Single NVIDIA GPU (recommended for local / on-prem interactive Corrector)

| Spec | Entry (acceptable) | Recommended (closer to cloud *speed*) |
|------|--------------------|----------------------------------------|
| GPU | **RTX 4060 8 GB** (or similar 8 GB Ada/Ampere) | **RTX 4070 Ti / 4080 / L4 24 GB** |
| VRAM | 8 GB (Q4_K_M fits fully) | 12–24 GB (headroom for longer context / Q5) |
| System RAM | 16 GB | 32 GB |
| Expected decode (Q4_K_M) | ~**30–45 tok/s** | ~**50–100+ tok/s** |
| Corrector (3 attempts) | Roughly **~1–3 minutes** typical | Closer to **tens of seconds–~2 minutes** |

Rough wall-clock intuition (order of magnitude, single stream):

| Hardware | ~tok/s | One 1.5k-token generation | Up to 3 LLM checks |
|----------|--------|---------------------------|--------------------|
| Weak / nested Docker CPU | ~0.3–2 | Many minutes | Hours (impractical) |
| Server CPU Q4/Q5 | ~8–20 | ~1–3 min | ~3–10+ min |
| RTX 4060 class | ~30–45 | ~30–50 s | ~2–4 min worst case |
| High-end / datacenter GPU | ~80–120+ | ~15–20 s | Often under ~1–2 min |

Cloud Gemini will still often win on **tail latency** and **consistency** under load; local GPU aims for **same order of magnitude** for single-user Corrector, not identical SLA.

#### 3. If you need accuracy closer to Gemini 3.5

| Approach | Hardware implication |
|----------|----------------------|
| Stay on **Gemini 3.5 Flash** (cloud) | No local GPU; API latency only |
| Larger open weights (e.g. **Qwen3 32B** Q4) | Typically **≥24 GB VRAM** (4090 / L40 / A10-class) |
| Qwen3 8B only | Expect **weaker** Schematron-fix success vs Gemini on hard invoices |

### Concurrency

Figures above are **single-stream**. Two concurrent Corrector jobs roughly need either:

- two GPUs, or  
- one larger GPU with enough VRAM for two contexts, or  
- accept ~½ throughput on a single GPU.

CPU-only concurrency collapses quickly; prefer a queue (one job at a time) for CPU hosts.

### Software stack (self-hosted)

- Docker Compose: API container + **Ollama** (or llama.cpp / vLLM on GPU hosts)
- Model: `qwen3:8b` (Q4/Q5 GGUF); disable thinking for Corrector (`think=false`) so tokens go into XML, not chain-of-thought
- OS: Linux x86_64 with NVIDIA Container Toolkit when using a GPU

### Checklist before customer / on-prem handoff

- [ ] ≥16 GB RAM; SSD with ~20 GB free  
- [ ] Q4_K_M or Q5_K_M weights pulled once and persisted on a volume  
- [ ] Context ≥ 8192 for production invoices when possible  
- [ ] GPU: NVIDIA driver + nvidia-container-toolkit verified (`nvidia-smi` inside the inference container)  
- [ ] CPU-only: set expectations for **minutes-scale** Corrector, async UX preferred  
- [ ] Do **not** claim Gemini 3.5 accuracy parity for Qwen3 8B in sales/onboarding materials  

---

## Related

- Self-hosted Compose notes live with the API deployment docs (`SELF_HOSTED.md` in the library API repo)
- Cloud Corrector continues to use Gemini via the managed API path
