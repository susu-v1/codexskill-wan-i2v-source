---
name: wan-i2v
description: Stable ComfyUI Wan image-to-video workflow for this Windows desktop with an RTX 5070 Ti 16GB GPU. Use when generating or iterating local Wan 2.1 I2V clips in G:\ComfyUI_windows_portable, especially for safe parameter selection, crash avoidance, reusable API templates, and 480p-to-1080p upscale workflow.
---

# ComfyUI Wan I2V Safe

Use this skill to keep local ComfyUI video generation stable on this machine.

## Follow This Workflow

1. Verify `http://127.0.0.1:8188/system_stats` is online before submitting any job.
2. Use the local `Wan 2.1 I2V 14B 480P` workflow family, not larger native 1080p generation.
3. Copy the closest template from `references/api_short_template.json` or `references/api_long_template.json`.
4. Replace prompts, seeds, and filename prefix.
5. Generate at 480p first.
6. Upscale to 1080p only after a successful render.
7. Prefer the safe presets in `references/parameters.md`.
8. If the same service-level failure repeats, stop changing prompts and reduce workload instead.

## Use The Safe Presets

Read [references/parameters.md](references/parameters.md) for the proven parameter sets.

Apply these rules:
- Prefer `832x480` generation.
- Prefer `4` sampling steps for Wan long/short safe runs on this machine.
- Use `lightx2v_I2V_14B_480p_cfg_step_distill_rank128_bf16.safetensors` when speed and stability matter.
- Treat `49` frames as risky on this machine.
- Treat `41` frames at `10 fps` as the safe long preset.
- Treat `33` frames at `16 fps` as the safe short preset.

## Template Rules

Use the bundled API templates as the default starting point.

Update at minimum:
- the image prompt
- the video prompt
- the negative prompt if the task changes materially
- all seed values if you want a new variation
- the `filename_prefix`

Keep the graph shape unchanged unless there is a clear reason to alter it.

## Prompting Rules

Keep prompts tasteful and explicit about motion quality.

Include:
- identity stability
- subtle motion
- camera drift if wanted
- lighting style
- hand detail
- non-erotic framing when applicable

Avoid relying on prompt-only fixes for crashes. Crashes here are usually workload-related, not wording-related.

## Output Rules

Generate local output as 480p MP4 first.
Then upscale with ffmpeg to `1920x1080` using Lanczos and center crop.

Use distinct filename prefixes for each variant so outputs are easy to compare.

## Failure Rules

If ComfyUI is offline, start or wait for service recovery before queueing more jobs.
If a long run crashes the service, reduce one of these before retrying:
- frame count
- fps
- motion ambition

Do not keep retrying the same high-risk preset after a confirmed crash.
