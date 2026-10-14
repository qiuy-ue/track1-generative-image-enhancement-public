# Method Summary

## FreqOrtho-SR

The first stage operates at quarter resolution and focuses on stable structural recovery. Frequency-domain degradation features guide pixel-level LoRA experts. Orthogonal gradient projection reduces interference between pixel fidelity and semantic adaptation.

## TADSR

The second stage uses a time-aware one-step diffusion refinement path. A teacher-guided student model restores high-frequency detail and returns the output to native 4K geometry. The time step is adjusted for difficult scenes.

## Candidate selection

The pipeline compares candidate outputs across the full test set and applies an explicit utility rule. This keeps the final selection auditable instead of relying on an opaque ranking model.

## End-to-end stages

1. Prepare the low-quality input at quarter resolution.
2. Run FreqOrtho-SR structure recovery.
3. Run TADSR high-resolution refinement.
4. Compare candidate outputs with the available quality measures.
5. Package the selected outputs with the required filenames and resolution.
