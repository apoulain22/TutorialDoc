.. _srcfvfiles:

FORTRAN files
===========

* geom -> metrics computation.

* phys -> primitives and viscosity computations.

* borders -> boundary conditions.

* lhs -> basic matrix-free inversion for implicit time-stepping method.

* rhs -> spatial numerical scheme for residual computation.

* prepro -> preprocessed files for Algorithmic Diffenrentiation (AD).

* tangent -> linearised files computed by AD.

* adjoint -> linearised files computed by AD in backward mode.

* tangenttangentHess -> twice linearised files computed by AD (Hessian).

* dz -> transverse contributions of the Jacobian operator.

* tangentdz -> linearised transverse contributions.

* misc/ComputeJacobian.f90 -> functions to construct the Jacobian (test-vectors and index ordering).

Linearisation of routines
----------

Routines stored in *tangent*are linearised from the folder *prepro*. Routines in *prepro* are generated from sub-routines in *phys*, *borders*, *lhs* and *rhs*.

.. note::
   
   Never update a routine inside *prepro* or *tangent*. Always modify the sub-routines in the associated folders.


Any modification of a numerical scheme or a boundary condition (anything inside *phys*, *borders*, *lhs* and *rhs*) must be propagated to the preprocessed file *prepro* and the linearised files (at least *tangent* for the Jacobian and optionnaly *adjoint*, *tangenttangentHess*,... if you also use adjoint, Hessian...). For example, if the function :py:func:`bc_no_reflexion_2d` inside *borders* has been updated. Run *compile_borders.py* to update the function in *prepro* and compile it.

.. code-block:: console

   (ENV_NAME) $ python compile_borders.py

Then, to linearise a routine, run the associated program *tap_tangent.py* and compile again the associated source.

.. code-block:: console

   (ENV_NAME) $ python tap_tangent.py
   (ENV_NAME) $ python compile_tangent.py

