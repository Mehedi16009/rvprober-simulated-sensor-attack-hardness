# Simulation-Based Study of Physical Sensor Attack Hardness in Robotic Vehicles

This repository presents a safe, simulation-based reproduction and extension of the methodology from the USENIX Security 2024 paper: A Systematic Study of Physical Sensor Attack Hardness (Hyungsub Kim, Rwitam Bandyopadhyay, Muslum Ozgur Ozmen, Z. Berkay Celik, Antonio Bianchi, Yongdae Kim, Dongyan Xu).

The goal of this project is to faithfully reconstruct the analytical framework and experimental reasoning of the original work — particularly the RVPROBER pipeline — while avoiding real-world attack instructions and using purely abstract, non-actionable simulation models.

This repository demonstrates my ability to:

Reconstruct a complex systems-security methodology from a top-tier paper

Design reproducible simulation workflows

Implement sensor-level models, attack abstractions, and statistical analyses

Produce publication-quality figures suitable for IEEE / NeurIPS / USENIX venues
Reason carefully about ethics, safety, and responsible disclosure.

--

## Project Motivation:
Physical sensor attacks on robotic vehicles (RVs) are often dismissed as impractical due to presumed hardware redundancy or environmental constraints. The referenced paper challenges this assumption by introducing attack hardness: a systematic way to quantify how often attacks are feasible in practice by analyzing their prerequisites, parameter sensitivity, and real-world configuration prevalence.

This project reproduces that reasoning in a fully synthetic, simulation-only environment and is intended for:

Academic understanding

Methodology validation

Safe experimentation

Future defensive research

No real-world exploit parameters, hardware details, or procedural guidance are included.

--
## High-Level Methodology:
The project follows the same logical structure as the original paper:
```
Step 1 — Attack Prerequisite Analysis
```
For each sensor attack category, we identify:

Required sensors (e.g., GNSS, IMU, magnetometer, optical flow)

Vehicle types and operating modes

Environmental constraints

These prerequisites are encoded as boolean predicates over synthetic RV configurations.
```
--
Step 2 — Prerequisite Frequency Estimation
```
We generate a synthetic population of RV users (simulated flight logs) and estimate:

How often each prerequisite occurs

What fraction of RV configurations are vulnerable to each attack

This mirrors the paper’s large-scale log analysis (30k+ real flights), but using benign synthetic data only.
```
--
Step 3 — Abstract Attack Simulation
```
We implement non-actionable sensor perturbation models that simulate:

GPS spoofing/jamming effects

IMU acoustic-style disturbances

Magnetic interference

Optical-flow manipulation

Bus-level corruption

Each attack is controlled by a normalized intensity parameter ∈ [0,1], representing attack capability without encoding real-world instructions.
```

--
Step 4 — RVPROBER-Style Evaluation
```
We perform parameter sweeps and observe:

When attacks succeed (mission deviation / instability)

How satisfying additional prerequisites increases success probability

This reproduces the paper’s key finding:

satisfying prerequisites increases successful attacks (e.g., from 6 to 11).

```
--
```

## Key Figures and What They Show:
Figure 1 — Vulnerable RV Users per Attack

<img width="600" height="700" alt="fig_vulnerability_bar (1)" src="https://github.com/user-attachments/assets/690f2ce3-c52f-4b08-bcad-e8f323ffb01e" />


This bar chart visualizes attack hardness by showing the percentage of RV configurations that satisfy all prerequisites for each sensor attack category.

Interpretation:
Even without tuning parameters, a large fraction of RV users are structurally vulnerable — consistent with the paper’s ~57% average vulnerability finding.
--
Figure 2 — Attack Success vs. Attack Intensity

<img width="600" height="700" alt="fig_success_vs_intensity (1)" src="https://github.com/user-attachments/assets/1e3ba626-5b24-4ebd-9550-273790028c56" />


This plot shows attack success probability as a function of abstract attack intensity.

Interpretation:
Attacks transition from ineffective to reliable only after specific thresholds, illustrating why naïve demonstrations underestimate feasibility without systematic parameter exploration.
--
Figure 3 — Impact of Satisfying Prerequisites (6 → 11)

