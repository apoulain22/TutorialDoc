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

To use BROADCAST; the following Python packages are required:

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

To install BROADCAST, run the following compile command in the three folders **./**, **./misc/** and **./srcfv/**:

.. code-block:: console

   (ENV_NAME) $ python compile*.py



Creating recipes
----------------

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. autofunction:: lumache.get_random_ingredients

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']

