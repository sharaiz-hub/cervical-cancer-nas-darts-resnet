# Cervical Cell Classification | DARTS-style Mixed Operations vs ResNet50



**Status:**** implementation: Sharaiz Shahid;    Research/educational implementation; **not clinically validated**. Historical outputs are **not trustworthy held-out benchmarks**.  

## Research question and scope

This repository explores two image-classification pipelines for five cervical cytology cell categories: a compact differentiable mixed-operation CNN and a pretrained ResNet50 classifier. The goal is to study architecture parameterization and transfer-learning workflows, **not** to claim a new NAS method, superior diagnostic accuracy, or clinical deployment readiness.

> **Interpretation warning:** These are cell-image labels, not patient-level cervical cancer diagnoses. A classifier output must not be used as medical advice.

## Navigation

- [Methods and experimental protocol](docs/METHODOLOGY.md)
- [Historical results and visualization gallery](docs/RESULTS.md)
- [Reproducibility and setup](docs/REPRODUCIBILITY.md)
- [Limitations and future research](docs/LIMITATIONS.md)
- [Attribution and maintainer contributions](MAINTAINERS.md)
- [Release and permission checklist](GITHUB_RELEASE_CHECKLIST.md)

## Dataset

The source code expects an `ImageFolder` directory at `data/raw/cervicalCancer/`, with five class folders: `im_Dyskeratotic`, `im_Koilocytotic`, `im_Metaplastic`, `im_Parabasal`, and `im_Superficial-Intermediate`. The scripts refer to the [SIPaKMeD cervical cytology dataset](https://www.kaggle.com/datasets/prahladmehandiratta/cervical-cancer-largest-dataset-sipakmed). Dataset files are **not included**; verify original dataset licensing and grouping before use. The archive does not establish a patient-disjoint evaluation protocol.

## Implementation summary

| Property | DARTS-style mixed-operation CNN | ResNet50 baseline |
|---|---|---|
| Implementation | `src/nas_darts_model.py`, `src/nas_train_darts.py` | `src/model_utils.py`, `src/train_resnet.py` |
| Input | RGB 64×64 | RGB 224×224 |
| Initialization | Custom network, learned weights | Torchvision pretrained ResNet50 by default |
| Architecture | Stem, three softmax-weighted mixed-operation cells, global pooling, linear head | ResNet50 backbone with five-class output layer |
| Optimization | Adam, 30 epochs, LR 0.003; model weights and architecture parameters optimized together | Adam, 30 epochs, LR 1e-4; CUDA required by configuration |
| Evaluation | Random subset regenerated in evaluation script | Training-script validation evaluation; shared transform object issue |

**Important:** The mixed-operation training loop is **not canonical bilevel DARTS**; it does not independently optimize architecture parameters on a validation objective or retrain a discretized final genotype. Thus the title uses *DARTS-style*, not *DARTS-discovered optimized architecture*. The two pipelines are not controlled for input resolution, pretraining, or optimization budget.

## Existing results (historical, unverified)

The bundled `outputs/results/metrics.txt` records **ResNet accuracy 0.9741, F1 0.9751**. The bundled DARTS classification report records **accuracy approximately 0.81 and macro F1 approximately 0.81 on 1,003 samples**. These are **historical artifact values only**, not independently reproduced results and **not valid comparative test-set evidence**. The original README reported a different ResNet accuracy (78.43%); this conflict remains unresolved. See [results provenance](docs/RESULTS.md).

## Installation and execution

Requires Python and PyTorch-compatible environment; GPU is needed for the unmodified ResNet training configuration. From the repository root:

```bash
python -m venv .venv
# Linux/macOS: source .venv/bin/activate
# Windows: .venv\Scripts\activate
pip install -r requirements.txt
# Populate data/raw/cervicalCancer/<class_name>/ with properly licensed images
python src/nas_train_darts.py
python src/train_resnet.py
```

Commands are **documented from code, not verified end-to-end** in this maintenance pass. Training overwrites generated outputs; preserve originals before running. `src/evaluate_darts.py` does not use a persistent held-out split. Do not treat its output as test performance. See [reproducibility notes](docs/REPRODUCIBILITY.md).

Optional web UI: run backend dependencies from `Web_UI/backend/requirements.txt`, start `python main.py` from that directory, and run `npm install && npm run dev` from `Web_UI/frontend/`. These instructions are based on code inspection, not a successful runtime test. Do not expose the API publicly without security hardening.

## Repository map

```text
src/                  Training, architectures, data loading, evaluation scripts
models/               Bundled historical checkpoint artifacts
outputs/results/      Historical figures, reports and genotype outputs
Web_UI/backend/       FastAPI inference service and a DARTS checkpoint
Web_UI/frontend/      React/Vite user interface
 docs/                Research-method and provenance documentation
MAINTAINERS.md        Original author and maintainer roles
GITHUB_RELEASE_CHECKLIST.md  Permission and publication gates
```

## Maintainer Contributions: Sharaiz Shahid



## Limitations and next steps

Randomized, unsaved split indices, potential training/evaluation overlap, shared augmentation transforms, and the absence of patient-level controls prevent a defensible generalization claim. Future work should create group-disjoint reproducible splits, a clean external test set, controlled baselines, repeated-seed experiments, calibrated confidence estimates, and a genuine bilevel NAS study if warranted. See [limitations](docs/LIMITATIONS.md).

## Attribution and permissions


