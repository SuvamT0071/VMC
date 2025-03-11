# Quantum Monte Carlo for Hydrogen Atom and Quantum Harmonic Oscillator

This project implements a **Variational Monte Carlo (VMC)** simulation for the **Hydrogen atom's ground state wavefunction** and the **Quantum Harmonic Oscillator (QHO)** using Python. It includes functions for defining the trial wavefunction, probability density function (PDF), local energy calculation, and Monte Carlo sampling to optimize the variational parameter (alpha).

---

## Features
✅ Variational Monte Carlo (VMC) implementation for the Hydrogen atom and QHO  
✅ Analytic solution for the Hydrogen atom's ground state wavefunction and QHO wavefunction  
✅ Numerical optimization of the variational parameter 
✅ Efficient Monte Carlo sampling with controlled step size 
✅ Stable PDF calculation with improved numerical stability 

---

## Installation

To run this project, ensure you have the following libraries installed:

```bash
pip install numpy tqdm
```

---

## Code Structure

- **`Hyd_GS()`** : Defines the trial wavefunction for the ground state of the Hydrogen atom.  
- **`Hyd_GSPDF()`** : Computes the probability density function (PDF) for the wavefunction.  
- **`Hyd_local()`** : Calculates the local energy of the Hydrogen atom's wavefunction.  
- **`Hyd_VMC()`** : Performs Variational Monte Carlo sweeps to sample positions and calculate energy.  
- **`Hyd_alpha_opt()`** : Optimizes the variational parameter (alpha) for minimum energy.  
- **`psi_analytic()`** : Defines the exact analytic solution for the Hydrogen atom's ground state.  
- **`QHO_GS()`** : Defines the trial wavefunction for the Quantum Harmonic Oscillator.  
- **`QHO_GSPDF()`** : Computes the PDF for the QHO wavefunction.  
- **`QHO_local()`** : Calculates the local energy of the QHO wavefunction.  

---

## Usage

### Example Code
```python
import numpy as np
from tqdm import tqdm

def Hyd_GS(r, alpha=2):
    return alpha * r * np.exp(-alpha * r)

def Hyd_GSPDF(r, alpha=1):
    wave_func = Hyd_GS(r, alpha)
    return np.abs(wave_func)**2

def Hyd_local(r, alpha=2):
    if r <= 0:
        raise ValueError("Position value 'r' must be greater than zero.")
    return -1/r - (alpha/2) * (alpha - (2/r))

def Hyd_VMC(r, step, samples=10000, alpha=2):
    position_saved = []
    energy_saved = []
    for n in range(samples):
        r_new = r + np.random.uniform(-step, step)
        if r_new <= 0:
            continue
        P_old = Hyd_GSPDF(r, alpha)
        P_new = Hyd_GSPDF(r_new, alpha)
        ratio = P_new / (P_old + 1e-10)
        s = np.random.rand()
        if ratio > s:
            r = r_new
        position_saved.append(r)
        energy_saved.append(Hyd_local(r, alpha))
    return position_saved, energy_saved

def psi_analytic(r, alpha):
    return (alpha**(3/2)) * np.exp(-alpha * r) / np.sqrt(np.pi)
```

---

### Running the Code
To run the code, follow these steps:
1. Clone the repository:
```bash
git clone <repository-link>
cd <repository-folder>
```

2. Run the Jupyter Notebook or Python script:
```bash
jupyter notebook "QMC QHO.ipynb"
```

3. Use the provided functions to test wavefunctions, visualize results, or optimize alpha.

---

## Mathematical Background
### Choosing a Trial Wavefunction
In the Variational Monte Carlo method, we approximate the ground state wavefunction with a trial function containing a free parameter (alpha). For accurate energy estimation, the chosen trial function should capture key properties of the system such as symmetry, boundary conditions, and decay behavior. 

For example:
- Hydrogen Atom Trial Function: \( \, \Psi(r) = \alpha r e^{-\alpha r} \)
- Quantum Harmonic Oscillator Trial Function: \( \, \Psi(x) = \frac{\alpha^{1/2}}{\pi^{1/4}} e^{-\alpha^2 x^2 / 2} \)

### Local Energy Calculation
The **local energy** is defined as:

\[ E_{\text{local}}(r) = \frac{H\Psi(r)}{\Psi(r)} \]

Where \( H \) is the Hamiltonian operator. For the hydrogen atom:

\[ E_{\text{local}}(r) = -\frac{1}{r} - \frac{\alpha}{2} \left( \alpha - \frac{2}{r} \right) \]

For the Quantum Harmonic Oscillator:

\[ E_{\text{local}}(x) = \alpha^2 + x^2(1 - \alpha^4) \]

Minimizing the mean local energy leads to the optimal parameter \( \alpha \) that best approximates the true ground state energy.

---

## Sample Plot
To visualize the results, you can plot the optimized wavefunction and compare it with the analytic solution.

```python
import matplotlib.pyplot as plt

x_vals = np.linspace(0, 5, 500)
optimal_alpha = 1.5  # Example optimized value
plt.plot(x_vals, psi_analytic(x_vals, optimal_alpha), label="Analytic Solution")
plt.legend()
plt.title("Hydrogen Atom Ground State Wavefunction")
plt.xlabel("r")
plt.ylabel(r"$\Psi(r)$")
plt.grid(True)
plt.show()
```
![image](https://github.com/user-attachments/assets/0a0fbca7-b85c-408c-bcf0-e715bf5f1e51)
![image](https://github.com/user-attachments/assets/1e28196b-cd69-4483-abb7-036507e275fb)
![image](https://github.com/user-attachments/assets/ae149eb5-9589-49d0-a70d-aa80eebbbfef)
![image](https://github.com/user-attachments/assets/1d6f68f1-7f1d-4905-bc89-cfa2eda4d3c0)

---

## This was made by:
- **Suvam Tripathy, A MSc Physics student at IIT Madras as a part of a mini project.**

---

## License
This project is licensed under the MIT License. Feel free to use, modify, and distribute the code.

