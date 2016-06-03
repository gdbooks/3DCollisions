# Sphere AABB Intersection

Testing whether a sphere intersects an axis-aligned bounding box is best done by computing the distance between the sphere center and the AABB. This is done using the "Closest Point To AABB" function, with the sphere center being the point. 

Once you have the closest point compare its distance with the radius of the sphere. ```Distance = Sphere.Position - ClosestPoint``` If the distance is less than the radius,
the sphere and AABB must be intersecting. 

To avoid expensive square root operations, both distance and radius can be squared before the comparison is made without
changing the result of the test.

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