<img width="600" height="700" alt="fig_6_to_11" src="https://github.com/user-attachments/assets/2939556c-92e7-438b-9760-fe7a5486b252" />


This figure shows the number of successful attacks:

Without enforcing prerequisites

After satisfying identified prerequisites

Interpretation:
This reproduces the key insight of the original paper: most failures are due to missing prerequisites, not inherent infeasibility.
--
Figure 4 — Distribution of Attack Prerequisites

<img width="600" height="700" alt="fig_prereq_hist" src="https://github.com/user-attachments/assets/4365593a-b0a3-44da-84a0-1183021ef6b7" />


This histogram shows how many prerequisites each attack requires.

Interpretation:
Sensor attacks are multi-condition events. Ignoring prerequisite structure leads to incorrect security conclusions.
--
Figure 5 — Gyroscope Measurements (High-Resolution)

<img width="800" height="1000" alt="fig_gyro_measurements" src="https://github.com/user-attachments/assets/065199db-7a57-4693-bafd-76fcd5c4bcd4" />


These plots show:

True angular rate

Noisy gyroscope measurements

Smoothed estimates

Across baseline and attack scenarios.

Interpretation:
Abstract IMU disturbances cause structured deviations that propagate through control estimation pipelines.
--
Figure 6 — Roll Angle and Altitude Trajectories

<img width="700" height="800" alt="Screenshot 2025-12-09 at 6 20 25 PM" src="https://github.com/user-attachments/assets/7ee6c4c8-4d96-470b-96b4-9b089b4a6275" />



Top row: roll angle estimates
Bottom row: altitude estimates (barometer, GPS, fused)

Interpretation:
Sensor perturbations introduce divergence between redundant estimators, highlighting multi-sensor consistency as a defense signal.

This figure is exported as vector PDF for direct inclusion in academic papers.


--

## Reproducibility

All simulations are deterministic (fixed RNG seeds)

No external hardware or private data required

Runs fully in Google Colab (CPU-only)

Unit tests validate dynamics, sensors, and attack logic

To run:
```
pip install -r requirements.txt
```
Then open the notebooks in order:
```
01_rvprober_analysis.ipynb

02_simulation_and_attacks.ipynb 
--
```
## Ethics, Safety, and Responsible Disclosure

This repository:
- Does **not** include real-world exploit instructions  
- Uses abstract, normalized parameters instead of physical attack details  
- Is intended entirely for defensive research and methodology analysis  

See `ethics_and_limitations.md` for:
- IRB-style checklist  
- Disclosure recommendations  
- Limitations of simulation-based studies  

Designed to run in Google Colab using CPU‐only resources. The code uses fixed random seeds for full reproducibility.



--
## Author Note

This project was developed independently as a **research-focused reconstruction** of prior work. It demonstrates:
- Technical depth in systems and sensor security  
- Faithful methodology reproduction  
- Safe and ethical experimentation  
- Readiness to contribute to advanced research in this area  

I welcome the opportunity to extend this work—particularly toward detection, mitigation, and real-world validation under appropriate ethical review—in collaboration with Professor Kim’s research group.



-------
## 📬 Contact:

Developed by Md Mehedi Hasan Senior Lecturer in Computer Science and Engineering Department at Global Institute of Information Technology (GIIT), Tangail, Dhaka, Bangladesh.

Email: [mehedi.hasan.ict@mbstu.ac.bd](mehedi.hasan.ict@mbstu.ac.bd) | [mehedi.hasan.ict13@gmail.com](mehedi.hasan.ict13@gmail.com)

Phone: +8801789113669 | +8801334110929

Institution: [GIIT University / IdeaVerse / MBSTU]


---
# Original Paper Citation Link:

@inproceedings{kim2024systematic,
  title={A systematic study of physical sensor attack hardness},
  author={Kim, Hyungsub and Bandyopadhyay, Rwitam and Ozmen, Muslum Ozgur and Celik, Z Berkay and Bianchi, Antonio and Kim, Yongdae and Xu, Dongyan},
  booktitle={2024 IEEE Symposium on Security and Privacy (SP)},
  pages={2328--2347},
  year={2024},
  organization={IEEE}
}
