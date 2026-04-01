# Dynamic-Hardware-Aware-NC

**Neural Control Reinforcement Learning for Autonomous Quadcopters**

A framework for training and deploying reinforcement learning-based flight controllers that run entirely on-device — no cloud dependency, no offloading.

---

## Overview

Dynamic-Hardware-Aware-NC (DHANC) develops quadcopter control systems powered by neural networks trained via reinforcement learning. The key constraint driving every design decision: **all inference runs on the quadcopter's own hardware in real time**.

This means the project spans three tightly coupled domains:

- **Control theory** — modeling flight dynamics, stability, and response
- **Reinforcement learning** — training policies that generalize across maneuvers and disturbances
- **Embedded real-time optimization** — fitting neural inference into the memory and compute budget of onboard hardware

---

## Hardware-Aware Design

"Hardware-aware" is a first-class concern, not an afterthought. The system adapts to the specific processor, memory, and power constraints of the target flight controller. This includes:

- Quantization and pruning of neural network weights
- Latency-bounded inference loops compatible with flight control rates (typically 400–1000 Hz)
- Dynamic adaptation to hardware degradation (e.g., motor asymmetry, sensor noise)

---

## Architecture

```
Sensors → State Estimator → Neural Controller → Motor Mixer → ESCs
                 ↑                   ↑
           IMU / Baro         Trained Policy
                                (on-device)
```

The neural controller augments a traditional PID stack. It receives an estimated state (attitude, angular velocity, position) and outputs motor commands, closing the loop entirely onboard.

---

## Goals

- Achieve stable hover and basic navigation using a network small enough to run within a 1ms control cycle on a Cortex-M7 class processor, and go beyond through hand-gesture based control
- Demonstrate robustness to hardware faults without retraining
- Provide a reproducible baseline for on-device neural flight control research

We insipre ourselves from the research paper [Learning to Fly—a Gym Environment with PyBullet Physics for
Reinforcement Learning of Multi-agent Quadcopter Control](https://arxiv.org/abs/2103.02142) by Panerati et al., where we use quadcopter physics and the state space models to our advantage to control quadcopter movement.

