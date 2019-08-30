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
