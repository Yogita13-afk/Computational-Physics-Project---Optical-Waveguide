# Numerical Analysis of Optical Waveguide Eigenmode

A Computational Physics project on modelling optical waveguides and studying their guided modes using numerical methods.

The project starts with a one-dimensional slab waveguide and extends to a two-dimensional rectangular waveguide. It further explores multilayer cladding design and the use of Monte Carlo sampling to investigate different material combinations.

The main idea is to convert the wave equation into a matrix eigenvalue problem and solve it numerically to obtain the propagation constants and corresponding mode profiles.

## Project Overview

Optical waveguides are structures that confine light within a region of higher refractive index, surrounded by materials with lower refractive indices.

The propagation of light depends on the geometry of the waveguide and the refractive-index distribution across it.

In this project, numerical methods are used to calculate the guided modes and investigate how changing the waveguide dimensions and material properties affects light confinement.

The notebook is divided into four main parts:

1. One-dimensional slab waveguide.
2. Two-dimensional rectangular waveguide.
3. Multilayer cladding design and optimisation.
4. Monte Carlo search for suitable refractive-index combinations.

## 1. One-Dimensional Slab Waveguide

The first part considers a slab waveguide with a high-index core surrounded by lower-index cladding on either side.

The scalar Helmholtz equation is used to describe the transverse electric field.

Assuming propagation along the z-direction:

$$
E(x,z)=\psi(x)e^{i\beta z}
$$

The resulting equation is:

$$
\frac{d^2\psi}{dx^2}+
[k_0^2n^2(x)-\beta^2]\psi=0
$$

where:

- $\psi(x)$ is the transverse electric-field profile.
- $\beta$ is the propagation constant.
- $k_0=2\pi/\lambda$ is the free-space wave number.
- $n(x)$ is the refractive-index profile.

### Numerical approach

The Finite Difference Method (FDM) is used to approximate the second derivative:

$$
\frac{d^2\psi}{dx^2}
\approx
\frac{\psi_{i+1}-2\psi_i+\psi_{i-1}}{\Delta x^2}
$$

This converts the differential equation into a matrix eigenvalue problem:

$$
A\boldsymbol{\psi}=\beta^2\boldsymbol{\psi}
$$

The eigenvalues give the propagation constants, while the corresponding eigenvectors represent the transverse mode profiles.

### What is investigated?

- Construction of the refractive-index profile.
- Numerical solution of the eigenvalue problem.
- Identification of guided modes.
- Visualisation of the transverse field distributions.
- Effect of core width on the effective refractive index.

The effective refractive index is calculated using:

$$
n_{\mathrm{eff}}=\frac{\beta}{k_0}
$$

For a guided-mode candidate:

$$
n_{\mathrm{cladding}}<n_{\mathrm{eff}}<n_{\mathrm{core}}
$$

The variation of the effective index with core width is also studied.

## 2. Two-Dimensional Rectangular Waveguide

The second part extends the problem to two dimensions.

Instead of a slab, the waveguide now consists of a rectangular high-index core surrounded by a lower-index cladding.

The transverse field depends on both x and y:

$$
E(x,y,z)=\psi(x,y)e^{i\beta z}
$$

The scalar Helmholtz equation becomes:

$$
\nabla_\perp^2\psi+
[k_0^2n^2(x,y)-\beta^2]\psi=0
$$

where:

$$
\nabla_\perp^2=
\frac{\partial^2}{\partial x^2}
+
\frac{\partial^2}{\partial y^2}
$$

### Numerical approach

The two-dimensional Laplacian is constructed using finite differences.

Sparse matrices and Kronecker products are used to build the numerical operator efficiently.

The resulting eigenvalue problem is:

$$
A\boldsymbol{\psi}=\beta^2\boldsymbol{\psi}
$$

The eigenvalues and eigenvectors are calculated using the sparse eigenvalue solver `eigsh()` from SciPy.

### What is investigated?

- Construction of the 2D refractive-index profile.
- Implementation of the two-dimensional finite difference Laplacian.
- Use of sparse matrices and Kronecker products.
- Calculation of propagation constants and effective indices.
- Visualisation of the transverse field distributions and intensity profiles.
- Identification of fundamental and higher-order mode candidates.
- Effect of core width on the effective refractive index.

## 3. Multilayer Cladding Design and Optimisation

In this part, the uniform cladding is replaced with three rectangular layers having different refractive indices.

The refractive-index structure consists of:

- A high-index rectangular core.
- Three surrounding cladding layers.
- An outer medium with a lower refractive index.

The objective is to investigate how changing the thicknesses of the cladding layers affects the confinement of the optical mode inside the core.

### Core confinement factor

The core confinement factor is defined as:

