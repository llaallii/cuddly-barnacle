# Panda Gazebo RL – 2‑Day Mini Project

This project demonstrates a complete **Reinforcement Learning pipeline** using Gazebo Jetty and a Franka Panda robot.

---

## Goal
Train a Panda arm to **reach a 3D target point** in simulation using RL.

---

## Tech Stack
- Gazebo Jetty
- ROS 2 (Jazzy recommended)
- Python
- Stable‑Baselines3 (SAC or PPO)
- Gymnasium style environment

---

# 2‑Day Timeline

## Day 1 – Build the Environment

### Phase 1 – Simulation Setup (1–2 hours)
**Milestone:** Panda spawns and joints can be moved via script.
- Spawn Panda in Gazebo
- Add table and target marker
- Write Python script to publish joint commands
- Verify joint states can be read

---

### Phase 2 – Gym Environment Wrapper (2–3 hours)
**Milestone:** Custom Gym environment exists.

Create `PandaReachEnv` with:
- `reset()` → resets robot + random goal
- `step(action)` → sends joint delta commands

**Observations**
- Joint positions (7)
- Joint velocities (7)
- End‑effector XYZ
- Goal XYZ

**Actions**
- 7 joint position deltas

---

### Phase 3 – Reward & Termination (1 hour)
**Milestone:** Environment gives learning signals.

Reward:
- `-distance_to_goal`
- +1 bonus if within 3 cm
- Small action penalty

Done if:
- Success (goal reached)
- Timeout (200 steps)

---

## Day 2 – Train & Evaluate

### Phase 4 – RL Training (3–4 hours)
**Milestone:** Agent can reach target reliably.

- Algorithm: SAC (recommended) or PPO
- Train 200k–1M steps
- Log reward, distance, success rate

---

### Phase 5 – Evaluation (1–2 hours)
**Milestone:** Agent performance measured.

- Run 100 evaluation episodes
- Compute success rate
- Save rollout videos

---

### Phase 6 – Polish & Report (1 hour)
**Milestone:** Clean project & results.

- Save best model
- Create plots
- Write short report

---

## Expected Outcome
After 2 days you will have:
- A working Gazebo RL environment
- A trained Panda reaching agent
- Evaluation plots & videos
- A reusable RL training pipeline

---

## Optional Extensions
- Add cube and train pick‑and‑place
- Add camera observations
- Add domain randomization

---

Happy Simulating 🤖
