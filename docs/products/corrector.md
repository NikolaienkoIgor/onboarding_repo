# Corrector

Corrector fixes invalid Peppol BIS / UBL invoice XML (and related EN 16931 files). It asks the language model to rewrite the XML, then checks it with Schematron/XSD. That repeats until the file is valid or we hit the retry limit (usually **3** model calls).

**Cloud today:** **Gemini 3.5 Flash**  
**On the customer’s own server:** usually **Qwen3 8B** (Ollama / llama.cpp)

---

## Baseline (cloud)

The speed and quality baseline for Corrector is **Gemini 3.5 Flash** in the cloud.

| Metric | Baseline |
|--------|----------|
| **Model** | **Gemini 3.5 Flash** (cloud) |
| **Corrector time** | **~30 seconds** |
| **Input tokens** (per model call) | **~6k** |
| **Output tokens** (per model call) | **~2k** |

!!! note "How to read this"
    **~30 seconds** is a normal full Corrector run on cloud (fix + checks), not only one tiny step. Token numbers are for **one** model rewrite. If the invoice needs all 3 retries, you pay for about 3× those tokens. Schematron/XSD checks are usually quick next to the model.

On a local **Qwen3 8B** box with no GPU, one model call is often around **~3 minutes**. With a mid GPU (see **M** below), one job can land near the cloud **~30 s**. With a stronger GPU setup (**L**), the same 8B job can be as fast as cloud or a bit faster — and you can run more jobs at once without each one dragging. Qwen3 8B still will not match Gemini **quality** on hard invoices.

---

## How one Corrector job uses the model

For a **UBL XML** invoice:

| Step | What happens | Tokens (rough) |
|------|----------------|----------------|
| Fix attempt 1–3 | Model rewrites XML from validation errors | **~6k** in + **~2k** out **each time** |
| Optional compare | Extra model call after a good file | One more call |

Most of the wait is the model writing tokens. Validation is rarely the bottleneck.

**Qwen3 8B context:** about **32K** tokens (roughly **30–32K** in practice). Raise the runtime context in Ollama if the default is too small, or large invoices get cut off.

---

## Hardware requirements

### What we are sizing

Running Corrector with **Qwen3 8B** on the customer’s machine (UBL XML), compared to cloud **Gemini 3.5 Flash** (**~30 s**, good quality).

| Goal | What hardware can do |
|------|----------------------|
| **Speed** vs Gemini (~30 s) | Yes — with enough GPU, one job can match cloud time or beat it a little. Extra GPUs mainly help when several people run Corrector at the same time |
| **Quality** vs Gemini | **Qwen3 8B will not match Gemini** — it is a practical on-prem choice, not the same quality |

!!! warning "Quality vs cloud"
    Buying a bigger GPU does not make Qwen3 8B as good as Gemini 3.5. For cloud-level quality, stay on Gemini or move to a much larger open model. The hardware below is so Qwen3 8B Corrector **runs well**, not so it **equals Gemini**.

Use at least **16 GB** system RAM (more is better). Keep context at least **8k** for real invoices; the model can go up to about **32K**.

### Dell on-prem sizing (S / M / L)

What we suggest from **Dell PowerEdge** for Qwen3 8B Corrector. Speed target = cloud Gemini **~30 s**. Details and photos: **[Dell hardware](dell-hardware.md)**.

Qwen3 8B needs roughly **8–12 GB** of GPU memory at a useful context — not a 48 GB card by default. Almost all wait time is the GPU (or CPU) generating text.

Prices = **average net B2B Germany (~2026)**.

| | **S — CPU** | **M — GPU** (default) | **L — more GPU** |
|---|-------------|------------------------|------------------|
| **Best for** | Background / queued jobs only | Day-to-day Corrector for one user or a small team | Several jobs at once, longer context, or larger models later |
| **Server** | **R360** or **R660** (one CPU) | **R760** / **R760xa** (can take a GPU) | Same **R760** / **R760xa** |
| **Time for UBL XML** | **~3–10+ min** (slow CPU) | About **~30 s – 2 min** (aim near cloud **~30 s** for one job) | About **~15–60 s** on 8B; can match or beat cloud for one job |
| **vs Gemini ~30 s** | Much slower | Same ballpark for one job | Can match or beat cloud for one job; extra GPU helps more when several jobs run together |
| **Typical cost (net, avg)** | **~€7k** | **~€16k** | **~€23k** (L40S) · **~€22k** (2× L4) |

