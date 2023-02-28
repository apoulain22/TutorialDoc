Methods
=======

Numerical scheme
-----



Convective scheme
^^^^

The convective scheme implemented is the FE-MUSCL scheme, called DNC in the program files. This high-order scheme is available at order 3 (5 points stencil), 5 (7 points stencil), 7 (9 points stencil) and 9 (11 points stencil).


Viscous scheme
^^^^

The viscous scheme is a compact scheme at order 4 (5 point stencil) inside the domain and at order 2 (3 point stencil) near the solid boundary at j=0.


Boundary conditions
-----



Linearised operators - Jacobian
-----



Time solvers
-----



