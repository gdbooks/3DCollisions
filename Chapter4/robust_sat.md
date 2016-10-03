# Robust SAT

The reason that our triangle - triangle collision has a false negative is because the two triangles are on the exact same plane. When this happens, the cross product of any of the vectors yields 0! 

We can actually fix this issue. First, we have to know if the result of the cross product is 0. Remember, floating point errors tend to creep up, so we have to check against an epsilon. The easyest way to do this is to check the square magnitude of the vector

```cs
bool isZeroVector = crossVector.LengthSquared() < 0.0001f;
```

## Constructing a new axis

The key here is, if we are given two parallel vectors, to create a new vector that is perpendicular to both. For this all we need is a new axis to test.

We have the function __TestAxis__ defined as so

```cs
bool TestAxis(Triangle triangle1, Triangle triangle2, Vector3 axis)
```

The key here is, instead of passing in the axis to test, we want to pass in the components that are used in the cross product to create the axis. The above would be rewritten like so

```cs
bool TestAxis(Triangle triangle1, Triangle triangle2, Vector3 A, Vector3 B, Vector3 C, Vector3 D) {
    Vector3 axis = Vector3.Cross(A - B, C - D);
    
    if (axis.LengthSquared() < 0.0001f) {
        // Axis is zero, try other combination
        Vector3 n = Vector3.Cross(A - B, C - A);
        axis = Vector3.Cross(AB, n);
        
        if (axis.LengthSquared() < 0.0001f) {
            // Axis is still zero, not a seperating axis
            return false;
        }
    }
    
    // The rest of the function
}
```

## With a triangle

We know a trianlge has the following properties:

```cs
Vector3 edge1 = p1 - p0; // Y - X
Vector3 edge2 = p2 - p1; // Z - Y
Vector3 edge3 = p0 - p2; // X - Z
Vector3 faceNormal = Cross(edge1, edge2); YX CROSS ZY
```

When you look at our triangle, everything is constructed out of 4 vectors. The face normal for example is constructed out of ```YX CROSS ZY```, or ```p1 - p0 CROSS p2 - p1```. So, for the axis that is the face normal, we would call the ```TestAxis``` method like so:

```cs
if (TestAxis(triangle1, triangle2, p1, p0, p2, p1)
```

Similarly, to construct the test axis out of the edges of the triangle, at some point the axis is found like this:

```cs
Vector3 axis = Cross(triangle1.edge1, triangle2.edge1)
```

This is repeated 9 times in all edge combinations. The above edge would be passed into the ```TestAxis``` function like this:

```cs
if (TestAxis(triangle1, triangle2, triangle1.p1, triangle1.p0, triangle2.p1, triangle2.p0)
```

Take note, all we're doing here is substituting.

This does mean that you can no longer hold a simple array of vectors called the testAxis and loop trough them. You now have to hold an array of 4 vectors, that was used to previously build the testAxis.

Implement this however you want, you can write it all out, or try to make a clever loop. Once you adjust your code to be a robust SAT test, the unit test for same plane triangles that has been failing until now will start to work.