$$
\Gamma_{\mathrm{core}}=
\frac{
\sum_{(i,j)\in\mathrm{core}}|\psi_{ij}|^2
}{
\sum_{(i,j)\in\mathrm{domain}}|\psi_{ij}|^2
}
$$

It estimates the fraction of the modal intensity present inside the core.

### Optimisation procedure

A parameter sweep is performed over different combinations of cladding thicknesses.

For each combination:

1. A new refractive-index profile is constructed.
2. The finite difference matrix is rebuilt.
3. The eigenvalue problem is solved.
4. The effective refractive index and core confinement factor are calculated.
5. The results are compared with other configurations.

The configuration with the highest calculated core confinement factor is selected, subject to the effective-index condition.

A heatmap is also generated to visualise how the confinement factor changes with the layer thicknesses.

The selected configuration is then recalculated using the original computational grid.

## 4. Monte Carlo Search for Refractive Indices

The final part uses Monte Carlo sampling to explore different combinations of cladding refractive indices.

Instead of varying the layer thicknesses, the thicknesses are kept fixed and the three cladding refractive indices are randomly generated.

The sampled indices must satisfy:

$$
n_{\mathrm{core}}>n_1>n_2>n_3>n_{\mathrm{out}}
$$

For each valid combination, the refractive-index profile is reconstructed and the eigenvalue problem is solved.

The effective refractive index is then compared against a chosen target value.

A combination is accepted if:

$$
|n_{\mathrm{eff}}-n_{\mathrm{target}}|
\leq \epsilon
$$

where $\epsilon$ is the allowed tolerance.

### What is investigated?

- Random generation of cladding refractive indices.
- Application of physical ordering constraints.
- Repeated numerical solution of the eigenvalue problem.
- Identification of material combinations close to a target effective index.
- Visualisation of the distribution of sampled effective indices.

The search uses 250 random trials with a fixed random seed for reproducibility.

Monte Carlo sampling does not guarantee a global optimum. It provides a way to explore possible material combinations and identify promising candidates.

## Numerical Methods Used

| Method | Purpose |
|---|---|
| Finite Difference Method | Discretisation of the Helmholtz equation |
| Matrix Eigenvalue Method | Calculation of propagation constants and mode profiles |
| Sparse Matrix Computation | Efficient handling of large matrices |
| Kronecker Products | Construction of the 2D Laplacian |
| Parameter Sweep | Investigation of cladding thickness |
| Core Confinement Factor | Measurement of modal intensity inside the core |
| Monte Carlo Sampling | Random exploration of refractive-index combinations |

## Technologies and Libraries

- **Python** – Main programming language.
- **NumPy** – Numerical calculations and array operations.
- **SciPy** – Sparse matrices, Kronecker products and eigenvalue solvers.
- **Matplotlib** – Plotting and visualisation.
- **Jupyter Notebook** – Development and execution of the simulations.

## Running the Project

### Requirements

Python 3.x and the following libraries:

```bash
pip install numpy scipy matplotlib jupyter
```

### Clone the repository

```bash
git clone <your-repository-url>
cd <repository-name>
```

### Launch Jupyter Notebook

```bash
jupyter notebook
```

Open:

`Optical_Waveguide_Complete_1D_2D_Monte_Carlo.ipynb`

Run the notebook cells sequentially from top to bottom, since later sections reuse functions and variables defined earlier.

## Results and Observations

Some of the important observations from the simulations:

- The fundamental mode is concentrated around the high-index core.
- The field decays into the surrounding cladding.
- Increasing the core width generally improves confinement and moves the effective index closer to the core refractive index.
- Different cladding thickness combinations produce different core confinement factors.
- The multilayer design search identifies a configuration with higher core confinement within the tested parameter range.
- Monte Carlo sampling produces a distribution of effective refractive indices and helps identify material combinations close to a specified target.

## Limitations

There are a few limitations to the model:

- The simulations use the scalar Helmholtz equation rather than solving the full vector Maxwell equations.
- Polarisation effects and vector-field interactions are therefore not fully represented.
- Zero-field boundary conditions are imposed at the edges of the computational domain.
- Grid spacing and computational domain size can affect the numerical results.
- The optimisation depends on the selected parameter ranges and objective function.
- The Monte Carlo search is stochastic and may miss suitable combinations when the number of trials is limited.
- Absorption, scattering, fabrication imperfections and propagation losses are not included.

## Repository Structure

```text
optical-waveguide-eigenmodes/
│
├── Optical_Waveguide_Complete_1D_2D_Monte_Carlo.ipynb
│
└── README.md
```

## Acknowledgement

This project was carried out as part of a Computational Physics study, applying numerical methods to the problem of optical wave propagation and mode confinement.It was done by Praneth,Yogita and Tanush.
