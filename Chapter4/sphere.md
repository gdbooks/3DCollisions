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