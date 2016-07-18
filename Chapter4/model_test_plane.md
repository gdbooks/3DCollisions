# Model Plane Collision

By now you can probably see the pattern of how model collisions are done. Model plane is the same way.

You must multiply the plane normal by the inverse world matrix of the model. This is a normal, not a point, so use the ```Matrix4.MultiplyVector``` method.

Scale the distance of the plane the same way we scaled the radius of the sphere.

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
// conveniance method
public static bool Intersects(OBJ model, Plane plane) {
    return Intersects(plane, model);
}

public static bool Intersects(Plane plane, OBJ model) {
    // TODO
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