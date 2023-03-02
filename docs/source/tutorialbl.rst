Tutorial: Boundary layer
=====================


Base-flow
--------

To compute the base-flow of a boundary layer, two Python programs should be modified:

#. the card (choice of geometry, physics, solver methods...): **card_bl2d_fv_npz.py**.
#. the main program (choice of mesh, BC...): **BROADCAST_npz.py**.

In this tutorial, we will come through each program step by step to setup a hypersonic boundary layer over an adiabatic flat plate.

card_bl2d_fv_npz.py
^^^^^

First, inside **card_bl2d_fv_npz.py**, specify first the geometry of your domain: discretisation in i-direction (equal to x-direction only if the domain is horizontal) and j-direction, length, height and initial offset from origin:

.. code-block:: python

   im    = 300     # X discretization
   jm    = 150     # Y discretization 
   L     = 0.59    # FP length
   high  = 0.035   # FP high
   xini  = 0.0060  # beginning 

Select the folder *out_dir* to save your state solution and the *npz* file *treename* to save your geometry, state, residual, Jacobian.

.. code-block:: python

   out_dir = 'Wksp/'
   treename = 'tree300x150'

Select the number of iterations of your method (for any method). Select the CFL number, the frequency of printing/writing (for explicit/implicit methods only). For Newton method, CFL number is specified inside **BROADCAST_npz.py**, it prints residual every iterations and writes at the last iteration. 

.. code-block:: python

   ite     = 10
   cfl     = 0.9
   freqres = ite/2 # frequency to print residual defined by the integer, here 2
   freqsort= ite/1 # frequency to write solution defined by the integer, here 1

Select the order of the convective scheme:

.. code-block:: python

   sch   = 'dnc'  # numerical scheme 
   order =   5    # formal FD order for dnc

Select the order of the extrapolation for the extrapolation BC (if used):

.. code-block:: python

   extraporder = 2   # extrapolation order for outflow

Select the equations/type of numerical scheme to use: *polar* for axisymmetric, *nowall* for general cartesian configuration and default one for a flat plate:

.. code-block:: python

   if sch == 'dnc':
       routinesch = 'flux_num_dnc%i_2d' % order
       # routinesch = 'flux_num_dnc%i_nowall_2d' % order
       # routinesch = 'flux_num_dnc%i_nowall_polar_2d' % order 

Select the type of Boundary Conditions to use (location and application of the BC must be specified inside **BROADCAST_npz.py**):

.. code-block:: python

   routineout = 'bc_extrapolate_o%i_2d' % extraporder
   routinein  = 'bc_supandsubinlet_2d'
   # routinein  = 'bc_general_2d'
   routinenr  = 'bc_no_reflexion_2d'
   routinew   = 'bc_wall_viscous_adia_2d'


Select the solver method, *fixed_point* for Newton method:

.. code-block:: python

   compmode = 'fixed_point'  # computational mode in ['direct', 'impli', 'fixed_point']

Select your physical setup parameters with:

#. Mach number
#. Static free-stream temperature
#. Unit Reynold number

.. code-block:: python
   
   dphys['Mach']     = 4.5  
   dphys['T0']       = 288.  
   # dphys['P0']       = 728.312  
   dphys['Runit']    = 3.4e6

At the end of the file **card_bl2d_fv_npz.py**, the function :func:`bl2d_prepro` from **BROADCAST_npz.py** file initialises the geometry, the BC location and the normalisation. This function should also be modified by the user.

Then, the function :func:`bl2d_fromNPZtree` from **BROADCAST_npz.py** solves the configuration previously setup by :func:`bl2d_prepro`.

.. note::

   To restart a computation, comment the call to function :func:`bl2d_fromNPZtree` inside **card_bl2d_fv_npz.py**, otherwise you will repeat the pre-process.


BROADCAST_npz.py
^^^^^

Secondly, go inside the function :func:`bl2d_fromNPZtree` in **BROADCAST_npz.py**.

Specify the mesh in x-direction, the mesh is here uniform:

.. code-block:: python

   ## MESH in x-direction
   x  = _np.linspace(xini, xini+L , im+1)


Specify the mesh in y-direction, the mesh is splitted into two parts with different stretching if y<*deltaBL* or y>*deltaBL*:

.. code-block:: python

   ## MESH in y-direction
   Ny_in   = 80*jm//100 #number of points inside the BL    
   deltaBL = high/4     #height of the BL
   percent = 0.02       #growth factor increase inside the BL

   Ny_out  = jm - Ny_in 
   Nend    = high/deltaBL
   y_int   = mesh.bigeom_stretch_in(Ny_in, deltaBL, percent)
   y_out   = mesh.exp_stretch_out(Ny_out, deltaBL, percent, Nend)
   y       = _np.concatenate((y_int, y_out)) 

.. note::

   You can create your own mesh with an external meshing tool. For a cartesian rectangular mesh, import *x* and *y* grid point profiles as numpy arrays. Otherwise, import the full range of grid points as numpy arrays and store it inside the variables *x0* and *y0*.

Normalisation is performed with :math:`\rho_\infty`, :math:`U_\infty` and :math:`T_\infty`. In this example, normalisation of the length is performed with the unit Reynolds number.

.. code-block:: python

   ## Adim with ref length
   # Lref   = 8.e-2  
   # Muref  = Roref*Vref*Lref
   ## OR Adim with unit Reynolds
   Muref  = muinf
   Lref   = Muref/(Roref*Vref)
   ## OR no normalisation
   # Roref = 1.
   # Vref  = 1.
   # Tref  = 1.
   # Lref  = 1.
   # Muref = 1.

.. note::

   Dimensionalised data were provided inside **card_bl2d_fv_npz.py** because the normalisation is performed here. It is recommended to run the solver with normalisation as the operators are better conditionned for linear solvers. Resolvent and global stability analysis assumes normalised operators so normalisation is strongly recommended.




Resolvent
--------



Global stability analysis
--------


