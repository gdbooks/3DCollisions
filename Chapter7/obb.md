
#OBB

An OBB, or Oriented Bounding box is essentially an AABB with a rotation. An OBB structure often looks like this:

```cs
class OBB {
    Vector3 Extents;
    Point Position;
    Matrix3 Orientation;
}
```

We've actually written most OBB collision tests already.

To test against collision, you take whatever shape the OBB is being tested against, and bring it into the local space of the OBB. You do this by multiplying the shape with the inverse of the OBB's Rotation Matrix. 

Once the Other object is local to the OBB, in the OBB's view it's not an AABB. So, you just do an AABB to whatever test.

The one exception to this is of course OBB-OBB collisions. The best you can do is turn that into an OBB v AABB test. Once you have an OBB and an AABB, you test for collision with a seperating axis therom test.