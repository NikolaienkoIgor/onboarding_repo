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

### Dell on-prem sizing (S / M / L)

What from a **Dell PowerEdge** build matches Corrector on **Qwen3 8B** (3 LLM checks), relative to cloud Gemini *speed* (not Gemini accuracy).

Prices = **net B2B Germany (~2026)** list / market ranges. Shared chassis base from Dell R760 quotes; GPU choices differentiate the tiers.

| | **S — CPU** | **M — GPU** | **L — more GPU** |
|---|-------------|-------------|------------------|
| **Fit for Corrector** | Batch / internal / async only | Interactive single-user / small team | Concurrent jobs, longer context, room for larger models later |
| **Processing time** (typical Peppol UBL, up to 3 LLM checks, Q4/Q5, thinking off) | **~3–10+ min** (~8–20 tok/s) | **~30 s – 2 min** (~50–100 tok/s) | **~15–60 s** (~100–200+ tok/s on 8B; headroom) |
| **vs Gemini 3.5 Flash latency** | Much slower | Same *order of magnitude* for single jobs | Closest local feel; still not identical cloud SLA |
| **Equipment** | Dell PowerEdge **R760** 2U · 2× Xeon Silver **4410Y** (12c/24t) · **128 GB** DDR5 ECC · dual redundant PSU (e.g. 1100–1400 W class) · BOSS-N1 + 2× M.2 NVMe RAID 1 · dual 10GbE · **no GPU** · ProSupport 3y | Same R760 base · **1× NVIDIA L4 24 GB** (or **RTX 4000 Ada 20 GB** as cheaper workstation/server GPU) · GPU riser / cables / shrouds · stronger PSU if required · ProSupport 3y | Same R760 base · **1× NVIDIA L40S 48 GB** · Dell GPU riser kit + Gen5 power + air shrouds · **2× 2400 W** Titanium/Platinum hot-plug (dual-GPU ready) · ProSupport 3y Mission Critical |
| **Equipment price (net, per major piece)** | Chassis+board+iDRAC: **€3.8k–4.5k** · 2× CPU: **€1.4k–1.8k** · 128 GB RAM: **€0.7k–0.95k** · PSUs: **€0.5k–0.8k** · Boot RAID: **€0.65k–0.85k** · 10GbE: **€0.35k–0.5k** · ProSupport: **€1.8k–2.5k** | Base as S (PSU often **€0.7k–1.0k**) · **L4**: **€4.5k–6.0k** *or* **RTX 4000 Ada**: **€2.0k–2.5k** · GPU kit: **€0.4k–0.7k** · ProSupport: **€2.2k–3.0k** | Chassis: **€3.8k–4.5k** · 2× CPU: **€1.4k–1.8k** · RAM: **€0.7k–0.95k** · 2× 2400 W PSU: **€0.8k–1.1k** · **L40S**: **€10.5k–12.0k** · GPU kit: **€0.6k–0.9k** · Boot RAID: **€0.65k–0.85k** · 10GbE: **€0.35k–0.5k** · ProSupport: **€2.4k–3.2k** |
| **Hardware investment total (net)** | **~€9.5k – €12.5k** | **~€15k – €20k** (with L4) · **~€13k – €17k** (with RTX 4000 Ada) | **~€21k – €26k** |

!!! tip "Which Dell tier matches our local Corrector setup?"
    Compose + **Qwen3 8B** for Corrector maps most cleanly to **M (1× L4 / RTX 4000 Ada)**: enough VRAM for Q4/Q5 at 8k context and latency near cloud for single jobs. **S** is only for queued/batch. **L (L40S)** is overkill for 8B alone but right if you want concurrency, Qwen 32B later, or dual-GPU expansion.

!!! note "Price sources"
    **L** component ranges follow DE B2B market quotes for R760 + L40S (2026). **M** GPU bands from EU reseller list prices (L4 ~€4.5k–6k; RTX 4000 Ada ~€2.0k–2.5k excl. VAT). **S** is the same R760 BOM without GPU/GPU kit and with lighter PSUs. Always re-quote Dell/reseller — list prices move.

### If you need accuracy closer to Gemini 3.5

| Approach | Hardware implication |
|----------|----------------------|
| Stay on **Gemini 3.5 Flash** (cloud) | No local GPU; API latency only |
| Larger open weights (e.g. **Qwen3 32B** Q4) | Prefer **M with L4** minimum; **L (L40S)** more comfortable |
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
