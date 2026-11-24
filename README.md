# FS-GSS-CBO-KS

**A Robust Self-Adaptive Hybrid Framework for the Dynamic Economic Load Dispatch Problem**

Martha Yustika, Lindiasari · Rahman Nur Ikhsan, Rifki · Raharjo, Jangkung  
*JOIV, 10(3), 1307–1315, 2026*  
[![DOI](https://img.shields.io/badge/DOI-10.62527%2Fjoiv.10.3.3814-blue)](https://doi.org/10.62527/joiv.10.3.3814)

Fibonacci Search + Golden Section Search + Clone-Based Optimization + Koch Snowflake perturbation for dynamic economic load dispatch.

## Abstract

The Economic Load Dispatch (ELD) problem involves determining the most cost-effective allocation of power generation while satisfying system constraints. To address its complexity and improve performance, this study proposes a hybrid optimization framework named FS-GSS-CBO-KS. This method combines Fibonacci Search (FS) for global exploration, Golden Section Search (GSS) for fast local convergence, Clone-Based Optimization (CBO) for iterative refinement, and Koch Snowflake (KS) perturbations to maintain diversity and avoid local optima. Compared to established algorithms such as Grey Wolf Optimizer (GWO), Particle Swarm Optimization (PSO), Artificial Bee Colony (ABC), Differential Search (DS), and Whale Optimization Algorithm (WOA), FS-GSS-CBO-KS achieves the lowest total operational cost of approximately IDR 39.05 billion and the shortest computation time of 0.49 seconds. The ABC algorithm, in contrast, resulted in a 16.9% cost increase and required more than 5.6 seconds to compute. The proposed hybrid method demonstrates superior economic efficiency, fast convergence, and robust adaptability, making it a promising tool for large-scale ELD applications in dynamic power systems. Future research could expand the FS-GSS-CBO-KS framework to multi-objective Economic Load Dispatch (ELD), addressing not only fuel cost but also objectives such as emission reduction, system reliability, and power loss minimization. Including these would boost its relevance to modern power systems that prioritize both economic and environmental goals. Adapting the algorithm for renewable systems and distributed generation (DG) could enhance its use amid uncertainty and variability. Assessing impacts on power quality metrics, such as voltage stability and harmonic distortion, would broaden its operational utility. Conducting real-time tests in large-scale, practical scenarios would validate its robustness, scalability, and effectiveness.


## Proposed method

FS-GSS-CBO-KS is a hyper-heuristic for economic load dispatch. Each unit's coefficient triple $(a_i,b_i,c_i)$ is perturbed with Koch-snowflake steps; the perturbed coefficients define a one-dimensional cost function in the unit's output; that function is minimised by Fibonacci search over wide intervals and golden-section search over narrow ones. Clones of the coefficient vector are evaluated and only improving clones survive. The method is tested on the 42-unit Jamali system and compared with GWO, PSO, ABC, DS and WOA.

### Objective

$$\min F(\mathbf{P})=\sum_{i=1}^{N}\left(a_iP_i^2+b_iP_i+c_i\right)+\lambda\left|P_D-\sum_iP_i\right|,\qquad \lambda=10^{6},$$

subject to $P_i^{\min}\le P_i\le P_i^{\max}$ and the ramp limits between consecutive loads. The notebook divides the cost by 1000, so its printed values are in thousands of rupiah; the published total is IDR 39.05 billion.

### Notation

| Symbol | Meaning | Unit |
|---|---|---|
| $N$ | number of units | – |
| $P_i$ | output of unit $i$ | MW |
| $a_i,b_i,c_i$ | cost coefficients (quadratic, linear, constant) | – |
| $\lambda$ | balance penalty factor | – |
| $P_D$ | demand | MW |
| $F_n$ | $n$-th Fibonacci number | – |
| $\varphi$ | golden ratio $(1+\sqrt5)/2$ | – |
| $[\ell,u]$ | search interval for one unit | MW |
| $x_1,x_2$ | interior evaluation points | MW |
| $\varepsilon$ | Koch perturbation scale | – |
| $f_x^{n},f_y^{n}$ | random coordinates on the Koch curve | – |
| $a_i',b_i',c_i'$ | perturbed (cloned) coefficients | – |

### Fibonacci search

Choose the smallest $n$ such that the Fibonacci number covers the interval at the requested tolerance:

$$F_n\ge\frac{u-\ell}{\text{tol}},\qquad F_n=F_{n-1}+F_{n-2},\quad F_0=0,~F_1=1.$$

Evaluate the objective at the two interior points:

$$x_1=\ell+\frac{F_{n-2}}{F_n}(u-\ell),\qquad x_2=\ell+\frac{F_{n-1}}{F_n}(u-\ell).$$

The worse half is discarded:

$$\text{if } f(x_1)\lt f(x_2):~u\leftarrow x_2;\qquad \text{if } f(x_1)\ge f(x_2):~\ell\leftarrow x_1.$$

The final estimate is $x^{\ast}\approx(\ell+u)/2$. The Fibonacci ratios are the discrete version of the golden ratio, so each evaluation reuses one previous point.

### Golden-section search

For narrow intervals the same logic uses the golden ratio directly:

$$\varphi=\frac{1+\sqrt5}{2},\qquad x_1=u-\frac{u-\ell}{\varphi},\qquad x_2=\ell+\frac{u-\ell}{\varphi},$$

with the same shrinking rule and final estimate $(\ell+u)/2$. The golden-section iteration is cheaper per step because the ratio stays constant.

### Hyper-heuristic switch

$$\text{use Fibonacci if } u-\ell\ge50,\qquad \text{use golden section if } u-\ell\lt50.$$

The switch lets the method use Fibonacci's stronger shrinkage on wide intervals and golden section's cheaper refinement on narrow ones.

### Koch-snowflake perturbation

For a segment from $(x_1,y_1)$ to $(x_2,y_2)$, the one-third, two-third and apex points are

$$x_3=\frac{2x_1+x_2}{3},\quad y_3=\frac{2y_1+y_2}{3},\qquad x_4=\frac{x_1+2x_2}{3},\quad y_4=\frac{y_1+2y_2}{3},$$

$$x_5=\frac{x_3+x_4}{2}-\frac{\sqrt3}{6}(y_4-y_3),\qquad y_5=\frac{y_3+y_4}{2}+\frac{\sqrt3}{6}(x_4-x_3).$$

The apex completes an equilateral triangle on the middle third, which is the Koch construction. Random points $f_x^{n},f_y^{n}$ on the level-3 curve give the coefficient perturbations

$$\Delta a_i=\varepsilon f_x^{n},\qquad \Delta b_i=\varepsilon f_y^{n},\qquad \Delta c_i=\varepsilon\left(f_x^{n}+f_y^{n}\right),$$

with scale $\varepsilon=0.1$. The fractal walk is structured but non-repeating, so clones differ in a controlled way.

### Clone-based refinement

$$\mathbf{a}'=\mathbf{a}+\Delta\mathbf{a},\qquad \mathbf{b}'=\mathbf{b}+\Delta\mathbf{b},\qquad \mathbf{c}'=\mathbf{c}+\Delta\mathbf{c}.$$

Every clone is dispatched by FS/GSS and scored; the lowest-cost clone is retained for the next iteration, otherwise the incumbent parameters survive.

### Reported result

42-unit Jamali system: IDR 39.05 billion at 0.49 s, against 16.9% higher cost and over 5.6 s for ABC.

### Implementation notes

The notebook applies the hybrid search per unit with a scalar surrogate, `cost(x \ast ones)`, which repeats one unit's output across all units. It also has an earlier clone loop that calls the hybrid search with vector bounds; that branch returns the mean of the per-unit results, not a dispatch vector. The published results in `Result.xlsx` therefore cannot be reproduced by the notebook as committed. The method description above follows the paper and the intended vectorised implementation.

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


## Results

### 42-unit Jamali, 24 hours, best of 30 runs per hour

| Hour | FS-GSS-CBO-KS | GWO | WOA | PSO | ABC | DS | FS time (s) |
|---|---|---|---|---|---|---|---|
| 1 | 1,415,373,318.15 | 1,574,527,339.04 | 1,527,869,513.27 | 1,538,435,912.22 | 1,804,067,394.00 | 1,649,406,997.21 | 0.0194 |
| 2 | 1,408,098,017.82 | 1,577,570,835.60 | 1,542,386,637.72 | 1,522,775,730.03 | 1,718,733,812.00 | 1,578,369,171.05 | 0.0198 |
| 3 | 1,406,973,762.19 | 1,585,653,806.33 | 1,522,450,767.79 | 1,910,573,047.54 | 1,830,675,196.00 | 1,615,405,778.50 | 0.0199 |
| 4 | 1,406,830,643.65 | 1,531,401,918.95 | 1,488,429,011.29 | 1,575,093,663.19 | 1,782,416,930.00 | 1,539,160,854.52 | 0.0194 |
| 5 | 1,399,270,622.65 | 1,546,914,842.39 | 1,502,150,893.08 | 1,623,888,612.07 | 1,648,814,379.00 | 1,577,547,540.08 | 0.0195 |
| 6 | 1,398,578,456.26 | 1,607,802,581.50 | 1,477,850,347.57 | 1,534,243,531.92 | 1,661,808,051.00 | 1,674,159,650.05 | 0.0194 |
| 7 | 1,406,866,793.57 | 1,575,990,795.58 | 1,504,710,151.03 | 1,581,294,562.10 | 1,814,148,833.00 | 1,575,213,992.40 | 0.0198 |
| 8 | 1,409,115,753.64 | 1,571,311,327.20 | 1,538,489,364.68 | 1,542,280,127.69 | 1,734,170,874.00 | 1,566,769,552.53 | 0.0199 |
| 9 | 1,415,484,226.06 | 1,608,852,240.20 | 1,583,762,625.71 | 1,632,052,655.15 | 1,796,490,083.00 | 1,600,967,974.75 | 0.0236 |
| 10 | 1,435,191,157.08 | 1,673,788,549.06 | 1,601,302,580.83 | 1,631,240,329.37 | 1,826,525,843.00 | 1,696,608,072.41 | 0.0220 |
| 11 | 1,415,467,814.11 | 1,621,797,060.24 | 1,585,239,152.33 | 1,556,974,549.12 | 1,824,537,791.00 | 1,584,893,266.06 | 0.0206 |
| 12 | 1,415,473,971.45 | 1,621,833,666.89 | 1,552,379,199.64 | 1,736,332,474.42 | 1,856,237,536.00 | 1,601,240,715.63 | 0.0197 |
| 13 | 1,431,142,754.07 | 1,637,313,226.79 | 1,605,435,286.47 | 1,570,243,417.40 | 1,786,266,200.00 | 1,617,556,164.13 | 0.0195 |
| 14 | 1,546,601,731.05 | 1,769,155,241.38 | 1,712,635,113.47 | 1,698,810,504.86 | 1,963,614,153.00 | 1,723,241,595.34 | 0.0200 |
| 15 | 1,652,120,004.72 | 1,874,468,301.54 | 1,850,551,781.70 | 1,780,532,045.45 | 2,079,088,799.00 | 1,775,431,215.26 | 0.0299 |
| 16 | 1,848,791,123.72 | 2,066,153,407.81 | 1,978,097,617.56 | 2,010,028,345.46 | 2,096,601,149.00 | 1,907,691,146.01 | 0.0202 |
| 17 | 1,935,211,883.58 | 2,065,635,134.36 | 2,051,135,872.81 | 2,066,128,555.41 | 2,168,652,530.00 | 2,004,701,779.57 | 0.0202 |
| 18 | 1,991,570,781.75 | 2,230,046,390.09 | 2,088,539,179.08 | 2,034,409,700.06 | 2,266,790,525.00 | 2,040,153,619.20 | 0.0203 |
| 19 | 2,049,777,214.40 | 2,262,672,635.85 | 2,139,539,079.85 | 2,196,541,572.79 | 2,327,017,925.00 | 2,127,671,259.86 | 0.0196 |
| 20 | 2,111,063,652.71 | 2,324,335,198.92 | 2,192,159,460.78 | 2,280,259,109.68 | 2,332,581,633.00 | 2,197,195,787.90 | 0.0199 |
| 21 | 2,117,572,718.26 | 2,283,365,624.31 | 2,199,153,334.06 | 2,243,791,186.38 | 2,377,363,705.00 | 2,184,996,027.89 | 0.0202 |
| 22 | 2,085,184,893.89 | 2,346,858,989.79 | 2,154,043,059.56 | 2,189,727,582.12 | 2,370,504,787.00 | 2,132,090,453.30 | 0.0197 |
| 23 | 1,938,222,924.91 | 2,114,778,683.43 | 1,971,111,729.20 | 1,992,778,717.92 | 2,194,060,602.00 | 2,040,634,416.45 | 0.0194 |

### Result workbooks

| File | Content |
|---|---|
| [`results/Result.xlsx`](results/Result.xlsx) | raw output |

The workbooks hold the raw solver output behind the tables above: per-unit schedules, per-hour costs, constraint checks and run statistics.


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
