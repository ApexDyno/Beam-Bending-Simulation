

---

# Beam Bending Simulation

## Project Overview

This repository provides Python scripts to simulate and analyze beam deflection (bending) under various loading conditions. The simulations focus on two fundamental support types in structural engineering: cantilever beams (fixed at one end) and simply supported beams (pinned at both ends). The scripts use both **linear** (small-deflection) and **nonlinear** (large-deflection) formulations to capture the behavior of slender beams under load. The central goal is to compare the accuracy of the **Euler–Bernoulli beam theory** against a **nonlinear elastic model** and explore how beam stiffness (quantified by flexural rigidity, $EI$) and loading conditions affect deflection outcomes.

The nonlinear formulation uses the curvature-based beam equation:

$$
\frac{d^2v}{dx^2} \Big/ \left(1 + \left(\frac{dv}{dx}\right)^2 \right)^{3/2} = \frac{M(x)}{EI}
$$

This allows accurate modeling when deflections are large and the small-slope assumptions of classical beam theory are no longer valid. Plots are generated using both **Matplotlib** and **Plotly**, providing flexible and interactive visualization options. The code is modular and easy to extend for additional scenarios.

## Introduction to Beam Bending Theory

Beam bending theory is a fundamental aspect of structural mechanics that describes how beams deform under transverse loading. The **Euler–Bernoulli beam theory** assumes small deflections and linear material behavior, leading to the governing equation:

$$
EI \frac{d^2v}{dx^2} = M(x)
$$

where:

* $E$ is the Young’s modulus (material stiffness),
* $I$ is the second moment of area (geometry-related),
* $EI$ is the **flexural rigidity**, and
* $M(x)$ is the bending moment at position $x$.

In practice, this theory is valid only when the slope $dv/dx$ is small (i.e., $\tan \theta \approx \theta$) and the deformed length $ds \approx dx$. If this assumption fails (due to large loads or flexible beams), nonlinear behavior must be considered. In such cases, the full nonlinear curvature equation (as shown above) provides better accuracy by accounting for geometric nonlinearities.

The scripts implemented here include both formulations and allow side-by-side comparisons.

## Boundary Conditions

**Cantilever Beam**:

* At fixed end ($x = 0$): $v(0) = 0$, $v'(0) = 0$
* At free end ($x = L$): shear and moment are zero

**Simply Supported Beam**:

* At ends ($x = 0$, $x = L$): $v(0) = v(L) = 0$
* No bending moment at supports: $v''(0) = v''(L) = 0$

## Cantilever Beam Simulations

The `CantileverBeam(Point_&_Continuous) (1).ipynb` script handles the following cases:

### Code 1 – Point Load (Nonlinear vs Linear)

* A vertical point load is applied at the free end.
* The script computes the deflection using:

  * Linear Euler–Bernoulli model
  * Nonlinear elastic model using the curvature equation
* Numerical results and both **Matplotlib** and **Plotly** plots are generated.
* This highlights when the linear approximation breaks down.

**Output Plot**: 
```bash
For N = 250, L = 1, EI = 2000, P(Force) = -100.0, w(Relaxation Fcator)  = 0.5, init_guess as uniform, dv_dx_at_firstPoint as FD2
```
1. Plot for NR (Point Load) and Euler (Point Load):
   
