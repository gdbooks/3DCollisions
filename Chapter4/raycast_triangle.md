# Raycast Triangle

Raycsating against a triangle is a simple process

* Create a plane from the 3 points of the triangle
* Raycast against that plane
* Check if the raycast point is inside the triangle

You already have all the fnuctions needed to implement this in a rudimentary way. However, our Point In Triangle test is not optimal! Remember, when we did the point in triangle test, we did not use barycentric coordinates.

It's time to switch that. You can either implement barycentric coordinates straight up in the raycast triangle code, or you can change your point in triangle code and call that. If you change the point in triangle equation, make sure to run the unit test again to ensure everything still works.

Onto implementing barycentric coordinates! Watch this video:

{% youtube %}https://www.youtube.com/watch?v=EZXz-uPyCyA{% endyoutube %}

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static bool Raycast(Ray ray, Triangle triangle, out float t) {
    // Implement your code here
}

// Conveniance method, returns t without an out param
// If no collision happened, will return -1
public static float Raycast(Ray ray, Triangle triangle) {
    float t = -1;
    if (!Raycast(ray, plane, out t)) {
        return -1;
    }
    return t;
}

// Conveniance method, returns the point of intersection
public static bool Raycast(Ray ray, Triangle triangle, out Point p) {
    float t = -1;
    bool result = Raycast(ray, plane, out t);
    p = new Point(ray.Position.ToVector() + ray.Normal * t);
    return result;
}
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/SAMPLE.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```