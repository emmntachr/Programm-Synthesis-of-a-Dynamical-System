This repository contains the work I completed for a technical assignment related to program synthesis and dynamical systems.
The goal of the project was to generate data from two given systems and then recover their governing equations using a simple synthesis-based approach.
I include here only my own implementation and explanation.
The repository contains three Jupyter notebooks (System1.ipynb and System2.ipynb,System1synthesis.ipynb), along with the task and a report where I describe my reasoning process and how I approached the problem.

The main idea behind the project is to treat the discovery of a dynamical system as a small program-synthesis problem.

Since the systems are known to be polynomial (up to degree 3), the task reduces to inferring the polynomial coefficients from simulated data.
I implemented two methods for this:

Direct reconstruction, where I assume from the start that the system has degree ≤ 3 and solve for all coefficients at once.

Bottom-up polynomial enumeration, where I begin with the simplest possible models (degree 1), test them using least squares, and only increase the polynomial degree when the simpler models fail to fit the data well.

-----------------------------------------------------
Direct reconstruction

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

------------------------------------------

Bottom-Up Polynomial Enumeration Approach

In addition to the direct degree-3 polynomial reconstruction, I implemented a bottom-up program synthesis approach.
Instead of assuming from the start that the governing equations are degree 3, we enumerate polynomial models by increasing complexity.

The process works similar as before but changes as follows:
1. Generate polynomial design matrices by size
For each candidate model size, we construct a different polynomial basis:
Size 1: degree ≤ 1  1, x, y
Size 2: degree ≤ 2  1, x, y, x², xy, y²
Size 3: degree ≤ 3  1, x, y, x², xy, y², x³, x²y, xy², y³ 

The function (matrix_generation) builds these matrices depending on the inserted size.

For each size level, we solve: Ax=b like we did prior but this time we use the least square method 
using np.linalg.lstsq, which finds the best approximation.

-------------------------------------------

Comparison of the Two Approaches

The main difference between the two methods is how the polynomial model is chosen:

In the first approach, the system is assumed from the start to have a polynomial of degree 3.
We build the basis and solve a single linear system.
This always produces a valid model, but it may be more complex than necessary.

In the bottom-up enumeration approach, we start with the smallest possible model.
We solve each case using the least-squares method and compute the fitting error.
If the error is already sufficiently small for the dynamics, then the system can be described by these simpler equations, without needing to use the full degree-3 polynomial.
Only if the simple models fail, we move up to degree 2 and then degree 3.

This approach ensures that we select the simplest model that explains the data.
