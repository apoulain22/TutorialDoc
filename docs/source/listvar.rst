.. _listvar:


Variables
==========


Required input:

* **im** (*integer*): number of cells in i-direction.
* **jm** (*integer*): number of cells in j-direction.
* **gh** (*integer*): number of ghost cells for the boundary conditions. **gh** depends on the order of the numerical scheme: **gh**=2 (3th order), **gh**=3 (5th order), **gh**=4 (7th order) or **gh**=5 (9th order).
* **xo** (*numpy array of size (**im**+1+2**gh**,**jm**+1+2**gh**)*): x coordinates of the cell nodes.
* **yo** (*numpy array of size (**im**+1+2**gh**,**jm**+1+2**gh**)*): y coordinates of the cell nodes.
* **xc** (*numpy array of size (**im**+2**gh**,**jm**+2**gh**)*): x coordinates of the cell centers.
* **yc** (*numpy array of size (**im**+2**gh**,**jm**+2**gh**)*): y coordinates of the cell centers.
* **vol** (*numpy array of size (**im**+2**gh**,**jm**+2**gh**)*): volumes of the cells.
* **volf** (*numpy array of size (**im**+2**gh**,**jm**+2**gh**,2)*): inverse of the volume computed at the cell faces. 
* **w** (*numpy array of size (**im**+2**gh**,**jm**+2**gh**,5)*): state. **w[:,:,0]**:math:`=\rho` is the density, **w[:,:,0]** is the momentum in x-direction, 
* **res** (*numpy array of size (**im**+2**gh**,**jm**+2**gh**)*): volumes of the cells.
