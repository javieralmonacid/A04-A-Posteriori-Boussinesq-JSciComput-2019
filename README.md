Adaptive Refinement for Natural Convection problems
=================================================

Natural convection is a heat transfer process that is present in our everyday life: from the cooling of little electronic devices, to indoor climate systems, to environmental transport problems. In this set of codes, we implement an "a posteriori" error estimator for the finite element method implemented in [A01-MixedPrimal-Boussinesq-Calcolo2018](https://github.com/javieralmonacid/A01-MixedPrimal-Boussinesq-Calcolo2018). This estimator is capable of detecting zones where local refinement can be applied to improve the accuracy of the solution in a cheap way.

## Main Characteristics

* The method can handle a temperature-dependent viscosity and a tensorial, space-dependent thermal conductivity.

* The implementation is based on [FreeFem++](https://freefem.org/). 

* The method is of mixed nature. As such, it computes approximations for the pseudostress tensor, velocity, vorticity, temperature and normal heat flux through the boundary. Quantities such as pressure are postprocessed using the computed variables.

* For a first order method, the following combination of finite elements can be used:
```C++
fespace Hhs(Th,[RT0,RT0]); 	// Pseudostress
fespace Hhu(Th,[P1,P1]); 	// Velocity
fespace Hhg(Th,P0);			// Vorticity
fespace Hhp(Th,P1);			// Temperature
fespace Hhl(Sh,P0edge);		// Normal heat flux through the boundary
```
* For a second order method, use
```C++
fespace Hhs(Th,[RT1,RT1]); 	// Pseudostress
fespace Hhu(Th,[P2,P2]); 	// Velocity
fespace Hhg(Th,P1dc);			// Vorticity
fespace Hhp(Th,P1);			// Temperature
fespace Hhl(Sh,P1edge);		// Normal heat flux through the boundary
```
This requires to load "Element_Mixte" and "Element_PkEdge".

* Higher order finite elements can also be used, however, the computational cost of this method might then become prohibitive.

## Refinement algorithm

1. Start with a coarse mesh Th,
2. Solve the discrete equations,
3. Compute the estimator in each triangle,
4. Check stopping criteria and decide whether to finish or continue to the next step,
5. Generate adapted mesh through a variable metric/Delaunay automatic meshing algorithm,
6. Define resulting mesh as Th and go to step 2.

## Reference article

To find out more about the theoretical aspects of this method, as well as comparisons between the codes in this repository and their expected results, read

> J. A. Almonacid, G. N. Gatica and R. Oyarzua, 
> [*A posteriori error analysis of a mixed-primal finite element method for the Boussinesq problem with temperature-dependent viscosity*](https://rdcu.be/4YAs). 
> Journal of Scientific Computing 78 (2019), 887--917.
