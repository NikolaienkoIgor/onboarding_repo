# Dell hardware (Corrector on the customer’s server)

Parts for the **S / M / L** Dell builds on the [Corrector](corrector.md) page. Sized for **Qwen3 8B** Corrector (UBL XML).

**Speed baseline on cloud:** Gemini 3.5 Flash Corrector in about **~30 seconds**.  
**Default on-prem pick:** **M = R760-class + one NVIDIA L4**.

Prices are **average net B2B Germany (~2026)** rough numbers. Always get a fresh quote.

---

## Quick map

| Tier | What we pick |
|------|----------------|
| **S — CPU** | **R360** or **R660** · one Xeon · **64 GB** · NVMe · **no GPU** |
| **M — GPU** | **R760** / **R760xa** · one Xeon · **64–128 GB** · **one L4** · GPU kit |
| **L — more GPU** | Same as M · **128 GB** · **one L40S** *or* **two L4s** · stronger power supplies |

---

## Server — R360 / R660 (S) and R760 / R760xa (M, L)

### S — R360 or R660

| | |
|--|--|
| **What we buy** | PowerEdge **R360** or **R660** · iDRAC — about **~€3.0k** |
| **Why** | Cheaper one-CPU box for **queued / overnight** Corrector. Fine for Docker + CPU model runs; not for “feels like cloud (~30 s)”. |
| **Looks like** | 1U rack server. [R360](https://www.dell.com/en-us/shop/dell-poweredge-servers/poweredge-r360-rack-server/spd/poweredge-r360), [R660](https://www.dell.com/en-us/shop/dell-poweredge-servers/poweredge-r660-rack-server/spd/poweredge-r660). |

### M / L — R760 or R760xa

!!! note "What **2U** and **xa** help with"
    - **2U** is the height of the box in the rack (about **9 cm**, twice a thin **1U** server like the R360 / R660). That extra height is what lets us put a GPU inside, plus the fans and power it needs so it does not overheat. A **1U** box is usually too short for an L4 or L40S. **2U helps Corrector run on a GPU** instead of the slow CPU.
    - **xa** (the **R760xa**) is the same R760 family, but the inside is already set up for a GPU: the right slots, power, and cooling. A plain R760 can take a GPU with extra parts; **xa makes that the normal setup**, so the quote is cleaner and the card fits without a fight. **xa helps us actually install the GPU** that gets Qwen near the cloud **~30 s** time.

| | |
|--|--|
| **What we buy** | PowerEdge **R760** / **R760xa** · iDRAC — about **~€4.2k** |
| **Why** | **2U** server that can take a GPU. Use for normal Corrector (**M**) and for bigger GPU setups (**L**) without changing the whole story. Prefer **R760xa** if the quote is for a GPU box. |
| **Looks like** | **2U** rack server. [R760](https://www.dell.com/en-us/shop/dell-poweredge-servers/poweredge-r760-rack-server/spd/poweredge-r760). |

![Dell PowerEdge rack servers (family photo)](https://upload.wikimedia.org/wikipedia/commons/thumb/1/13/Dell_PowerEdge_Servers.jpg/960px-Dell_PowerEdge_Servers.jpg)

*Dell PowerEdge rack servers (Wikimedia Commons). Use the Dell product pages above for current photos.*

---

## CPU — one Xeon Silver-class

| | |
|--|--|
| **What we buy** | **One** Xeon Silver-class — about **~€0.9k** (all tiers) |
| **Why** | Runs the OS, Docker, API, and validation. On **S** it also runs the model (slow). On **M/L** the GPU does most of the model work — one mid Xeon is enough. |
| **Looks like** | Server CPU. Example family: [Intel Xeon Silver](https://www.intel.com/content/www/us/en/products/details/processors/xeon/scalable.html). |

---

## Memory — 64 GB (S/M) · 128 GB (L)

| | |
|--|--|
| **What we buy** | **S:** **64 GB** — **~€0.4k** · **M:** **64–128 GB** — **~€0.4k–0.8k** · **L:** **128 GB** — **~€0.8k** |
| **Why** | Room for OS, Docker, and the model. Floor elsewhere is **16 GB**; these builds are more comfortable without putting 128 GB on every quote. |
| **Looks like** | DDR5 ECC sticks in the server. |

---

## GPU — L4 (M) · L40S or two L4s (L)

### NVIDIA L4 24 GB (M; also two on L)

| | |
|--|--|
| **What we buy** | **One L4** on **M** — **~€5.3k** · **two L4s** on **L** — **~€10.6k** |
| **Why** | Right size for Qwen3 8B. One L4 ≈ cloud **~30 s** for one job. Two L4s = two jobs at once without each one slowing down. |
| **Looks like** | |

![NVIDIA L4](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/l4/proviz-l4-og.jpg)

Source: [NVIDIA L4](https://www.nvidia.com/en-us/data-center/l4/)

### NVIDIA L40S 48 GB (L option)

| | |
|--|--|
| **What we buy** | **One L40S** — **~€11.3k** |
| **Why** | Faster / more headroom than one L4: one 8B job can match or beat cloud **~30 s**, plus longer context, more parallel work, or bigger models later. **Too much for 8B alone** — not the default Corrector SKU. |
| **Looks like** | |

![NVIDIA L40S](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/l40s/nvidia-l40s-og-1200x630.jpg)

Source: [NVIDIA L40S](https://www.nvidia.com/en-us/data-center/l40s/)

---

## GPU kit

| | |
|--|--|
| **What we buy** | Riser / cables / shrouds — **~€0.6k** (M) / **~€0.8k** (L) |
| **Why** | Fits and cools the GPU in the R760 / R760xa. Needed for **M** and **L**, not for **S**. |
| **Looks like** | Metal riser and cables inside the server. |

---

## Power supply

| | |
|--|--|
| **What we buy** | **S:** dual entry — **~€0.5k** · **M:** dual for one L4 — **~€0.9k** · **L:** stronger dual — **~€1.0k** |
| **Why** | Steady power and hot-swap if one supply fails. Upsize on **L** when GPUs draw more. |
| **Looks like** | Hot-plug modules at the back of the server. |

---

## Boot disk — NVMe

| | |
|--|--|
| **What we buy** | NVMe boot (optional mirror) — **~€0.5k** |
| **Why** | OS and Docker. Model files can sit on another disk. |
| **Looks like** | M.2 or U.2 NVMe in the server. |

---

## Network

| | |
|--|--|
| **What we buy** | **S:** 1GbE or 10GbE — **~€0.2k** · **M/L:** 10GbE preferred — **~€0.3k** |
| **Why** | Reach the Corrector API and move XML files. |
| **Looks like** | Ports on the back of the server. |

---

## Support — ProSupport

| | |
|--|--|
| **What we buy** | **S:** ProSupport 3y — **~€1.5k** · **M:** **~€2.2k** · **L:** Mission Critical — **~€2.8k** |
| **Why** | Dell repairs and support. Mission Critical for important **L** boxes. |
| **Looks like** | Service contract (no box). |

---

Back to Corrector: [Hardware requirements](corrector.md#hardware-requirements).
