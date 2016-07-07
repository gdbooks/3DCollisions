# Robust SAT

The reason that our triangle - triangle collision has a false negative is because the two triangles are on the exact same plane. When this happens, the cross product of any of the vectors yields 0! 

We can actually fix this issue. First, we have to know if the result of the cross product is 0. Remember, floating point errors tend to creep up, so we have to check against an epsilon. The easyest way to do this is to check the square magnitude of the vector

```cs
bool isZeroVector = crossVector.LengthSquared() < 0.0001f;
```

## Constructing a new axis

The key here is, if we are given two parallel vectors, to create a new vector that is perpendicular to both. For this all we need is a new axis to test.


## With a triangle

We know a trianlge has the following properties:

```cs
Vector3 edge1 = p1 - p0; // B - A
Vector3 edge2 = p2 - p1; // C - B
Vector3 edge3 = p0 - p2; // A - C
Vector3 faceNormal = Cross(edge1, edge2); BA X CB
```