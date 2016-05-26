#Closest point on Ray

Finding the closest point on a ray is similar to finding the closest point on a line! When we check if a point is on a line segment, we do so by projecting the point onto an infinate line. Then, we clamp the projection to a range of 0 to 1. The 0 to 1 range is what makes the check be on a line segment.

A ray goes infinatley in one direction. If we change the formula to only cap to 0, the same formula will work! Becuase a ray has no length, the interpolated t value is useless.

Now you might think to yourself, a point has a start and end, a ray only has a start and direction, how do i get an end point?!?! Well, because we don't cap the interpolation at 1, the end point doesn't matter. It just has to be some point on the ray. You can easily get some point by adding the normal to the origin of the ray.

If the above doesn't sound familiar, re-read Closest Point On Line and make sure you understand the algorithm behind it! Call me if you have questions!

### The algorithm

Again, it's so similar to the closest point on line algorithm, i just copied it and made some subtle changes. You shouldn't even need this guide, you should be able to make the same changes based on the above paragraph.

```cs
// This is Pseudo code, the types are not 100% correct,
// that is, casting is needed. But the formula is fully 
// implemented for demonstration purposes

Point ClosestPoint(Ray r, Point c) {
  // t is now local, there is no need to pass it out of the function
  float t = 0f;
  // Construct a line segment out of the ray
  Line ab = new Line(r.Position, r.Position + r.Normal);
  // Break ray into a (start) and b (end) components
  Vector a = r.Position;
  Vector b = r.Position + r.Normal;
  
  // Project c onto ab, computing the 
  // paramaterized position d(t) = a + t * (b - a)
  t = Dot(c - a, ab) / Dot(ab, ab);
  
  // We only want to clamp t in the positive direction.
  // The ray extends infinatley in this direction!
  t = Max(t, 0f);
  
  // Compute the projected position from the clamped t
  Point d = new Point(a + t * ab.ToVector());
  
  // Return result
  return d;
}
```

## On Your Own

Add the following functions to the ```Collisions``` class:

```cs
public static Point ClosestPoint(Line ab, Point c, out float t)
public static Point ClosestPoint(Line ab, Point c)
```

And provide an implementation for them! The second function can just call the first function. It's for conveniance, because we don't always care about t.

### Unit Test

You can [Download](../Samples/CollisionRay.rar) the samples for this chapter to see if your result looks like the unit test.

This code doesn't spit out any errors. It's all visual. There is 1 ray and several points. The points are projected to the ray and lines are drawn between the points.

![UNIT](TODO)

```cs
TODO
```