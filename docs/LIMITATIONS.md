# Limitations and defensible research extensions

## Current limitations
- Unseeded splits and separately randomized evaluation introduce potential sample overlap.
- No documented patient-/slide-disjoint partition, external cohort, or out-of-distribution evaluation.
- Training augmentation for ResNet is accidentally overridden by shared transform assignment.
- DARTS-style alpha parameters are trained jointly with weights, not in canonical bilevel optimization.
- Reported ResNet numbers conflict between documentation and saved artifacts.
- DARTS vs ResNet differences include pretraining, resolution and optimization; no controlled comparison.
- No calibration, uncertainty, failure-case or subgroup analysis; not a clinical system.
- Existing visualization of genotype may be hardcoded.

## Potential future research (not completed work)
1. Audit dataset provenance and build patient-disjoint fixed splits with stored hashes/manifests.
2. Correct training/validation transforms and evaluate on untouched external test data.
3. Establish fair ResNet and compact-CNN baselines under matched computational budgets.
4. Implement a true bilevel DARTS protocol, select and retrain the discrete architecture, then measure search cost.
5. Repeat experiments across seeds; report uncertainty, per-class recall, calibration and inference cost on named hardware.
6. Evaluate robustness to staining, microscope differences and domain shift.

These are proposed experiments only, not claims about work already performed.
