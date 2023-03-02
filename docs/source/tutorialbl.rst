Tutorial: Boundary layer
=====================


Base-flow
--------

To compute the base-flow of a boundary layer, two Python programs should be modified:

#. the card (choice of geometry, physics, solver methods...): **card_bl2d_fv_npz.py**.
#. the main program (choice of mesh, BC...): **BROADCAST_npz.py**.

In this tutorial, we will come through each program step by step to setup a hypersonic boundary layer over an adiabatic flat plate.

Inside **card_bl2d_fv_npz.py**, specify first the geometry of your domain: discretisation in i-direction (equal to x-direction only if the domain is horizontal) and j-direction, length, height and initial offset from origin:

.. code-block:: python

   #GEOMETRY
im    = 300   # X discretization
jm    = 150  # Y discretization 
L     = 0.59  # FP length
high  = 0.035   # FP high
xini  = 0.0060  # beginning 

Choose to write the 

Resolvent
--------



Global stability analysis
--------


