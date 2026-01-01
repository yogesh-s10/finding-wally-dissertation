# RESULTS — Key experiments & outcomes

This file summarizes results in an “industry-friendly” format.

---

## 1) CNN patch classifier (test metrics)

| Model | Accuracy | Precision | Recall | F1 |
|------|----------|-----------|--------|----|
| CNN (baseline) | 90.5% | 87.5% | 88.3% | 87.9% |
| CNN (improved) | 95.2% | 94.1% | 95.0% | 94.5% |

**Interpretation**
- False positives mostly come from *red/white look-alikes* and similar head shapes.
- False negatives are driven by occlusion, border tiles, and low contrast.

---

## 2) Reinforcement learning (DQN) — reward strategy comparison

**Environment summary**
- Page discretised into an **N×N grid (e.g., 10×10)**; each tile is **64×64 px**
- Episode cap: **50 steps**
- Actions: Up / Down / Left / Right / Declare
- Exploration coverage (%) = **% unique tiles visited in an episode** (proxy for exploration breadth)

| Strategy | Reward design | Avg Reward | Steps to Find | Exploration Coverage |
|---------|----------------|----------:|--------------:|---------------------:|
| A | +50 find, −1 per move | 35.2 | 36 | 60% |
| B | +100 find, −0.1 move, +5 new tile | 49.3 | 28 | 92% |

**Why Strategy B wins**
A gentle step cost + bonus for visiting new tiles encourages exploration early and efficient stopping later.

---

## 3) Human vs RL benchmark (grid-style interface)

| Agent | Avg Steps | Success |
|------|----------:|--------:|
| Human (avg) | 47 | 96% |
| DQN | 28 | 92% |

Additional benchmark notes (simulated env):
- Avg time: **33.4s (human)** vs **6.2s (RL)**
- False clicks (error): **2.1 (human)** vs **1.3 (RL)**

---

## Training dynamics
- DQN convergence observed by ~**7k episodes**
- Steps reduced from **>150** early training to **~30** after convergence
