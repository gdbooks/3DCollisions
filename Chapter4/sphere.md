#Sphere

In the last section we built an AABB around out model, this actually gives us everything we need to build a bounding sphere.

The center of the sphere is the same as the center of the AABB. 

The radius of the sphere is either the distance between the center of the sphere and the min point or the max point. Take the length of both, and use the larger one.

## Creating the sphere

Add a new sphere in the ObjLoader class:

```cs
protected Sphere containerSphere = null;
```

Now, in the constructor, after you have created the AABB, make the sphere object. Follow the instructions in the introduction paragraph.

## Debug Render

Now, let's add a public getter for the sphere:

```cs
public Sphere BoundingSphere {
    get {
        return containerSphere;
    }
}
```

And modify the debug-render method to draw this box:

```cs
public void DebugRender() {
    containerAABB.Render();
    containerSphere.Render();
    foreach (Triangle trianlge in collisionMesh) {
        trianlge.Render();
    }
}
```

## Unit Test

You can [Download](../Samples/3DModels.rar) the samples for this chapter to see if your result looks like the unit test.

The constructor only errors out if the AABB or triangle count is off. Otherwise the rest of the test is visual only. You should see susane in the test

![UNIT](obj_debug_aabb.png)

```cs