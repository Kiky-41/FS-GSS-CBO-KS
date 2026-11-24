# FS-GSS-CBO-KS

**A Robust Self-Adaptive Hybrid Framework for the Dynamic Economic Load Dispatch Problem**

Martha Yustika, Lindiasari · Rahman Nur Ikhsan, Rifki · Raharjo, Jangkung  
*JOIV, 10(3), 1307–1315, 2026*  
[![DOI](https://img.shields.io/badge/DOI-10.62527%2Fjoiv.10.3.3814-blue)](https://doi.org/10.62527/joiv.10.3.3814)

Fibonacci Search + Golden Section Search + Clone-Based Optimization + Koch Snowflake perturbation for dynamic economic load dispatch.

## Abstract

The Economic Load Dispatch (ELD) problem involves determining the most cost-effective allocation of power generation while satisfying system constraints. To address its complexity and improve performance, this study proposes a hybrid optimization framework named FS-GSS-CBO-KS. This method combines Fibonacci Search (FS) for global exploration, Golden Section Search (GSS) for fast local convergence, Clone-Based Optimization (CBO) for iterative refinement, and Koch Snowflake (KS) perturbations to maintain diversity and avoid local optima. Compared to established algorithms such as Grey Wolf Optimizer (GWO), Particle Swarm Optimization (PSO), Artificial Bee Colony (ABC), Differential Search (DS), and Whale Optimization Algorithm (WOA), FS-GSS-CBO-KS achieves the lowest total operational cost of approximately IDR 39.05 billion and the shortest computation time of 0.49 seconds. The ABC algorithm, in contrast, resulted in a 16.9% cost increase and required more than 5.6 seconds to compute. The proposed hybrid method demonstrates superior economic efficiency, fast convergence, and robust adaptability, making it a promising tool for large-scale ELD applications in dynamic power systems. Future research could expand the FS-GSS-CBO-KS framework to multi-objective Economic Load Dispatch (ELD), addressing not only fuel cost but also objectives such as emission reduction, system reliability, and power loss minimization. Including these would boost its relevance to modern power systems that prioritize both economic and environmental goals. Adapting the algorithm for renewable systems and distributed generation (DG) could enhance its use amid uncertainty and variability. Assessing impacts on power quality metrics, such as voltage stability and harmonic distortion, would broaden its operational utility. Conducting real-time tests in large-scale, practical scenarios would validate its robustness, scalability, and effectiveness.

## Contents

```
.gitignore
FS-GSS-CBO-KS.ipynb
results/
  Result.xlsx
  sensitivity_summary.csv
```

## Data

Input data is **not included**. Place files in `data/` (notebooks) or next to the `.m` script (MATLAB):

| File | Columns |
|---|---|
| `input42u.xlsx` | Sheet1: `Unit`, `a`, `b`, `c`, `pmin`, `pmax`, `rampup`, `rampdown`, `load` |
| `power_demand42u.xlsx` | Sheet1: `load` |

## Requirements

Python. Python notebooks: `numpy`, `pandas`, `scipy`, `matplotlib` (as imported). MATLAB scripts: R2021a+.

## Citation

```bibtex
@article{Martha_Yustika_2026, title={A Robust Self-Adaptive Hybrid Framework for the Dynamic Economic Load Dispatch Problem}, volume={10}, ISSN={2549-9610}, url={http://dx.doi.org/10.62527/joiv.10.3.3814}, DOI={10.62527/joiv.10.3.3814}, number={3}, journal={JOIV : International Journal on Informatics Visualization}, publisher={Society of Visual Informatics}, author={Martha Yustika, Lindiasari and Rahman Nur Ikhsan, Rifki and Raharjo, Jangkung}, year={2026}, month=May, pages={1307} }
```

## License

[![CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](LICENSE)

Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). **Attribution is required**: any use of this code, results, or figures, including in publications, must cite:

> Lindiasari Martha Yustika; Rifki Rahman Nur Ikhsan; Jangkung Raharjo (2026). A Robust Self-Adaptive Hybrid Framework for the Dynamic Economic Load Dispatch Problem. *JOIV : International Journal on Informatics Visualization*. https://doi.org/10.62527/joiv.10.3.3814
