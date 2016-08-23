# Point In Frustum

Perhaps the easyest frustum test to perform is the Point In Frustum test. Actually, we've [implemented this functionality](https://gdbooks.gitbooks.io/legacyopengl/content/Chapter8/frustum.html) before. The only difference is, now we will formally include it in our collision library.

## The Algorithm

```cs
code
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static bool Intersects(Plane[] frustum, Point point) {
    // TODO: Implement this!
}

public static bool Intersects(Point point, Plane[] frustum) {
    return Intersects(frustum, point);
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