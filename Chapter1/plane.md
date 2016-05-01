#Plane

A plane is a flat surface that extends infinateley in all directions. There are three common ways to represent a plane:

* [Three points (not on a straight line)](https://www.khanacademy.org/math/geometry/tools-of-geometry/points-lines-planes/v/specifying-planes-in-three-dimensions)
* A normal and a point on the plane
* __A normal and the distance from origin__

Your native plane implementation should represent the plane as the third, a normal and a distance from origin. This is the plane equasion. Given the normal and distance, any point ```X``` that satisfyes the equation

```
Dot(X, Normal) == Distance
```

Is on the plane. 