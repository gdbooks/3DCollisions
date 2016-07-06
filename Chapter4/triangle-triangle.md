# Triangle Triangle

As mentioned above, the triangle-triangle test is a generic form SAT test. As such

## On Your Own

Add the following private static function to your collision class, it's going to be a helper function to get the interval of the triangle. You can return the interval however you like (Make a new class, use a vector 2, use out variables), whatever makes sense to you. I did mine with a Vector2.

```cs
Vector2 GetInterval(Triangle triangle, Vector3 axis) {
    // Vector2.X = min
    // Vector2.Y = max.
}
```

Next, we need to add another private static helper function, to test two triangles on an actual axis

```cs
bool TestAxis(Triangle triangle1, Triangle triangle2, Vector3 axis) {
    // TODO: Test the intervals for overlap
}
```

Finally, add the public static function that tests 2 triangles for intersections, and provide an implementation for it!

You will need to test 11 axis, the face normal of triangle1, the face normal of triangle2, and the cross products of each of the 3 edges of the triangles. The previous section "Generic SAT" outlined how to get this information from a triangle.

```cs
public static bool Intersects(Triangle triangle1, Triangle triangle2) 
```

### Unit Test

You can [Download](../Samples/SAMPLE.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```