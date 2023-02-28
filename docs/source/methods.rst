Methods
=======

Equations
------

:math:`\frac{dq}{dt} = R(q)`

with q the conservative state and R the residual of Navier-Stokes equations.

Numerical scheme
-----

Convective scheme
^^^^

The convective scheme implemented is the FE-MUSCL scheme, called DNC in the program files. This high-order scheme is available at order 3 (5 points stencil), 5 (7 points stencil), 7 (9 points stencil) and 9 (11 points stencil). Different versions of the scheme exist:

* :func:`flux_num_dnc3_nowall_2d` calls FE-MUSCL at order 3. It is the baseline scheme for cartesian equations.
* :func:`flux_num_dnc3_nowall_polar_2d` is the baseline scheme for axisymmetric equations.
* :func:`flux_num_dnc3_2d` is the scheme for cartesian equations with a wall boundary at j=0 along all i-direction (flat plate case for instance). This scheme includes off-centered computation of gradients in order not to pick values far inside the wall. Results are very similar to those obtained with :func:`flux_num_dnc3_nowall_2d`.
* :func:`flux_num_dnc3_polar_2d` is the same as above for axisymmetric equations.


Viscous scheme
^^^^

The viscous scheme is a compact scheme at order 4 (5 point stencil) inside the domain and at order 2 (3 point stencil) near the wall boundary at j=0 if there is one.


Boundary conditions
-----



Linearised operators - Jacobian
-----

Exact linearisation of the residual is computed by the Algorithmic Differentiation tool. Then, the Jacobian is computed by series of test-vectors to fill in the different entries of the Jacobian without overlapping cross contributions. Test-vectors and indexing of matrix-vector products functions are inside *ComputeJacobian.f90*.

.. note::
   
   Opposite of the Jacobian is computed from the residual: :math:`A = - \frac{dR}{dq} \Rightarrow \frac{dq'}{dt} + Aq' = 0`

Time solvers
-----

Three (pseudo-)time solvers are available:

* *direct*: low-storage Runge-Kutta.
* *implicit*:  matrix-free implicit solver (similar to LU-SGS on approximated Jacobian).
* *fixed_point*: Newton solver.

