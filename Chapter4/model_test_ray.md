# Model Raycast

To raycast against a model is similar to what we've been doing so far. Make a new ray by multiplying the position of the ray (MultiplyPoint) and the normal of the ray (MultiplyVector)  by the inverse matrix. Then, use this new ray to raycast against the models bounding sphere, bounding box and finally all it's triangles. 

When you call the raycast against the models bounding sphere or box, pass the argument t that this method recieves as the out argument. Example:

```cs
public static bool Raycast(Ray ray, OBJ model, out float t) {
    // Code to create newRay
    if (!Raycast(newRay, model.BoundingSphere, out t)) {
        return false;
    }
    // rest of function
}
```

## The Algorithm

```cs
code
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
code
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/SAMPLE.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```