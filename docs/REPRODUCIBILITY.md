# Reproducibility and setup notes

## Environment
The root `requirements.txt` lists PyTorch, torchvision, scikit-learn, plotting and numerical libraries but does not pin versions or CUDA runtime. The backend has separate dependencies; frontend uses Vite/React and `package-lock.json`. Exact original training environment and hardware are unknown.

## Dataset placement
Create `data/raw/cervicalCancer/` with one directory per five SIPaKMeD class labels. Acquire data separately and comply with dataset terms. The archive does not contain the dataset or a deterministic manifest. If samples from the same patient/slide occur across folders, implement group-disjoint splitting before reporting performance.

## Documented commands (not runtime validated)
```bash
pip install -r requirements.txt
python src/nas_train_darts.py
python src/train_resnet.py
```
Run from the repository root. ResNet `src/config.py` sets `DEVICE = 'cuda'`, so CPU-only training requires a code change (not made here). The code may download pretrained ResNet weights. Checkpoints in `models/` differ in location from `outputs/models/` paths expected by evaluation scripts.

## Known blockers
1. No dataset included; cannot reproduce training/evaluation as delivered.
2. DARTS evaluation creates a fresh random split; it is not a valid independent test.
3. ResNet training and validation share the same dataset transform object.
4. No pinned seeds, package lock for Python, data manifests or test fixtures.
5. UI backend uses relative paths; startup depends on current working directory.
6. Public deployment needs input validation and CORS/security review.

## Validation status
Static source inspection performed; no claim of end-to-end training, checkpoint inference, frontend build or benchmark reproduction. Preserve original model files before retraining; new runs may overwrite outputs.
