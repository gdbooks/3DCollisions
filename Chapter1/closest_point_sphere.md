# Closes Point On Sphere

Finding the closest point on a sphere can be just as useful as knowing if a point is inside a sphere or not. The closest point on sphere is often used for AI, Physics simuation and special effects.

The key to the closest point on sphere algorithm is to realize that no matter where that point may be, it will always be the same distance from the center of the sphere. That distance is equal to the radius.

Before reading the next paragraph (Because i will answer the question there), see if you can figure out given a point and a sphere how to get the closest point on the sphere to that point. I'll provide an image:

# TODO

### The algorithm

The algorithm is not as straight forward as Point in Sphere, but it's pretty simple. First, you need to subtract the point from the center of the sphere. Subtraction order matters on this one. You are left with a vector that points from the center of the sphere, to the test point.

Normalize this vector, then multiply it by the radius of the sphere. You now have a vector that points from the center of the sphere, to the edge of the sphere, towards the point in question.

Finally, add the position of the sphere (as a vector) to the new vector you have. This gives you a vector that points from the origin of the world to the edge of the sphere (A Point)

### Prototype

The sample code below is probably not needed, it's a literal translation of the paragraph above. 

```cs
// THIS BLOCK IS JUST SAMPLE CODE, DON'T COPY IT!
Point ClosestPoint(Vector3 spherePos, float sphereRad, Vector3 point) {
    // First, get a vetor from the sphere to the point
    Vector3 sphereToPoint = point - spherePos;
    // Normalize that vector
    sphereToPoint = Vector3.Normalize(sphereToPoint);
    // Adjust it's length to point to edge of sphere
    sphereToPoint *= sphereRad;
    // Translate into world space
    Vector3 worldPoint = spherePos + sphereToPoint;
    // Return new point
    return new Point(worldPoint.x, worldPoint.y, worldPoint.z);
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public Point ClosestPoint(Sphere sphere, Point point)
```

And provide an implementation for it!

### Unit Test

TODO: Gabor, Need to implement unit tests still