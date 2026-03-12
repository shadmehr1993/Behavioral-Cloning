# Behavioral Cloning Agent for HVAC Control in District Energy Systems

<div align="center">

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=flat-square&logo=python)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0%2B-EE4C2C?style=flat-square&logo=pytorch)
![Supervised Learning](https://img.shields.io/badge/Method-Behavioral%20Cloning-purple?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-HVAC%20Control-orange?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

*Imitation learning for energy-efficient supervisory HVAC control in district buildings*

</div>

---

## Overview

This repository provides a modular implementation of a **Behavioral Cloning (BC) agent** for supervisory HVAC control in district energy systems.

The BC agent learns a control policy by **imitating expert behavior** from a **Rule-Based Controller (RBC)**, which generates state–action demonstration pairs under varying environmental and load conditions. A neural network is then trained via supervised learning to approximate the expert's mapping from system states to control actions.

The result is a data-driven controller that **reproduces stable, expert-level behavior** while serving as a strong foundation for more advanced learning strategies such as Reinforcement Learning or Transfer Learning.
<img width="461" height="631" alt="image" src="https://github.com/user-attachments/assets/a7321de7-fd3e-48ca-938d-d58954b0dccd" />

> ⚠️ **Note:** This repository shares a **partial implementation** of the BC agent used in the study. The full experimental framework, including additional controllers and the complete co-simulation environment, is part of the associated research project. The shared code is intended for **educational purposes and reproducibility of the learning methodology**.

---

## Key Features

| Feature | Description |
|---|---|
| 🎓 **Imitation Learning** | Learns directly from RBC expert demonstrations |
| 🏗️ **Modular Pipeline** | Separate scripts for data collection, training, and testing |
| 🌡️ **HVAC-Specific** | Designed for district-scale heat pump cooling systems |
| ⚡ **Energy-Aware** | Inputs include PV generation and electricity price signals |
| 🔁 **Extensible** | Easily adaptable as a warm-start for RL or Transfer Learning |

---

## System Description

The BC agent controls a **district-scale HVAC system** composed of heat pumps, thermal storage, and PV generation. The controller operates at a supervisory level, determining setpoints and operational commands at each timestep.

```
Outdoor Temp ──┐
Cooling Demand ─┤
Electricity Price─┤──→ [ BC Neural Network ] ──→ SWT Setpoint
PV Generation ──┤                                 Flow Rate
Supply Temp ────┤                                 Storage Command
Return Temp ────┘
```

### Input State Vector

| Variable | Description |
|---|---|
| Supply Water Temperature (SWT) | Current supply temperature from heat pump |
| Return Water Temperature (RWT) | Return temperature from building loop |
| Outdoor Temperature | Ambient air temperature |
| Cooling Demand | Building thermal load requirement |
| Electricity Price Signal | Time-of-use tariff for cost-aware control |
| PV Generation | Available photovoltaic power output |

### Output Action Vector

| Variable | Description |
|---|---|
| SWT Setpoint | Target supply water temperature |
| Pump Mass Flow Rate | Hydraulic flow rate command |
| Storage Operation | Thermal storage charge / discharge command |

---

## Project Structure

```
Behavioral-Cloning/
│
├── collect_rbc_data/           # Step 1: Generate expert demonstrations
│   └── ...                     # Run RBC and collect state-action pairs
│
├── train_BC_network/           # Step 2: Train the BC neural network
│   └── ...                     # Supervised learning on expert data
│
├── bc_controller/              # Step 3: BC agent inference module
│   └── ...                     # Trained policy for deployment
│
├── test_BC_controller/         # Step 4: Evaluate the trained BC agent
│   └── ...                     # Run evaluation and compute KPIs
│
├── .gitignore
├── LICENSE
└── README.md
```

---

## Pipeline

The workflow follows four sequential steps:

```
Step 1          Step 2              Step 3           Step 4
────────        ────────────        ──────────       ──────────────
Run RBC    →    Train Neural   →    Deploy BC   →    Evaluate &
Collect         Network on          Agent            Compare vs
Expert Data     Demonstrations                       RBC Baseline
```

### Step 1 — Collect Expert Data
```bash
python collect_rbc_data/run_collection.py
```
Runs the Rule-Based Controller on the environment and saves state–action pairs to CSV.

### Step 2 — Train the BC Network
```bash
python train_BC_network/train.py
```
Trains a fully connected neural network on the collected demonstrations using supervised learning (MSE loss).

### Step 3 — Run the BC Controller
```bash
python bc_controller/run.py
```
Loads the trained model and applies it as a controller in the simulation environment.

### Step 4 — Test and Evaluate
```bash
python test_BC_controller/evaluate.py
```
Runs a full evaluation episode and computes performance metrics against the RBC baseline.

---

## Installation

```bash
# Clone the repository
git clone https://github.com/shadmehr1993/Behavioral-Cloning.git
cd Behavioral-Cloning

# Create a virtual environment
python -m venv venv
source venv/bin/activate       # Linux / macOS
venv\Scripts\activate          # Windows

# Install dependencies
pip install -r requirements.txt
```

---

## Methodology

### Why Behavioral Cloning?

Behavioral Cloning is an **imitation learning** technique where a neural network learns to replicate an expert's decisions from observed demonstrations. In the context of HVAC control:

- **No environment interaction needed** during training (unlike RL)
- **Fast convergence** — supervised learning is stable and sample-efficient
- **Expert knowledge encoded** — the RBC baseline encodes domain expertise
- **Warm-start for RL** — BC policies can initialize RL agents to accelerate learning

### Training Objective

The BC network minimizes the mean squared error between predicted and expert actions:

```
L(θ) = (1/N) Σ || π_θ(s_i) - a_i^expert ||²
```

where `s_i` is the state, `a_i^expert` is the RBC action, and `π_θ` is the learned policy.

---

## Results

The BC agent successfully replicates key RBC behaviors including:

- ✅ Temperature setpoint tracking within operating bounds
- ✅ Demand-responsive flow rate adjustment
- ✅ PV-aware storage operation
- ✅ Stable control without oscillations

> For full quantitative results, KPI comparisons, and simulation details, refer to the associated research publication.

---

## Relation to Other Work

This repository is part of a broader research project on **data-driven HVAC control** that includes:

| Repository | Method | Description |
|---|---|---|
| **Behavioral-Cloning** (this repo) | Imitation Learning | Learn from RBC expert demonstrations |
| [TL-hpc-cooling](https://github.com/shadmehr1993/TL-hpc-cooling) | Transfer Learning + DRL | Adapt pre-trained agents to new buildings |

---

## Citation

If you use this work in your research, please cite:

```bibtex
@misc{zaregarizi2024bc-hvac,
  author    = {Shadmehr Zaregarizi},
  title     = {Behavioral Cloning Agent for HVAC Control in District Energy Systems},
  year      = {2024},
  publisher = {GitHub},
  url       = {https://github.com/shadmehr1993/Behavioral-Cloning}
}
```

---

## Author

**Shadmehr Zaregarizi**
Politecnico di Torino
📧 shadmehr.zaregarizi@studenti.polito.it
🔗 [github.com/shadmehr1993](https://github.com/shadmehr1993)

---

<div align="center">
⭐ If you find this project useful, please consider giving it a star!
</div>
