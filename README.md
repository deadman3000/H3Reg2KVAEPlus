# Open-H3 Regenerate — LoRA and Curve Compatibility Update

**Status: Alpha candidate 0.1 — Experimental**

This document describes an experimental modification of the Open-H3 Regenerate node pack for technical review. It is intended for review alongside the modified source and test workflows. It is not an official release or a replacement for the original implementation.

## 1. Overview

This modification explores an alternative approach to model LoRA handling and LMS Sharp curve configuration within the Open-H3 Regenerate workflow.

The primary objective is to allow the model to pass through the standard ComfyUI LoRA application route while handling the regeneration curve branches separately.

This provides a potential workaround for using alternative model LoRAs without depending exclusively on the specialist regeneration node's bundled LoRA handling.

The work also incorporates the experimental fast H3 VAE decoder into the regeneration workflow.

## 2. Current status

Two model configurations have successfully completed end-to-end renders using the modified workflow.

| Model | Result | Notes |
|---|---|---|
| Singularity | PASS | Successful render; refinement appeared to apply correctly; no console warnings. |
| Hybrid | PASS | Successful render using the modified workflow. |

These results establish an encouraging initial compatibility baseline. They do not yet demonstrate compatibility across every H3 model variant, LoRA, resolution or hardware configuration.

### Confirmed functionality

- Standard ComfyUI LoRA handling successfully passes the Singularity model through the modified workflow.
- LMS Sharp curve branches are handled separately from the model LoRA path.
- Spatial upscaling completes successfully.
- Subsequent refinement completes successfully.
- Experimental fast H3 VAE decoding completes successfully.
- Singularity output shows the intended refinement behaviour.
- Hybrid produces a successful output.
- The successful Singularity run completed without console warnings.

## 3. What has changed

### 3.1 LoRA and curve handling

The experimental modification separates two responsibilities:

**Model and LoRA path**

The model is passed through the standard ComfyUI LoRA handling route rather than relying exclusively on the specialist regeneration node's internal LoRA handling.

**Regeneration curve path**

The LMS Sharp curve branches are handled separately by the modified regeneration-node logic.

The intention is to retain the required regeneration curve behaviour while allowing greater flexibility in model and LoRA selection.

The modified implementation is contained in the node pack's `__init__.py`, particularly the `OpenH3RegenLora` class and its registration in `NODE_CLASS_MAPPINGS`.

The implementation should be reviewed directly in the source rather than treating this description as a substitute for code-level verification.

### 3.2 Experimental fast H3 VAE decoder

The workflow incorporates the experimental `OpenH3RegenFastVAEDecode` node.

The decoder is based on a StreamRender-H3-derived implementation and is designed to decode H3 video latents without conventional spatial tiling.

The node also provides an optional connected H3 VAE fallback for situations where the fast decoder cannot fit into available GPU memory or encounters an out-of-memory condition.

The decoder's source documentation describes it as slightly lossy relative to the released decoder.

Output quality, memory consumption and fallback behaviour should therefore be evaluated for each target configuration.

## 4. Workflow configuration

The following points are important when reproducing the successful tests:

- Use a workflow containing the experimental fast decoder node when testing that implementation.
- Ensure the model follows the intended model/LoRA connection path.
- Configure the LMS Sharp curve branches for the selected model and workflow.
- Preserve the working spatial-upscale and refinement settings when comparing model compatibility.
- Avoid changing multiple variables simultaneously when testing another model.
- Verify the final decoded output rather than relying solely on sampler completion or intermediate previews.

### Initial test configuration

The successful Singularity test included:

| Setting | Value |
|---|---|
| Input resolution | 480 × 672 |
| Output resolution | 960 × 1344 |
| Spatial scaling | 2× |
| Spatial split | 2 × 2 |
| Tile size | 736 × 736 |
| Overlap | 128 px |
| Fade | 64 px |

These are recorded test values, not universal recommendations.

## 5. Compatibility testing

Singularity and Hybrid have both passed initial end-to-end testing.

Further testing is required before wider compatibility can be claimed.

Suggested additional tests:

| Test | Purpose | Status |
|---|---|---|
| Singularity | Baseline model compatibility | PASS |
| Hybrid | Alternative model compatibility | PASS |
| Original H3 / FL2VA | Additional architecture compatibility | Pending |
| Alternative LoRAs | Check standard LoRA handling across variants | Pending |
| Different LoRA strengths | Check behaviour at different strengths | Pending |
| Different resolutions | Check spatial processing and decoder behaviour | Pending |
| Longer clips | Check memory scaling and stability | Pending |
| Audio/video workflows | Check any intended AV compatibility | Pending |

Compatibility should only be claimed for configurations that have actually been tested.

## 6. Installation and update considerations

This is currently an experimental source modification rather than a packaged release.

Before installing or updating:

1. Back up the existing `ComfyUI-Open-H3-Regenerate` folder.
2. Back up the workflow being used for testing.
3. Apply the matching modified source files together.
4. Confirm that the experimental decoder's supporting Python modules and required model files are present.
5. Restart ComfyUI after modifying Python files.
6. Load the matching workflow and verify that the intended modified nodes are present.
7. Perform a short test before attempting a longer or higher-resolution render.

**Important:** Do not replace the modified `__init__.py` with an upstream copy without first preserving the experimental LoRA and curve modifications.

Likewise, do not assume an older workflow contains the newer experimental decoder node.

## 7. Console output and logging

During alpha testing, verbose diagnostic output should remain available.

Reducing console output is considered a later packaging and usability improvement, not a priority during compatibility testing.

A proposed final logging arrangement would provide:

**Normal mode**
- Important processing milestones.
- Progress information.
- Warnings and errors.
- Final output and memory statistics.

**Debug mode**
- Detailed model loading information.
- Tensor dimensions and latent information.
- Memory staging and allocation details.
- Detailed spatial processing and decoder diagnostics.

The objective is to reduce unnecessary console noise without sacrificing useful troubleshooting information.

## 8. Known limitations

- Compatibility has currently been demonstrated with Singularity and Hybrid only.
- Other model variants and LoRA combinations remain untested.
- Fast decoder memory requirements increase with clip length and latent resolution.
- The decoder is documented as slightly lossy relative to the released decoder.
- The modified LoRA and curve-handling implementation requires further source-level review.
- Broad regression testing has not yet been completed.
- Logging improvements and final packaging remain outstanding.

## 9. Suggested technical review checklist

For review of this modification:

- [ ] Inspect the `OpenH3RegenLora` implementation.
- [ ] Verify node registration and input/output definitions.
- [ ] Confirm that
