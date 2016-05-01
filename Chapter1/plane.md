#Plane

A plane is a flat surface that extends infinateley in all directions. There are three common ways to represent a plane:

* [Three points (not on a straight line)](https://www.khanacademy.org/math/geometry/tools-of-geometry/points-lines-planes/v/specifying-planes-in-three-dimensions)
* [A normal and a point on the plane](https://www.khanacademy.org/math/linear-algebra/vectors_and_spaces/dot_cross_products/v/defining-a-plane-in-r3-with-a-point-and-normal-vector)
* __A normal and the distance from origin__

Your native plane implementation should represent the plane as the third, a normal and a distance from origin. This is the plane equasion. Given the normal and distance, any point ```X``` that satisfyes the equation

```
Dot(X, Normal) == Distance
```

Is on the plane. We can represent a plane as a normal and a distance from origin, but we can still [construct it from 3 points](http://www.maplesoft.com/support/help/maple/view.aspx?path=MathApps%2FEquationofaPlane3Points) using the following formula:

```cs
// THIS BLOCK IS JUST SAMPLE CODE, DON'T COPY IT!
Plane ComputePlane(Point a, Point b, Point c) {
    Plane result = new Plane();
    result.Normal = Normalize(Cross(b - a, c - a));
    result.Distance = Dot(result.Normal, a);
    return result;
}
```