!!! tip "What to recommend"
    Standard on-prem Corrector: **M = R760-class + one L4**. **S** is the cheap CPU box for overnight/queue work. **L** is the same server with **L40S** or **two L4s** — not a totally different product. Do not sell L40S as the default for 8B alone.

!!! note "Prices"
    Rough DE B2B averages (2026): light 1-CPU PowerEdge for **S**; R760-class + **L4 ~€5.3k** for **M**; **L40S ~€11.3k** or a second **L4** for **L**. Always re-quote Dell or a reseller.

### Parts list

Average net price next to each part.

| Part | **S — CPU** | **M — GPU** | **L — more GPU** |
|------|-------------|-------------|------------------|
| **Server** | PowerEdge **R360** or **R660** · iDRAC — **~€3.0k** | PowerEdge **R760** / **R760xa** · iDRAC — **~€4.2k** | Same R760 / R760xa — **~€4.2k** |
| **CPU** | **1×** Xeon Silver-class — **~€0.9k** | **1×** Xeon Silver-class — **~€0.9k** | **1×** Xeon Silver-class — **~€0.9k** |
| **Memory** | **64 GB** DDR5 ECC — **~€0.4k** | **64–128 GB** DDR5 ECC — **~€0.4k–0.8k** | **128 GB** DDR5 ECC — **~€0.8k** |
| **GPU** | **None** | **1× NVIDIA L4 24 GB** — **~€5.3k** | **1× NVIDIA L40S 48 GB** — **~€11.3k** *or* **2× L4** — **~€10.6k** |
| **GPU kit** | — | Riser / cables / shrouds — **~€0.6k** | Riser kit + Gen5 power + air shrouds — **~€0.8k** |
| **Power supply** | Dual (entry) — **~€0.5k** | Dual sized for one L4 — **~€0.9k** | Stronger dual (L40S / two L4s) — **~€1.0k** |
| **Boot disk** | NVMe (mirror optional) — **~€0.5k** | NVMe (mirror optional) — **~€0.5k** | NVMe (mirror optional) — **~€0.5k** |
| **Network** | 1GbE or 10GbE — **~€0.2k** | 10GbE recommended — **~€0.3k** | 10GbE recommended — **~€0.3k** |
| **Support** | ProSupport 3y — **~€1.5k** | ProSupport 3y — **~€2.2k** | ProSupport 3y Mission Critical — **~€2.8k** |

### If you need quality closer to Gemini 3.5

| Approach | Hardware note |
|----------|----------------|
| Stay on **Gemini 3.5 Flash** (cloud) | No local GPU; ~**30 s** Corrector |
| Bigger open model (e.g. **Qwen3 32B**) | Prefer **L with L40S**, or stay on cloud |
| Qwen3 8B only | Weaker fixes on hard invoices; **M + L4** is enough if the goal is **speed** |

### Several jobs at once

**M** numbers are for **one** Corrector job at a time. For two jobs on 8B:

- prefer **L with two L4s**, or  
- one **L40S** with enough memory for two jobs, or  
- accept about half the speed on one L4.

On **S** (CPU only), run one job at a time with a queue.

### Software on the server

- Docker Compose: app + **Ollama** (or llama.cpp / vLLM if you use a GPU host that way)
- Model: `qwen3:8b`; turn thinking **off** for Corrector so the model spends tokens on XML, not on long “reasoning”
- Linux x86_64; with a GPU, install NVIDIA drivers + nvidia-container-toolkit

### Checklist before handoff

- [ ] At least 16 GB RAM; SSD with ~20 GB free  
- [ ] Model downloaded once and kept on a volume  
- [ ] Context high enough for real invoices (prefer ≥ 8k; model supports ~32K)  
- [ ] GPU path: drivers + toolkit OK (`nvidia-smi` in the model container)  
- [ ] CPU-only (**S**): expect minutes per call, not ~30 s; background UI is fine  
- [ ] Do **not** claim Qwen3 8B matches Gemini 3.5 quality in sales or onboarding  

---

## Related

- Self-hosted Compose notes: `SELF_HOSTED.md` in the library API repo
- Cloud Corrector stays on Gemini
