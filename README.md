# Experimental LoRA and LMS Sharp Curve Compatibility Modification — Singularity and Hybrid Tested

Hello,

I've been working on an experimental modification to the Open-H3 Regenerate node pack and would like to share the results with the community for technical review.

The work started with an attempt to use alternative H3 LoRA configurations with the regeneration workflow, without relying entirely on the specialist regeneration node's bundled LoRA handling.

## What has been changed

The main modification separates model LoRA handling from regeneration curve handling:

- The model is passed through the standard ComfyUI LoRA application route.
- LMS Sharp curve branches are handled separately by the modified regeneration-node implementation.
- The intention is to retain the regeneration curve behaviour while allowing alternative LoRA configurations to be used.

The workflow also incorporates the experimental fast H3 VAE decoder.

## Testing results

Two model configurations have now completed successful end-to-end renders:

**Singularity**
- Successful model pass-through.
- LMS Sharp curve handling operational.
- Spatial upscaling completed.
- Refinement appeared to apply correctly.
- Fast H3 VAE decoding completed.
- No console warnings during the successful run.

**Hybrid**
- Successful end-to-end render.
- Refinement and final decoding completed successfully.

These are genuine workflow renders rather than simply successful node-loading or sampler tests.

## Current development status

I'd consider this an experimental Alpha 0.1 candidate.

Further testing is required with additional model variants, LoRA combinations, resolutions and clip lengths. I haven't yet established universal compatibility.

There are also opportunities for further refinement of the implementation and eventual packaging.

## What I'd particularly appreciate

I'd welcome feedback from the original node-pack developer and community on:

1. Whether this alternative LoRA/curve handling approach is technically sound.
2. Whether the modifications could be incorporated into the existing node pack.
3. Whether there are any architectural or compatibility issues I should investigate before expanding testing.
4. Whether the experimental fast decoder integration is appropriate for this workflow.

I'm happy to provide the modified `__init__.py`, workflow and updated README for inspection.

My intention is to contribute a useful compatibility improvement to the existing project, while keeping the original implementation and attribution intact.

Thanks for the work on the original node pack, and I'd be interested to hear your thoughts.
