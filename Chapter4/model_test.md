# Collision testing

In the last section of this document we added collision triangles to a model, a bounding box and a bounding sphere. Hopefully the debug render function we added will show the accuracy of the collision testing we do against the model at each level.

## Spaces

The collision tests against a model are going to be a bit different than those against primitives, this is because the model is located in it's own model space. Model space is always located at the origin. So long as we render a model like this:

```cs
public override void Render() {
    base.Render();
    DrawOrigin();
    
    obj.Render();
}
```

Everything will be fine, because the model is at origin. But as soon as we scale, rotate or translate the model:

```cs
public override void Render() {
    base.Render();
    DrawOrigin();

    GL.PushMatrix();
    GL.Translate(1f, 0f, 0f);
    GL.Scale(3.0f, 3.0f, 3.0f);
    obj.DebugRender();
    GL.PopMatrix();
}
```

Now the collisions wont work. Visually, the model looks different than how it's represented in memory. And all of the bounding primitives (triangles, AABB, Sphere) that we built around it, we built around the model in memory, not the visual model.

Luckily, we can solve this using matrix operations. The translation-rotation-scale of a model can be used to create a transform matrix. This matrix represents where in world space the model is. If we take the inverse of that matrix, and multiply any point by it, the point will be transformed so that the model is at the origin of the points space.

This last bit can get a tad confusing, especially since we haven't done any matrix math in a while. If you need any help on any of it, give me a call on skype.