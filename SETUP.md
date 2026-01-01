# SETUP — How to run this repo (Colab-first)

This project is notebook-first (`2431303.ipynb`).

## Option A (recommended): Google Colab

1) Upload/open `2431303.ipynb` in Colab  
2) Install dependencies in a cell:

```bash
pip install -U numpy pandas matplotlib pillow tensorflow gymnasium
```

Optional (only if you use model diagram cells):
```bash
pip install -U pydot graphviz
```

3) Mount Google Drive (if your dataset is in Drive) and update dataset paths  
4) Run cells top-to-bottom

---

## Option B: Local (venv)

### 1) Create & activate venv
```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate
```

### 2) Install requirements
```bash
pip install -U numpy pandas matplotlib pillow tensorflow gymnasium
```

Optional:
```bash
pip install -U pydot graphviz
```

### 3) Run Jupyter
```bash
pip install -U jupyter
jupyter notebook
```

Open `2431303.ipynb` and run cells top-to-bottom.

---

## Common issues

### Dataset paths
If you get “file not found”, it’s almost always a path mismatch.  
Check `DATA.md` for the expected folder layout and update the notebook paths.

### Graphviz / pydot errors
Only needed for model visualisation. If it blocks you, skip those cells.

### Reproducibility tips
- keep train/test split **before** augmentation (avoid leakage)
- fix seeds (NumPy + TensorFlow) if you’re comparing runs
- record versions (`pip freeze > requirements-freeze.txt`)
