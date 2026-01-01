# MODEL_CARD — Finding Wally Patch Classifier + DQN Search Agent

This model card describes two parts:

1) **CNN patch classifier** (64×64 tile → probability of Wally)
2) **DQN search agent** (moves on a grid + Declare)

---

## Intended use
- Research prototype for **visual search** using a hybrid CNN + RL approach
- Demonstrates model iteration, evaluation, and end-to-end system design

## Not intended for
- Real-world surveillance, face recognition, or identification of real individuals
- Safety-critical decision making

---

## Model overview

### CNN (patch classifier)
- Input: 64×64 RGB tile
- Output: binary class (Wally / Not Wally) or probability score
- Training: Binary Cross-Entropy, Adam (lr=0.001), early stopping + checkpoints

### DQN agent (Gymnasium env)
- Environment: each page is tiled into an **N×N grid (e.g., 10×10)**, **64×64 px per cell**
- State: current tile (RGB) or CNN feature/score (higher-level cues)
- Actions (5): Up, Down, Left, Right, Declare
- Loop-avoidance: environment tracks visited tiles (reduces wasted revisits)
- Episode cap: **50 steps** maximum (ends earlier on Declare)

---

## Performance (headline)

### CNN (test)
- Baseline CNN:
  - Accuracy **90.5%**
  - Precision **87.5%**
  - Recall **88.3%**
  - F1 **87.9%**
- Improved CNN:
  - Accuracy **95.2%**
  - Precision **94.1%**
  - Recall **95.0%**
  - F1 **94.5%**

### RL (DQN)
- Best reward strategy: **92% success**, **28 steps average**, **92% exploration coverage**
- Human benchmark: **96% success**, **47 steps average**

---

## Limitations
- Discretised grid search can miss targets near tile borders
- Error propagation: CNN false negatives can reduce RL success
- Limited scene semantics vs human context use (humans skip sky/empty ground and focus crowds)

---

## Ethical considerations
- This is a cartoon/illustration domain project.
- Do not repurpose to identifying real people or sensitive contexts.
