# Simulation-Based Study of Physical Sensor Attack Hardness in Robotic Vehicles
A safe, simulation-based reproduction and extension of RVPROBER (USENIX Security 2024), analyzing physical sensor attack hardness in robotic vehicles using abstract sensor models and prerequisite-driven evaluation.

This repository presents a safe, simulation-based reproduction and extension of the methodology from the USENIX Security 2024 paper: A Systematic Study of Physical Sensor Attack Hardness
Hyungsub Kim, Rwitam Bandyopadhyay, Muslum Ozgur Ozmen, Z. Berkay Celik, Antonio Bianchi, Yongdae Kim, Dongyan Xu.

The goal of this project is to faithfully reconstruct the analytical framework and experimental reasoning of the original work — particularly the RVPROBER pipeline — while avoiding real-world attack instructions and using purely abstract, non-actionable simulation models.

This repository demonstrates my ability to:

Reconstruct a complex systems-security methodology from a top-tier paper

Design reproducible simulation workflows

Implement sensor-level models, attack abstractions, and statistical analyses

Produce publication-quality figures suitable for IEEE / NeurIPS / USENIX venues

Reason carefully about ethics, safety, and responsible disclosure.

#Project Motivation:
Physical sensor attacks on robotic vehicles (RVs) are often dismissed as impractical due to presumed hardware redundancy or environmental constraints. The referenced paper challenges this assumption by introducing attack hardness: a systematic way to quantify how often attacks are feasible in practice by analyzing their prerequisites, parameter sensitivity, and real-world configuration prevalence.

This project reproduces that reasoning in a fully synthetic, simulation-only environment and is intended for:

Academic understanding

Methodology validation

Safe experimentation

Future defensive research

No real-world exploit parameters, hardware details, or procedural guidance are included.
# High-Level Methodology:
The project follows the same logical structure as the original paper:

##Step 1 — Attack Prerequisite Analysis

For each sensor attack category, we identify:

Required sensors (e.g., GNSS, IMU, magnetometer, optical flow)

Vehicle types and operating modes

Environmental constraints

These prerequisites are encoded as boolean predicates over synthetic RV configurations.

##Step 2 — Prerequisite Frequency Estimation

We generate a synthetic population of RV users (simulated flight logs) and estimate:

How often each prerequisite occurs

What fraction of RV configurations are vulnerable to each attack

This mirrors the paper’s large-scale log analysis (30k+ real flights), but using benign synthetic data only.

##Step 3 — Abstract Attack Simulation

We implement non-actionable sensor perturbation models that simulate:

GPS spoofing/jamming effects

IMU acoustic-style disturbances

Magnetic interference

Optical-flow manipulation

Bus-level corruption

Each attack is controlled by a normalized intensity parameter ∈ [0,1], representing attack capability without encoding real-world instructions.

##Step 4 — RVPROBER-Style Evaluation

We perform parameter sweeps and observe:

When attacks succeed (mission deviation / instability)

How satisfying additional prerequisites increases success probability

This reproduces the paper’s key finding:

satisfying prerequisites increases successful attacks (e.g., from 6 to 11).

#Key Figures and What They Show:
Figure 1 — Vulnerable RV Users per Attack
fig_vulnerability_bar.png

This bar chart visualizes attack hardness by showing the percentage of RV configurations that satisfy all prerequisites for each sensor attack category.

Interpretation:
Even without tuning parameters, a large fraction of RV users are structurally vulnerable — consistent with the paper’s ~57% average vulnerability finding.

Figure 2 — Attack Success vs. Attack Intensity

fig_success_vs_intensity.png

This plot shows attack success probability as a function of abstract attack intensity.

Interpretation:
Attacks transition from ineffective to reliable only after specific thresholds, illustrating why naïve demonstrations underestimate feasibility without systematic parameter exploration.

Figure 3 — Impact of Satisfying Prerequisites (6 → 11)

fig_6_to_11.png

This figure shows the number of successful attacks:

Without enforcing prerequisites

After satisfying identified prerequisites

Interpretation:
This reproduces the key insight of the original paper: most failures are due to missing prerequisites, not inherent infeasibility.

Figure 4 — Distribution of Attack Prerequisites

fig_prereq_hist.png

This histogram shows how many prerequisites each attack requires.

Interpretation:
Sensor attacks are multi-condition events. Ignoring prerequisite structure leads to incorrect security conclusions.

Figure 5 — Gyroscope Measurements (High-Resolution)

fig_gyro_measurements.png

These plots show:

True angular rate

Noisy gyroscope measurements

Smoothed estimates

Across baseline and attack scenarios.

Interpretation:
Abstract IMU disturbances cause structured deviations that propagate through control estimation pipelines.

Figure 6 — Roll Angle and Altitude Trajectories

fig_roll_and_altitude.pdf

Top row: roll angle estimates
Bottom row: altitude estimates (barometer, GPS, fused)

Interpretation:
Sensor perturbations introduce divergence between redundant estimators, highlighting multi-sensor consistency as a defense signal.

This figure is exported as vector PDF for direct inclusion in academic papers.
--

# Reproducibility

All simulations are deterministic (fixed RNG seeds)

No external hardware or private data required

Runs fully in Google Colab (CPU-only)

Unit tests validate dynamics, sensors, and attack logic

To run:

pip install -r requirements.txt

Then open the notebooks in order:

01_rvprober_analysis.ipynb

02_simulation_and_attacks.ipynb
--

# Ethics, Safety, and Responsible Disclosure

This repository:

Does not include real-world exploit instructions

Uses abstract parameters instead of physical attack details

Is intended for defensive research and methodology analysis

See ethics_and_limitations.md for:

IRB-style checklist

Disclosure recommendations

Limitations of simulation-based studies
--
 #Author Note

This project was developed independently as a research-focused reconstruction of prior work, with the goal of demonstrating:

Technical depth in systems and sensor security

Faithful methodology reproduction

Safe and ethical experimentation

Readiness to contribute to advanced research in this area

I would be honored to further extend this work — particularly toward defenses, detection mechanisms, and real-world validation under appropriate ethical review — in collaboration with Professor Kim’s research group.

Citation:

If you refer to this repository, please cite the original paper:

@inproceedings{celik2024sensor,
  title={A Systematic Study of Physical Sensor Attack Hardness},
  author={Kim, Hyungsub and Bandyopadhyay, Rwitam and Ozmen, Muslum Ozgur and Celik, Z. Berkay and Bianchi, Antonio and Kim, Yongdae and Xu, Dongyan},
  booktitle={USENIX Security Symposium},
  year={2024}
}
