# Stable Parameters

## Environment

- Workspace: `G:\ComfyUI_windows_portable`
- ComfyUI API: `http://127.0.0.1:8188`
- GPU: `RTX 5070 Ti 16GB`
- Runtime flags:
  - `--lowvram`
  - `--fp8_e4m3fn-unet`
  - `--fp8_e4m3fn-text-enc`
  - `--use-split-cross-attention`

## Core Local Models

- Diffusion model: `Wan2_1-I2V-14B-480P_fp8_e4m3fn.safetensors`
- LoRA: `lightx2v_I2V_14B_480p_cfg_step_distill_rank128_bf16.safetensors`
- Wan VAE: `wan\wan_2.1_vae.safetensors`
- CLIP vision: `clip_vision_h.safetensors`
- Image model for seed frame: `z_image_turbo_bf16.safetensors`
- Wan text bridge path: `wan\umt5_xxl_fp8_e4m3fn_scaled.safetensors` via `CLIPLoader(type=wan)` + `WanVideoTextEmbedBridge`

## Template Files

- Short template: `references/api_short_template.json`
- Long template: `references/api_long_template.json`

Replace these fields before submitting:
- positive image prompt
- positive video prompt
- seed values
- filename prefix
- optional fps, frame count, and strengths within the safe ranges below

## Safe Short Preset

Use for prettier quick iterations.

- Resolution: `832x480`
- Frames: `33`
- FPS: `16`
- Wan steps: `4`
- CFG: `1.0`
- Shift: `5.0`
- Scheduler: `dpm++_sde`
- Noise aug: about `0.03` to `0.035`
- Start latent strength: about `0.97` to `0.98`
- End latent strength: about `0.96` to `0.97`
- Block swap: `10`
- Output: upscale successful result to `1920x1080`

## Safe Long Preset

Use for roughly 4 second clips without repeating the known crashy 49-frame setup.

- Resolution: `832x480`
- Frames: `41`
- FPS: `10`
- Wan steps: `4`
- CFG: `1.0`
- Shift: `5.0`
- Scheduler: `dpm++_sde`
- Noise aug: about `0.028` to `0.03`
- Start latent strength: about `0.98`
- End latent strength: about `0.96`
- Block swap: `10`
- Output: upscale successful result to `1920x1080`

## Risky Preset

Avoid as a default.

- `49` frames at `832x480`
- This machine has already crashed or dropped the ComfyUI service on that workload during long rendering.

## Upscale Command Pattern

Use ffmpeg after a successful 480p render:

```powershell
ffmpeg -y -i INPUT.mp4 -vf "scale=1920:1080:force_original_aspect_ratio=increase:flags=lanczos,crop=1920:1080" -c:v h264_nvenc -preset p5 -cq 18 -pix_fmt yuv420p -an OUTPUT_1080p.mp4
```

## Proven Output Prefixes

- `codex_wash_feet_cinematic_480p`
- `codex_wash_feet_beauty_480p`
- `codex_wash_feet_bathroom_safe_480p`

Use new prefixes per variation to avoid confusion.
