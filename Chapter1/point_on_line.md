# Point On Line



```cs
// This is Pseudo code, the types are not 100% correct,
// that is, casting is needed. But the formula is fully 
// implemented for demonstration purposes

// Given segment AB and Point C, compute the closest point D
// on segment AB. Also return T where D(T) = A + T * (B - A)
Point ClosestPoint(Line ab, Point c, out float t) {
  // Break ab appart into components a and b
  Vector a = c.start;
  Vector b = c.end;
  
  // Project c onto ab, computing the 
  // paramaterized position d(t) = a + t * (b - a)
  t = Dot(c - a, ab) / Dot(ab, ab);
  
  // Clamp T to a 0-1 range. If t was < 0 or > 1
  // then the closest point was outside the line!
  t = Clamp(t, 0f, 1f);
  
  // Compute the projected position from the clamped t
  Point d = a + t * ab;
  
  // Return result
  return d;
}

// Provide a useful overload, we don't always care about T
Point ClosestPoint(Line ab, Point c) {
    float t = 0f;
    return ClosestPoint(ab, c, out t);
}

```