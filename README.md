# AVERT

**Adversarial Vehicle Evasion via Reinforcement Training**

A three-stage curriculum framework for training highway driving agents to evade adversarial traffic using on-policy actor-critic methods (A2C and PPO).

---

## Overview

AVERT trains an ego vehicle in `highway-env` through progressively hostile traffic conditions:

- **Stage 1** -- 30 passive IDM vehicles, collision penalty -2
- **Stage 2** -- 40 aggressive vehicles with unsafe lane changes, collision penalty -3
- **Stage 3** -- 30 custom `PursuerVehicle` agents that actively track and intercept the ego, collision penalty -5

Each stage fine-tunes from the previous checkpoint. The curriculum agent reaches 90% of peak Stage 3 performance in 5 episodes vs. 107 for a scratch baseline -- a 21.4x sample efficiency advantage under PPO.

---

## Results

| Algorithm | Condition | Mean Reward | Survival |
|-----------|-----------|-------------|----------|
| A2C | Curriculum | 18.35 +/- 0.14 | 100% |
| A2C | Scratch | 15.24 +/- 5.81 | 50% |
| PPO | Curriculum | 18.35 +/- 0.14 | 100% |
| PPO | Scratch | 18.35 +/- 0.14 | 100% |

Transfer to `merge-v0` and `roundabout-v0` shows modest curriculum advantages (+1.17, +0.94), more pronounced on merge due to structural overlap with highway driving skills.

---

## Setup

```bash
pip install -r requirements.txt
```

**Requirements:** `stable-baselines3`, `highway-env`, `gymnasium`, `numpy`, `matplotlib`

---

## Usage

```bash
# Train curriculum (A2C or PPO)
python train.py --algo ppo --mode curriculum

# Train from scratch
python train.py --algo ppo --mode scratch

# Evaluate a checkpoint
python evaluate.py --checkpoint checkpoints/ppo_stage3.zip --episodes 10

# Transfer fine-tuning
python transfer.py --env roundabout-v0 --checkpoint checkpoints/ppo_stage3.zip
```

---

## Project Structure

```
avert/
  envs/
    pursuer_vehicle.py     # Custom PursuerVehicle adversary
    stage_configs.py       # Per-stage environment configurations
  agents/
    train.py               # Curriculum and scratch training
    evaluate.py            # Deterministic rollout evaluation
    transfer.py            # Fine-tuning on new environments
  checkpoints/             # Saved model weights per stage
  figures/                 # Training curves and result plots
requirements.txt
README.md
```

---

## Citation

Course project for CS 5756: Robot Learning, Cornell University.

```
@misc{avert2025,
  title  = {AVERT: Adversarial Vehicle Evasion via Reinforcement Training},
  author = {Suvarna, Kalyan and Tyagi, Uday},
  year   = {2025},
  note   = {CS 5756 Robot Learning, Cornell University}
}
```
