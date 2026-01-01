# DATA — Dataset structure & preparation

This project trains a **patch-based CNN** to classify tiles as **Wally** vs **Not Wally**, then uses those signals inside a **custom Gymnasium grid environment** for RL.

---

## Data sources (as used in the dissertation)
- Patch dataset: **Hey-Wally** (Wally / not-Wally patches at multiple sizes)
- Additional positives: manually curated **high-quality Wally face patches** from high-res pages (helps class imbalance + improves signal quality)

> ⚠️ Licensing note: If the dataset isn’t redistributable, do **not** commit raw images to GitHub.  
> Instead, share a download link + instructions and keep your repo dataset-free.

---

## Recommended folder structure

Keep data outside git (or use `.gitignore`):

```
data/
  raw/
    hey-wally/
      wally/
      not_wally/
  processed/
    64x64_rgb/
      train/
        wally/
        not_wally/
      test/
        wally/
        not_wally/
```

If your notebook uses different names, either update the paths in the notebook or rename folders to match.

---

## Preparation steps (high level)

### 1) Standardisation
- Resize to **64×64**
- Keep **RGB** (colour cues help vs grayscale in cluttered scenes)

### 2) Train/Test split
- Split **before** augmentation (prevents leakage)
- Balanced sets (as per experiments):
  - Train ≈ **4,000 Wally / 4,000 not-Wally**
  - Test  ≈ **1,000 Wally / 1,000 not-Wally**

### 3) Augmentation (positives only)
Used to increase positive diversity while preserving recognisable cues:
- flips
- small zoom
- brightness changes
- small translations
- **no rotation** (keeps orientation cues stable)

---

## Data quality pitfalls
- class imbalance (too many “not-Wally” tiles)
- leakage (augmenting before split)
- near-duplicate patches inflating test metrics if not separated cleanly
