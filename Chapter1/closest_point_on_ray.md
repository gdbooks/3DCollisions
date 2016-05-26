#Closest point on Ray

Finding the closest point on a ray is similar to finding the closest point on a line! When we check if a point is on a line segment, we do so by projecting the point onto an infinate line. Then, we clamp the projection to a range of 0 to 1. The 0 to 1 range is what makes the check be on a line segment.

A ray goes infinatley in one direction. If we change the formula to only cap to 0, the same formula will work! Becuase a ray has no length, the interpolated t value is useless.

Now you might think to yourself, a point has a start and end, a ray only has a start and direction, how do i get an end point?!?! Well, because we don't cap the interpolation at 1, the end point doesn't matter. It just has to be some point on the ray. You can easily get some point by adding the normal to the origin of the ray.

If the above doesn't sound familiar, re-read Closest Point On Line and make sure you understand the algorithm behind it! Call me if you have questions!

### The algorithm

The above formula will intepolate in all dimensions. Back to what i was saying, there are 3 ways to project a point (C) onto a line (AB):

![SAMPLE](point_line_projection_screen.png)

In the first and last example T would be __< 0__ and __> 1__. In the middle example T is between 0 and 1. In order for the projected point to be ON THE SEGMENT, we need to clamp T between 0 and 1. 

```cs
// This is Pseudo code, the types are not 100% correct,
// that is, casting is needed. But the formula is fully 
// implemented for demonstration purposes

// Given segment AB and Point C, compute the closest point D
// on segment AB. Also return T where D(T) = A + T * (B - A)
Point ClosestPoint(Line ab, Point c, out float t) {
  // Break ab appart into components a and b
  Vector a = ab.start;
  Vector b = ab.end;
  
  // Project c onto ab, computing the 
  // paramaterized position d(t) = a + t * (b - a)
  t = Dot(c - a, ab) / Dot(ab, ab);
  
  // Clamp T to a 0-1 range. If t was < 0 or > 1
  // then the closest point was outside the line!
  t = Clamp(t, 0f, 1f);
  
  // Compute the projected position from the clamped t
  Point d = new Point(a + t * ab.ToVector());
  
  // Return result
  return d;
}

// Provide a useful overload, we don't always care about T
Point ClosestPoint(Line ab, Point c) {
    float t = 0f;
    return ClosestPoint(ab, c, out t);
}
```