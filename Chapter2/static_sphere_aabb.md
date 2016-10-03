# Sphere AABB Intersection

Testing whether a sphere intersects an axis-aligned bounding box is best done by computing the distance between the sphere center and the AABB. This is done using the "Closest Point To AABB" function, with the sphere center being the point. 

Once you have the closest point compare its distance with the radius of the sphere. ```Distance = Sphere.Position - ClosestPoint``` If the distance is less than the radius,
the sphere and AABB must be intersecting. 

To avoid expensive square root operations, both distance and radius can be squared before the comparison is made without
changing the result of the test.

## The Algorithm

Again, this is pseudo-code. The types might not match up to what you expect them to be.

```cs
bool Intersects(Sphere sphere, AABB aabb) {
    Vector3 closestPoint = ClosestPoint(aabb, sphere.Position);
    Vector3 differenceVec = sphere.Position - closestPoint;

    float distanceSquared = Vector3.LengthSquared(differenceVec);
    float radiusSquared = sphere.Radius * sphere.Radius;

    return distanceSquared < radiusSquared;
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
// TODO: Implement this
public static bool Intersects(Sphere sphere, AABB aabb)

// Just a conveniance function, so argument order wont matter!
public static bool Intersects(AABB aabb, Sphere sphere) {
    return Intersection(sphere, aabb);
}
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/StaticIntersections.rar) the samples for this chapter to see if your result looks like the unit test.

This unit tests will spit out some errors in the constructor if your intersection code is returning the wrong values. There is a blue sphere rendered and 4 cubes. 3 of the cubes intersect the sphere. Any intersecting cubes are rendered red, while non-intersecting ones are rendered blue.

![UNIT](sphere_aabb_intersection_sample.png)

```cs
using OpenTK.Graphics.OpenGL;
using Math_Implementation;
using CollisionDetectionSelector.Primitives;

namespace CollisionDetectionSelector.Samples {
    class SphereAABBIntersection : Application {
        Sphere test = new Sphere(new Point(1f, 0f, 1f), 2f);

        AABB[] aabbs = new AABB[] {
            null, null, null, null // Size = 4
        };

        public override void Intialize(int width, int height) {
            GL.Enable(EnableCap.DepthTest);
            GL.Enable(EnableCap.CullFace);
            GL.PolygonMode(MaterialFace.FrontAndBack, PolygonMode.Line);
            GL.PointSize(5f);

            aabbs[0] = new AABB(new Point(-2f, -2f, -2f), new Point(-1f, -1f, -1f));
            aabbs[1] = new AABB(new Point(2f, 1f, 2f), new Point(4f, 3f, 4f));
            aabbs[2] = new AABB(new Point(1f, 0f, 1f), new Point(0f, -1f, 0f));
            aabbs[3] = new AABB(new Point(5f, 5f, 5f), new Point(-5f, -5f, -5f));

            bool[] results = new bool[] {
                false, true, true, true
            };
            int t = 0;

            for (int i = 0; i < aabbs.Length; ++i) {
                if (Collisions.Intersects(aabbs[i], test) != results[t++]) {
                    LogError("Expected aabb " + i + " to " +
                        (results[t - 1] ? "intersect" : "not intersect") +
                    " the sphere");
                }
            }
        }

        public override void Render() {
            base.Render();
            DrawOrigin();

            for (int i = 0; i < aabbs.Length; ++i) {
                GL.Color3(0f, 0f, 1f);
                if (Collisions.Intersects(test, aabbs[i])) {
                    GL.Color3(1f, 0f, 0f);
                }
                aabbs[i].Render();
            }

            GL.Color3(0f, 0f, 1f);
            test.Render();
        }
    }
}
```