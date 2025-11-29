This repository contains the work I completed for a technical assignment related to program synthesis and dynamical systems.
The goal of the project was to generate data from two given systems and then recover their governing equations using a simple synthesis-based approach.
I include here only my own implementation and explanation.
The repository contains two Jupyter notebooks (System1.ipynb and System2.ipynb), along with two short reports where I describe my reasoning process and how I approached the problem.

The main idea behind the project is to treat the discovery of a dynamical system as a small program-synthesis problem. Since the systems are known to be polynomial (up to degree 3),
the problem reduces to figuring out the coefficients of those polynomials based on simulated data.

To do that, I followed these steps:

Generate synthetic data
I simulated each system starting from chosen initial conditions for a fixed number of time steps. For System 1 I generated 11 steps, and for System 2 I generated 20.

Approximate the derivatives
Using the simulated trajectories, I computed finite-difference approximations of the derivatives.

Construct the polynomial form
Because the equations are assumed to be polynomials of degree ≤3, each derivative is expressed as a linear combination of monomials like
1, x, y, x², xy, y², x³, x²y, xy², y³ (and extended for the 3-variable system).

Solve for the coefficients
Each timestep gives one equation, and collecting them leads to a linear system. I solved these systems using NumPy’s linear algebra tools.

This produces a set of polynomial coefficients that approximate the underlying differential equations.
