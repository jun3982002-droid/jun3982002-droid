# Large MoE Inference on a 64 GB Mac

I investigate how far large open-weight models can run on a 64 GB Apple Silicon Mac when most routed expert weights live on SSD.

My current case study is **GLM-5.3 Full, IQ2_XXS quantization**, running through an experimental fork based on [DS4](https://github.com/antirez/ds4). I run controlled measurements and am preparing a reproducibility pack with the setup, code changes, and trade-offs—including cases where a speed idea does not help.

## Current research

- Two-SSD streaming on an M4 Max with 64 GB unified memory
- Exact decoding and explicitly approximate expert-substitution modes
- Measurements of prefill, decode, storage limits, and output quality
- Next: test whether a third NVMe drive or a different storage path changes real inference speed

## A measured snapshot

On a fixed Japanese prompt with 64 generated tokens (two runs per mode, 2026-09-27), `exact` averaged **1.955 tok/s** and approximate `fast4s` averaged **5.555 tok/s**—a 2.84× ratio of those two-run means. `fast4s` uses resident-expert substitution, so this is a speed/quality trade-off, not a lossless speedup. In a separate evaluation, its NLL was 5.98% higher on one 991-token text (959 scored); NLL is not a general quality score.

**The extra drive is an experiment, not a promised upgrade.** A third drive could raise the storage ceiling. Shared connections, scheduling, or other runtime costs may still limit the end-to-end gain, so I will measure it rather than predict it.

→ [Read the benchmark notes and experiment plan](https://github.com/jun3982002-droid/glm53-64gb-mac)

→ **[Support the experiments through GitHub Sponsors](https://github.com/sponsors/jun3982002-droid)**

Help make the next hardware comparison reproducible: one added SSD, a documented setup, and public results others can build on. The result may show no speedup; it will be reported either way.
