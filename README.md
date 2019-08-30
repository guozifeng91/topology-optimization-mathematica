# topology-optimization-mathematica
topology optimization using SIMP method by mathematica

## Results

2d example<br>
![alt text](https://github.com/guozifeng91/topology-optimization-mathematica/blob/master/2d.gif)

3d example<br>
![alt text](https://github.com/guozifeng91/topology-optimization-mathematica/blob/master/3d.gif)

3d definition visualization<br>
(left to right: degree of freedom (x, y and z); top to bottom: loads and fixed nodes)<br>
![alt text](https://github.com/guozifeng91/topology-optimization-mathematica/blob/master/3d%20definition.png)

Mathematica is good for solving large linear system, but managing data with it is not as efficient as python,
so it is quite painful to make sure the code is fast enough 
(for example filling the global K is super slow until i make it an operation purly using matrix slicing and summation).
Luckily, i was able to do this with the help of many articles that talk about the same issue, and the code is now somehow fast.
it is at least 300% faster than the code i implemented with python (which uses scipy to solve the linear system).

## Usage
define a domain for optimization (specify x, y, z size)

```
domain = CreateFEADomain[20, 10, 10];
```
<br>
specify the boundary condition (loads, fixed nodes)<br>
```
domain = Join @@ {domain, SetFixDOFRange[domain, {{0, 1, 0, 11, 0, 11}}, {{0, 1, 0, 11, 0, 11}}, {{0, 1, 0, 11, 0, 11}}]};
domain = Join @@ {domain, SetLoadDOFRange[domain, {}, {}, {{20, 21, 5, 6, 0, 1}}, {}, {}, {1}]};
```
the specification is in this format: {xmin, xmax, ymin, ymax, zmin, zmax}, where xmin start with 0 and xmax is excluded
<br>
then load the elemental stiffness matrix ke (currently only Q4.m and H8.m are available, which are for 2d and 3d problems respectively)
```
ke = ImportStiffnessMatrix[NotebookDirectory[] <> "H8.m", {a -> 0.5, b -> 0.5, c -> 0.5, Em -> 1, nu -> 1/3}];
```
<br>
set the dynamic variable to observe the process
```
Dynamic@$Info
```
<br>
then run the optimization
```
{result, process} = SIMP[domain, ke, 80, 0.1, True];
```
<br>
to visuale the result, use visualze2d and visualze3d accordingly
```
ListAnimate[Visualize3d[result, #] & /@ process]
```
<br>
the optimized design variable (the density/existence of each element) can be obtained via:
```
result["dsgvar"]
```
which is a flattened list, and can be reshaped by
```
ArrayReshape[result["dsgvar"], {result["ny"], result["nx"]}]
```
or
```
ArrayReshape[result["dsgvar"], {result["nz"], result["ny"], result["nx"]}]
```
for 2d or 3d problem respectively.
