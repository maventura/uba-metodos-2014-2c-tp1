# Blast Furnace Isotherm Estimation (CQTDAH)

This project was developed for the **Numerical Methods Laboratory** (Trabajo Práctico 1). It provides a high-performance C++ implementation to estimate the internal temperature distribution and specific isotherms within the wall of a cylindrical blast furnace.

The project models the steady-state heat equation in polar coordinates and solves the resulting linear system using **Gaussian Elimination** and **LU Factorization**.

##  Problem Overview

The goal is to determine if the $500^\circ\text{C}$ isotherm is too close to the outer wall of a furnace, which could indicate structural risk.
* **Internal Temperature ($T_i$):** $1500^\circ\text{C}$.
* **External Temperature ($T_e$):** Measured by $n$ sensors
* **Model:** 2D Heat Equation in steady state:
    $$\frac{\partial^2T}{\partial r^2} + \frac{1}{r}\frac{\partial T}{\partial r} + \frac{1}{r^2}\frac{\partial^2T}{\partial \theta^2} = 0$$

##  Project Structure

* **`src/`**: Core C++ source code.
    * `Matrix.h/cpp`: Custom matrix library for linear algebra operations.
    * `WithFifteenThetas.h/cpp`: Logic for system discretization and isotherm calculation.
    * `tiempo.h`: High-precision benchmarking using Intel x86-64 `RDTSC` assembly.
* **`src/exp/`**: Prototypes in MATLAB and Python used for verification.
* **`src/experimentacion/`**: Automation scripts in **Go** and **Bash** for performance analysis.
* **`docs/` & `report/`**: LaTeX documentation and the final technical report.

##  Getting Started

### Prerequisites

* **Compiler:** `g++` (version 4.6.3 or higher) with C++11 support.
* **Architecture:** Intel x86-64 (required for the hardware-level benchmarking extension).
* **Build Tool:** GNU Make.

### Compilation

Build the main executable using the provided Makefile:

    make main

This generates an executable named `tp`.

### Execution

The program accepts the following command-line arguments:

    ./tp <input_file> <output_file> <method> [isotherm_file]

* **`input_file`**: Path to the `.in` file containing furnace parameters and sensor data.
* **`output_file`**: Path where the resulting temperature vector will be saved.
* **`method`**: `0` for Gaussian Elimination, `1` for LU Factorization.
* **`isotherm_file`** (Optional): Path to save the calculated radii of the sought isotherm.

## 📊 Methodology

1.  **Discretization:** The furnace wall (Sector A) is partitioned into $m$ radial increments and $n$ angular increments.
2.  **Finite Differences:** Derivatives are approximated to transform the PDE into a system of linear equations $At = b$.
3.  **Solver:** * **Gaussian Elimination:** $\mathcal{O}((nm)^3)$ complexity per instance.
    * **LU Factorization:** $\mathcal{O}((nm)^3)$ for the first instance, $\mathcal{O}((nm)^2)$ for subsequent instances with different boundary conditions.
4.  **Interpolation:** Linear interpolation is used to find the exact radial position of the isotherm between discrete points.

## 📈 Performance Analysis

The repository includes a comprehensive experimental suite. Key findings from the report indicate:
* LU Factorization is significantly more efficient for real-time monitoring systems where sensor data changes frequently but the furnace geometry (the matrix $A$) remains constant.
* The system implementation is verified to be Diagonal Dominant (non-strict), ensuring stability for Gaussian Elimination without pivoting.

## ⚖️ License
This project was developed for academic purposes at the University of Buenos Aires (UBA).
