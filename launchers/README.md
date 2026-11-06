# launchers/  (public — Vulkan / AMD)

The public **Vulkan (AMD)** launchers. The current, tested files are here.

## Launchers in this folder (Vulkan, public)
| Release | File | Build | 27B vision | Context | Coder | Image models |
|---|---|---|---|---|---|---|
| **4–6 GB** | `RR-AI-4GB.bat` | 2026-10-13a | — (no 27B) | — | — | DreamShaper |
| **8–12 GB** | `RR-AI-8GB.bat` | 2026-10-13b | **CPU** | 8K + q8_0 KV | 7B | 4 |
| **8–12 GB** | `RR-AI-12GB.bat` | **2026-11-02a** | **GPU** | 16K full KV | 14B | 5 (+Z-Image) |

See `../docs/TIER-DIFFERENCES.md` for the vision + headroom detail, and
`../docs/LAUNCHER-AUDIT-2026-11-02.md` for the audit trail.

## The model payload is NOT in the repo
The launchers point at `C:\Users\Public\RenewableRevolt\` (engine + models +
webui). That folder is the **Release asset**, built by **7-zip-slicing a golden
v7.5 machine's `RenewableRevolt` folder** into volumes (see release notes), not a
downloader script and not committed to git (`.gitignore` blocks `*.gguf` etc.).

## ⚠️ Ship gates — test before releasing
- **8 GB:** quantized KV needs a recent Vulkan `llama-server`. Run **[2]
  Assistant + Vision** on a real 8 GB card; if it won't load, drop `-ctk/-ctv
  q8_0` (keep `-c 8192`), then `-c 6144`.
- **12 GB (GPU vision):** 27B + projector + 16K full KV can be tight. Run **[2]**
  on a real 12 GB card; if it OOMs, re-add `--no-mmproj-offload` (CPU vision) or
  trim `-c 16384` → `-c 12288` (both noted in the launcher).

## LibreWolf detection — aligned ✓
All three Vulkan launchers run `where librewolf.exe` first, then fall back to
`C:\Program Files\LibreWolf\librewolf.exe` — matching the private CUDA line, so
the AI opens in LibreWolf wherever it's installed.

## Notes
- The launchers are **authoritative** for the model set; `../docs/stack.html`
  must match them.
- The **CUDA (NVIDIA)** launchers are the *private* counterpart, in
  `rr-turbo-build-tools`.
