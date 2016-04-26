#Point in Sphere

Testing if a point is inside a sphere is one of the simplest 3D tests. Take a look at this image, and see if you can figure out the algorithm to perform the test.

#TODO

The actual algorithm is very simple. If the distance from the point to the center of the sphere is less than the radius of the sphere, the sphere contains the point!

### Prototype

You could achieve this by accessing the position of the sphere and point being tested as vectors. Subtract one vector from the other (order doesn't matter) and take the length of the resulting vector.

```cs
// THIS BLOCK IS JUST SAMPLE CODE, DON'T COPY IT!
function PointInSphere(Vector3 spherePos, float sphereRad, Vector3 point) {
  Vector3 difference = spherePos - point;
  float distance = Vector3.Length(difference);
  return distance < sphereRad;
}
```

That's a start, but it's a very sub-optimal way to do things. Remember, ```Vector3.Length``` requires a square root. Which is a relativley expensive operation! Before reading the next paragraph (because i will answer this question in it), how could you avoid doing a SquareRoot?

The answer is ```LengthSquared```! It's a function we implemented that does the same thing as ```Length```, minus the square root. If you are going to check the squared length, all you have to do is bring the sphere radius into square space as well. Like so:

```cs
// THIS BLOCK IS JUST SAMPLE CODE, DON'T COPY IT!
...
  float distanceSq = Vector3.LengthSquared(difference);
  return distanceSq < sphereRad * sphereRad;
```

That, is the most optimal way to check if a sphere contains a point.