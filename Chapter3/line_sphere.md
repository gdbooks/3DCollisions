#Line Against Sphere

In this chapter i will provide code for the wrapper functions of line against sphere. I'll provide all implementation as an example, the next few chapters i'll only provide the API and leave the implementation up to you.

# Implementation

```cs
public static bool LineTest(Line line, Sphere sphere, out Point result) {
    // First, create a ray from p0 to p1
    Ray ray = new Ray();
    ray.Position = line.start; // by reference, that's ok
    // Normal points from start to end, by value.
    // The Normal setter will automatically normalize this
    ray.Normal = (line.end.ToVector() - line.start.ToVector());

    // Not do the actual ray cast
    float t = -1;
    if (!Raycast(ray, sphere, out t)) {
        // If the raycast returns false, the point was never on the line!
        result = new Point(0f, 0f, 0f);
        return false;
    }

    // If t is less than 0, the point is behind the start point
    if (t < 0) {
        // By value! Call new to make a new point, don't want to assigne a reference
        result = new Point(line.start.ToVector());
        return false;
    }
    // Optimize by checking length squared instead of length!
    // if t is greater than the length of the line, intersection is after start point
    else if (t * t > line.LengthSquared) {
        result = new Point(line.end.ToVector()); // Again, by value!
        return false;
    }

    // If we made it here, the line intersected the sphere
    result = new Point(ray.Position.ToVector() + ray.Normal * t);
    return true;
}
````