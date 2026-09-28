# Large MoE Inference on a 64 GB Mac

How far can a 64 GB Apple Silicon Mac run a model whose weights are much larger than memory?

I measure SSD-streamed inference for **GLM-5.3 Full, IQ2_XXS quantization** (196.58 GiB model file) on an M4 Max with 64 GB unified memory. The experimental runtime is based on [DS4](https://github.com/antirez/ds4). I publish speed and quality trade-offs, including results that do not support a speedup claim.

## A measured speed/quality trade-off

In a two-run conversation-prompt test on 2026-09-28, exact decoding measured about **2.1 tok/s**. The experimental approximate mode `jevq6s` measured **3.60 tok/s**, about **1.7×** the exact result. Its NLL was **+2.06%** and **+2.03%** versus exact on two Japanese texts (959 and 2,222 tokens scored). This is a small evaluation, not a general quality guarantee.

An earlier, faster approximate mode, `fast4s`, measured **5.555 tok/s** versus **1.955 tok/s** exact on a different fixed-prompt test. In a separate one-text evaluation, its NLL was **5.98% higher** than exact. These are distinct tests; NLL is one language-modeling metric, not a general quality score.

The full project notes compare six named modes, from `exact` through `fast5`. `jevq6s` is the current quality-priority starting point in the limited tests; `fast5` reached the highest measured speed but is not recommended as the default because of its larger NLL difference. The experimental launcher can select modes by name, but its code is still being prepared for public release.

A later test on 2026-09-28 found one reproducible case where `jevq6s` answered with unrelated text; whether it happened depended on the earlier requests. The launcher now runs the first 16 decode steps of each answer without approximation. That fixed the reproduced case, but it lowers speed on short answers (about 15% for `jevq6s` on 64-token answers). The same night's notes also compare `jevq6s` with two other locally run models on small Japanese question sets.

→ [Read benchmark notes, limitations, and the experiment plan](https://github.com/jun3982002-droid/glm53-64gb-mac)

## What sponsorship supports

The next milestone is a controlled comparison of the current internal-plus-external SSD setup with an added third NVMe drive. Support will help cover the test SSD and any required enclosure or cable. A third drive may raise the storage ceiling, but shared connections or other runtime costs may prevent an end-to-end speedup. I will publish the setup and result either way.

**[Support the experiments through GitHub Sponsors](https://github.com/sponsors/jun3982002-droid)**

If this research is useful to you, a one-time or monthly sponsorship helps make the next hardware comparison possible. The benchmark notes and results will remain public; sponsorship does not guarantee a speedup or buy private results. Sharing the project also helps.
