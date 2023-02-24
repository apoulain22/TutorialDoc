Usage
=====

Languages
---------

* Python: main programs and user-interface.
* FORTRAN: sub-routines for computational efficiency.

Linear systems are solved through PETSc/SLEPc libraries.

.. note::

   Users need to be familiar with Python, developers may need to have a basic knowledge of FORTRAN.


.. _installation:

Installation
------------

To use BROADCAST, the following Python packages are required:

* numpy
* scipy
* matplotlib
* psutil
* mpi4py
* petsc4py (installed in *complex*)
* slepc4py (installed in *complex*)

An example of installation of the required packages is shown below:

.. code-block:: console

   $ module load anaconda
   $ anaconda-setup
   $ conda create -n ENV_NAME
   $ source activate ENV_NAME
   (ENV_NAME) $ conda install numpy scipy matplotlib psutil mpi4py petsc=*=*complex* petsc4py slepc=*=*complex* slepc4py

Once the packages installed, to install BROADCAST, run the following compile command in the **three** folders **./**, **./misc/** and **./srcfv/**:

.. code-block:: console

   (ENV_NAME) $ python compile*.py

Implementation
--------------

.. figure:: org1.png
   :scale: 90%
   :align: center
   :alt: organisation of the code

.. figure:: org2.png
   :scale: 90%
   :align: center
   :alt: organisation of the code


Program files
--------------

List of the programs and their description available: :ref:`listprogram`.


Update and linearisation of FORTRAN source routines
----------

Routines stored in *tangent* are linearised from the folder *prepro*. Routines in *prepro* are generated from sub-routines in *phys*, *borders*, *lhs* and *rhs*.

.. note::
   
   Never update a routine inside *prepro* or *tangent*. Always modify the sub-routines in the associated folders.


Any modification of a numerical scheme or a boundary condition (anything inside *phys*, *borders*, *lhs* and *rhs*) must be propagated to the preprocessed file *prepro* and the linearised files (at least *tangent* for the Jacobian and optionnaly *adjoint*, *tangenttangentHess*,... if you also use adjoint, Hessian...). 

For example, if the function :py:func:`bc_no_reflexion_2d` inside *borders* has been updated. Run *compile_borders.py* to update the function in *prepro* and compile it.

.. code-block:: console

   (ENV_NAME) $ python compile_borders.py

Then, to linearise a routine, run the associated program *tap_tangent.py* and compile again the associated source.

.. code-block:: console

   (ENV_NAME) $ python tap_tangent.py
   (ENV_NAME) $ python compile_tangent.py


Input/Output
--------------




List of required variables
--------------

List of the common variables and their meaning: :ref:`listvar`





