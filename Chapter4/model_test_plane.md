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

You can [Download](../Samples/3DModels.rar) the samples for this chapter to see if your result looks like the unit test.

This unit test is visual only, make sure your project looks like the screenshot. Pay special attention to the plane colors, green means there was a collision, red means there was not!

![UNIT](obj_model_plane_int_unit.png)

```cs
code
```