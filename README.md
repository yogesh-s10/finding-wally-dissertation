Where’s Wally? — Hybrid CNN + Deep RL Visual Search (Gymnasium + DQN)
A dissertation project that frames “Where’s Wally?” as a sequential visual search problem.  
The pipeline is modular:
CNN (“eyes”): classifies image patches as Wally vs Non‑Wally
DQN (“brain”): learns a policy to navigate a tiled grid of the page and decide when to Declare the target
This repo is intended as a compact, GitHub-friendly summary of the work (instead of the full dissertation).
---
Project goals
Build a patch-based CNN that can detect Wally cues in cluttered scenes
Create a custom Gymnasium environment that simulates scanning a page tile-by-tile
Train a Deep Q-Network (DQN) to search efficiently (fewer tiles / fewer steps)
Compare performance vs random baselines and a human benchmark
---
Method
1) Dataset & preprocessing
Source: Hey‑Wally patch dataset (multiple patch sizes; RGB & grayscale; Wally / not‑Wally folders)
Key issue: severe class imbalance in raw patches (few positives vs many negatives)
Fix: manually curated 58 high-quality Wally face patches from high‑resolution pages
Standardisation: resized/standardised to 64×64 RGB (kept colour cues)
Augmentation (positives only): flip, small zoom, brightness, slight translations (no rotation)
Split before augmentation to avoid leakage; final balanced sets:
Train ≈ 4,000 Wally / 4,000 not‑Wally
Test  ≈ 1,000 Wally / 1,000 not‑Wally
2) CNN patch classifier (TensorFlow/Keras)
Two CNN variants were trained (Binary Cross‑Entropy + Adam, lr=0.001, ~10 epochs, EarlyStopping + checkpoints).
CNN‑1 (baseline)
Accuracy 90.5%, Precision 87.5%, Recall 88.3%, F1 87.9%
CNN‑2 (improved)
Accuracy 95.2%, Precision 94.1%, Recall 95.0%, F1 94.5%
Practical effect: cleaner patch scores → fewer false alarms → stronger signal for RL
3) Custom Gymnasium environment (grid search)
Each Wally page is tiled into an N×N grid (e.g., 10×10), 64×64 px per cell
State: the current patch (or CNN feature/score for the patch)
Actions (5): Up, Down, Left, Right, Declare
Shaped rewards:
small step penalty (time cost)
positive terminal reward for correct Declare
negative reward for false Declare
Episode ends on Declare or step cap (e.g., 50)
4) DQN agent
DQN with experience replay + target network; ε‑greedy exploration decay
Uses CNN features as input to guide navigation decisions
Reward shaping has a big impact on learning stability and efficiency
---
Results (headline)
RL reward strategy comparison
Strategy A: +50 find, −1 per move → 36 steps, 60% coverage, avg reward 35.2
Strategy B: +100 find, −0.1 move, +5 new tile → 28 steps, 92% coverage, avg reward 49.3
Human vs RL benchmark (grid-style interface)
Human (avg): 47 steps, 96% success
RL (DQN): 28 steps, 92% success
Takeaway: humans were slightly more accurate; the RL agent was step-efficient.
---
Repository structure
```
.
├─ 2431303.ipynb                 # Main notebook (CNN + RL pipeline / experiments)
└─ images/
   └─ finding_wally/             # Demo assets used by the notebook
```
---
Quickstart
Requirements
Python 3.10+ recommended
Core libraries used in the project/notebook:
`tensorflow`, `numpy`, `matplotlib`, `Pillow`
`gymnasium` (custom environment)
optional (for model diagrams): `pydot`, `graphviz`
Run
Clone the repo
Install dependencies
```
   pip install tensorflow numpy matplotlib pillow gymnasium pydot graphviz
   ```
Open and run:
`2431303.ipynb`
> Note: If you want full reproducibility, keep the dataset directory structure consistent with the notebook, and update any local paths inside the notebook cells.
---
Limitations & future work
Grid discretisation can cause border misses → hierarchical zoom / finer tiles
Error propagation (CNN FN can force RL miss) → uncertainty-aware policies / joint training
Limited scene semantics vs human context use → add global context models / attention
---
Citation
If you build on this work, please cite the project repository and/or the dissertation:
Yogesh Srinivas (2025) — Where’s Wally? A Deep Reinforcement Learning and CNN‑Based Framework for Visual Search and Human‑AI Comparison.
---
Contact
If you’d like to discuss the approach or extend the environment/model, feel free to reach out via LinkedIn/GitHub.