![Image](https://github.com/user-attachments/assets/80ede695-ba96-4375-a0cc-5ee971231821)

2. Zoomed Image of the above output:

![Image](https://github.com/user-attachments/assets/1112e147-f67f-4518-8a94-ed97401a6440)

---

### Code 2 – EI Flexural Rigidity Error Table

* Studies how varying $EI$ affects deflection accuracy.
* For each $EI$, the script computes tip deflections using both methods.
* Outputs:

  * A table of linear vs. nonlinear deflections
  * Percentage error
  * Plots showing how increasing stiffness improves accuracy

**Example Output Table**: 
```bash
For N = 250, L = 1, EI vary between [20, 30, 40, 50, 100, 500, 1000, 2000], P(Force) = -100.0, w(Relaxation Fcator)  = 0.5, init_guess as uniform, dv_dx_at_firstPoint as FD2
```

| EI (Nm^2) | Deflection (linear) | Deflection (nonlinear) | % Difference |
|-----------|---------------------|-----------------------|---------------|
| 20        | -1.666667           | -1.179111             | -29.253325    |
| 30        | -1.111111           | -1.385655             | 24.708923     |
| 40        | -0.833333           | -1.634777             | 96.173294     |
| 50        | -0.666667           | -1.708163             | 156.224478    |
| 100       | -0.333333           | -0.367194             | 10.158329     |
| 500       | -0.066667           | -0.066895             | 0.343235      |
| 1000      | -0.033333           | -0.033361             | 0.084049      |
| 2000      | -0.016667           | -0.016670             | 0.019456      |

**Output Plot**:

1. Relative Percentage Difference (%) vs Flexural Rigidity (EI):

![Image](https://github.com/user-attachments/assets/dc5a9026-92b1-4a2c-9b60-e69868a6051b)

2. Difference with EI Variation (At the Free End) vs Flexural Rigidity (EI):
   
![Image](https://github.com/user-attachments/assets/4621eef4-3e99-4c4a-9366-3e020c813df5)


---

### Code 3 – Point Load vs Distributed Load

* Applies:

  * Point load at the tip
  * Uniformly Distributed Load (UDL) of equal total force
* Compares deflection shapes and maximum deflection values
* Includes plots using both **Matplotlib** and **Plotly**
* Demonstrates how load distribution affects bending behavior

**Output Plot**:
```bash
For N = 250, L = 1, EI = 2000, P(Force) = -100.0 and W(UCL) = -100.0, w(Relaxation Fcator)  = 0.5, init_guess as uniform, dv_dx_at_firstPoint as FD2
```
1. Plot for NR (Point & Continous Load) and Euler (Point & Continous Load):
   
![Image](https://github.com/user-attachments/assets/27f75de3-2eaf-46af-9e56-42a9a9c9b639)

2. Zoomed image of NR (Continous Load) and Euler (Continous Load):

![Image](https://github.com/user-attachments/assets/00065149-657f-4c86-9b28-bfdf6190cabd)

3. Plot for NR (Point Load) and NR (Continous Load):
   
![Image](https://github.com/user-attachments/assets/ce9058f9-b691-482b-a9ff-467a8b4e1028)

---

### Code 4 – Multiple Load Cases

* Combines point and distributed loads
* Computes resulting moment distribution and total deflection
* Applies superposition principle in linear theory
* Outputs both numeric and graphical results

> **Note:** As in Codes 1, 2, and 3, you can adjust the required variable values in Code 4 to generate your desired plots.  
> No sample output is attached here — feel free to experiment with different loading conditions.


---

## Simply Supported Beam Simulations

The `Simply_Supported_Beam(Point_&_Continuous).ipynb` script covers:

### Code 1 – Mid-Span Point Load

* Applies a single point load at mid-span
* Computes and plots deflection profile
* Peak deflection occurs at center
* Includes interactive **Plotly** and static **Matplotlib** visualizations

**Output Plot**:

```bash
For N = 250, L = 1, EI = 2000, P(Force) = -100.0, w(Relaxation Fcator)  = 0.5, init_guess as uniform, dv_dx_at_firstPoint as FD2
```
1. Plot for NR (Point Load) and Euler (Point Load):
   
![Image](https://github.com/user-attachments/assets/8c47120f-36b6-47e6-bdb0-f3f6fd4d468f)

2. Zoomed image of the above output:
   
![Image](https://github.com/user-attachments/assets/970060a8-6704-427b-bb0a-f613fc1019d9)

---

### Code 2 – Distributed vs Point Load

* Applies Uniform Distributed Load and point load of equal magnitude (You can change the magnitude according to your prefrence)
* Compares resulting deflections and peak values
* Demonstrates differences in moment distribution and deflection shape

**Output Plot**:

```bash
For N = 250, L = 1, EI = 2000, P(Force) = -100.0 and W(UCL) = -100.0, w(Relaxation Fcator)  = 0.5, init_guess as uniform, dv_dx_at_firstPoint as FD2
```
1. Plot for NR (Point & Continous Load) and Euler (Point & Continous Load):
   
![Image](https://github.com/user-attachments/assets/659a209b-e613-4199-b5b7-71814ed9ea36)

2. Zoomed image of NR (Continous Load) and Euler (Continous Load):
   
![Image](https://github.com/user-attachments/assets/efc14f03-8185-4d6b-8fd1-bb4515f08db4)

3. Plot for NR (Point Load) and NR (Continous Load):

![Image](https://github.com/user-attachments/assets/4549bc57-05fb-4d79-9b76-a79d26e6cdd6)


---

### Code 3 – Multiple Load Cases

* Superimposes effects of several point/distributed loads
* Outputs complete deflection profile
* Includes visual comparison of all load effects combined

> **Note:** As in Codes 1 and 2, you can adjust the required variable values in Code 3 to generate your desired plots.  
> No sample output is attached here — feel free to experiment with different loading conditions.

---

## Key Features

* **Linear vs. Nonlinear Deflection**: Direct comparison between Euler–Bernoulli theory and full nonlinear curvature equations shows accuracy loss when using linear theory for large deflections.
* **Flexural Rigidity Study**: Demonstrates how higher stiffness (larger $EI$) reduces error between linear and nonlinear predictions.
* **Load Type Effects**: Shows clear visual and numerical differences between point and distributed loading.
* **Superposition**: Scripts demonstrate load combination using linear theory, useful for understanding combined structural effects.
* **Interactive Visualization**: Uses both **Matplotlib** and **Plotly**. Plotly allows zooming, hovering, and dynamic updates, useful for detailed analysis.
* **Tabulated Output**: All data is organized using Pandas for clean tables and easy CSV export or further statistical analysis.

---

## Usage

To run simulations:

```bash
python3 CantileverBeam(Point_&_Continuous) (1).ipynb
```

This executes all cantilever beam cases and displays both console outputs and graphical plots. Similarly, run:

```bash
python3 Simply_Supported_Beam(Point_&_Continuous).ipynb
```

Each script is standalone and automatically processes each case sequentially. **Plotly** plots will open in a browser window (or inline if using Jupyter Notebook). **Matplotlib** plots pop up as figure windows.

---

## Requirements

* **Python 3.8+
* **pandas** – For structured data output
* **matplotlib** – For static plotting
* **plotly** – For interactive plotting

Install dependencies via:

```bash
pip install pandas matplotlib plotly
```

---

## Contributing and Future Work

Contributions are welcome! Please open issues or submit pull requests for improvements.

**Future directions** include:

* Support for **fixed-fixed** or **overhanging** beams
* Inclusion of **Timoshenko beam theory** to account for shear deformation
* **Vibration/dynamic analysis** under time-dependent loads
* **GUI/web interface** for inputting beam parameters and visualizing results interactively

Feel free to enhance simulations by integrating more complex physics, improving numerical methods, or expanding visualization options.

---

## 🤝 Connect with Me

I’d love to connect, collaborate, or just chat about structural analysis, Python simulations, or mechanical engineering!

- 📧 Email: [nitinnishikantadas@gmail.com](mailto:nitinnishikantadas@gmail.com)
- 📞 Phone: +91 7976841096
- 💼 LinkedIn: [LinkedIn Profile](www.linkedin.com/in/nitin-nishikanta-das)
- 💻 GitHub: [https://github.com/ApexDyno](https://github.com/ApexDyno)

Feel free to reach out if you have suggestions, questions, or want to collaborate on a project!


---

