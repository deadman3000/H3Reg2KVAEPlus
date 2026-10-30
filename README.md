# Open-H3 Regenerate — Extended LoRA and Curve Support

<badge>Experimental Alpha 0.1</badge>

An experimental extension to the Open-H3 Regenerate ComfyUI node pack, introducing an alternative approach to model LoRA handling and LMS Sharp regeneration curve configuration.

The objective is to improve compatibility with alternative H3 model and LoRA configurations while retaining the original regeneration workflow.

## Overview

The original regeneration workflow uses specialist LoRA handling within its regeneration node.

This modification introduces a separate node for handling the model and regeneration curve paths, allowing the model to pass through standard ComfyUI LoRA application nodes while the LMS Sharp curve branches are handled independently.

This provides greater flexibility when working with alternative H3 model configurations and LoRA conversions.

Two model configurations have successfully completed end-to-end testing:

- Singularity
- Hybrid

Both have produced successful refined outputs using the modified workflow.

## Changes and additions

### 1. New LoRA and curve handling node

A new node, `OpenH3RegenLora`, has been introduced into the node pack.

Its purpose is to separate model LoRA application from regeneration curve handling.

The modifications allow:

- The base model to pass through standard ComfyUI LoRA nodes.
- Alternative LoRA conversions to be applied through the normal ComfyUI model path.
- LMS Sharp curve branches to be configured and handled independently.
- The regeneration workflow to retain its intended curve behaviour without depending exclusively on the original specialist LoRA-loading route.

This is the principal modification in this experimental release.

### 2. LMS Sharp curve handling

The modified implementation allows the regeneration curve branches to be handled separately from the model LoRA application.

This is particularly relevant when working with alternative H3 LoRA conversions and model configurations that do not necessarily use the original bundled LoRA arrangement.

The intention is to preserve the original regeneration behaviour while allowing more flexible model and LoRA combinations.

### 3. Experimental fast H3 VAE decoder support

The workflow also supports the experimental `OpenH3RegenFastVAEDecode` node.

This provides an alternative decoding route based on the StreamRender-H3 fast decoder implementation.

The decoder is designed to process H3 video latents without conventional spatial tiling and includes an optional connected H3 VAE fallback for situations where the fast decoder cannot complete within available GPU memory.

The decoder implementation is experimental and documents a degree of quality loss relative to the released decoder.

Its memory requirements increase with clip length and latent resolution.

**Note:** The fast decoder is a separate experimental component of the node pack, not the principal LoRA/curve modification introduced here.

## Compatibility testing

The following configurations have successfully completed end-to-end rendering.

| Model | Status | Result |
|---|---|---|
| Singularity | PASS | Successful render and refinement |
| Hybrid | PASS | Successful render and refinement |

### Singularity test

The workflow successfully completed:

- Model pass-through using the modified LoRA route.
- LMS Sharp curve handling.
- Spatial upscaling.
- Subsequent refinement.
- Final fast H3 VAE decoding.

The resulting render appeared to show the intended refinement behaviour. No console warnings were reported during the successful run.

### Hybrid test

Hybrid also completed the modified workflow successfully, producing a satisfactory final output.

These results demonstrate that the approach is not limited to the initial Singularity configuration.

### Further testing required

The following areas remain to be investigated:

- Original H3 / FL2VA model compatibility.
- Additional model variants.
- Alternative LoRA conversions and combinations.
- Different LoRA strengths.
- Different output resolutions and aspect ratios.
- Longer video sequences.
- VRAM requirements and decoder fallback behaviour.
- Broader regression testing.

Compatibility should not be assumed for configurations that have not been tested.

## Workflow considerations

When using the modified workflow:

1. Connect the base model through the intended model/LoRA route.
2. Apply the desired LoRA using standard ComfyUI LoRA nodes.
3. Configure the regeneration curve branches for the selected model.
4. Connect the regeneration workflow as intended.
5. Configure spatial upscaling and refinement.
6. Use the experimental fast decoder where appropriate.

When testing different models, retain the same workflow and settings wherever possible, changing only the model-specific components required. This helps isolate genuine compatibility differences.

## Development status

**Alpha 0.1 — Experimental**

The current implementation has passed initial end-to-end testing with Singularity and Hybrid.

The principal LoRA and curve-handling modifications are operational in these configurations.

Further model compatibility testing and source review are required before considering a more general release.

### Planned improvements

- Additional model compatibility testing.
- Wider LoRA conversion support.
- Further validation of curve handling.
- Decoder memory and stability testing.
- Cleaner console output for normal operation.
- Retention of detailed diagnostic logging through an optional debug mode.
- Documentation and workflow packaging improvements.

Console-output reduction is deliberately deferred until the implementation is more mature, so useful diagnostic information remains available during development.

## Credits and acknowledgements

This project builds upon the existing Open-H3 Regenerate node pack and its underlying H3 regeneration components.

Credit and attribution should be retained for the original node-pack authors, underlying model and decoder implementations, and the authors of any LoRA conversions used.

This repository contains experimental modifications and compatibility work. It is not an official release of the original node pack or its upstream dependencies.

Please refer to the respective upstream repositories and their licences for original implementation details and usage conditions.

## Feedback and contributions

Technical feedback, compatibility reports and suggestions for further improvements are welcome.

In particular, feedback on the alternative LoRA application route, LMS Sharp curve handling and compatibility with additional H3 model configurations would be appreciated.

---

**Open-H3 Regenerate — Extended LoRA and Curve Support**

*Experimental Alpha 0.1*

*Singularity and Hybrid successfully tested.*
