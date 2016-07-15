#Sphere

In the last section we built an AABB around out model, this actually gives us everything we need to build a bounding sphere.
The center of the sphere is the same as the center of the AABB. The radius of the sphere is either the min or max, whichever point is further away from the center of the sphere.

## Creating the sphere

Add a new sphere in the ObjLoader class:

```cs
protected Sphere containerSphere = null;
```

Now, in the constructor, after you have created the AABB, make the sphere object. Follow the instructions in the introduction paragraph.