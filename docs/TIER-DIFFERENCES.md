# What differs between the 8 GB and 12 GB launchers

Both ship the same flagship — **Bonsai 2 27B, with vision**. They differ in two
ways: **where the vision projector runs**, and **how much headroom** the card has.

## Vision: CPU on 8 GB, GPU on 12 GB

The original prototype ran everything on the GPU. Sending the vision projector to
the **CPU** (`--no-mmproj-offload`) was added **only** to fit the 27B + vision on
an **8 GB** card. The **12 GB** launcher (build **2026-11-02a**) puts the
projector **back on the GPU**, where image reading is much faster.

| | **8 GB** (10-13b) | **12 GB** (11-02a) |
|---|---|---|
| Vision projector | **CPU** (`--no-mmproj-offload`) | **GPU** (flag removed) |
| Image read speed | ~**45 s** (measured, i7-6800K) | **much faster** — GPU; measure on the card |
| 27B context | 8K + quantized KV (`-c 8192 -ctk/-ctv q8_0`) | 16K + full KV (`-c 16384`) |
| Coding model | Qwen2.5-Coder **7B** | Qwen2.5-Coder **14B** |
| Image models | 4 (DreamShaper, Realistic Vision, SDXL, Juggernaut) | 5 (+ Z-Image Turbo) |

I'm not quoting a hard number for 12 GB GPU vision — the RX 6700 XT is far above
the i7-6800K's CPU projector, so expect well under the 8 GB figure, but **measure
it** and set the customer number from the bench.

## ⚠️ Two ship gates — test before releasing

1. **8 GB:** quantized KV (`-ctk/-ctv q8_0`) needs a recent **Vulkan**
   `llama-server`. Launch **[2] Assistant + Vision** on a real 8 GB card and
   confirm the 27B loads/answers. If not: drop `-ctk/-ctv q8_0` (keep `-c 8192`);
   still tight → `-c 6144`.
2. **12 GB (GPU vision):** the 27B (~5.8 GB) + projector (~0.6 GB) + 16K **full**
   KV can be tight on a 12 GB card. Launch **[2]** on a real 12 GB card and
   confirm it loads. If it OOMs, the launcher has a commented fallback: add
   `--no-mmproj-offload` back (CPU vision), or trim `-c 16384` → `-c 12288`.

Text chat is brisk on every tier regardless.
