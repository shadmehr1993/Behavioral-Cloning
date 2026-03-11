# Behavioral-Cloning
## Behavioral Cloning Agent for HVAC Control

This repository provides a simplified implementation of a **Behavioral Cloning (BC) agent** for supervisory HVAC control in district energy systems.
The goal of the BC agent is to learn a control policy by **imitating expert behavior** obtained from a baseline controller. In this project, expert trajectories are generated using a **rule-based control (RBC) strategy**, which provides state–action pairs representing system operation under different environmental and load conditions.
The BC model is trained using supervised learning to approximate the mapping between system states and control actions. The input state vector includes key o
* supply water temperature
* return water temperature
* outdoor temperature
* cooling demand
* electricity price signal
* photovoltaic (PV) generation
The model outputs the control actions applied to the system, including:
* supply water temperature setpoint
* pump mass flow rate
* storage operation commands
By learning from historical expert demonstrations, the BC agent can reproduce stable control behavior while providing a foundation for more advanced learning-based strategies.
⚠️ **Note:**
This repository shares **a partial implementation of the behavioral cloning agent** used in the study. The full experimental framework, including additional controllers and the complete co-simulation environment, is part of the research project described in the associated publication.
The shared implementation is intended for **educational purposes and reproducibility of the learning methodology**, rather than full replication of the entire district simulation environment.


<img width="454" height="622" alt="image" src="https://github.com/user-attachments/assets/ca601689-18df-481f-a282-66a304eaf83f" />

