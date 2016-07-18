#Triangle model collision

At this point you should have enough practice with the collision test methods to implement this one without any help. I'm only going to provide the unit test for this.

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
// Conveniance function
public static bool Intersects(OBJ model, Triangle triangle) {
    return Intersects(triangle, model);
}


public static bool Intersects(Triangle triangle, OBJ model) {
    // TODO: Implement this one!
}
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/3DModels.rar) the samples for this chapter to see if your result looks like the unit test.

This unit test is visual only, make sure your project looks like the screenshot. Pay special attention to the ray colors, green means there was a collision, red means there was not!

![UNIT](obj_model_ray_int_unit.png)

```cs