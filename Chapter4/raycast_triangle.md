# Raycast Triangle

Raycsating against a triangle is a simple process

* Create a plane from the 3 points of the triangle
* Raycast against that plane
* Check if the raycast point is inside the triangle

You already have all the fnuctions needed to implement this in a rudimentary way. However, our Point In Triangle test is not optimal! Also, we will have to make a special version of raycast plane!


### Barycentric Point In Tirangle

Remember, when we did the point in triangle test, we did not use barycentric coordinates.

It's time to switch that. You can either implement barycentric coordinates straight up in the raycast triangle code, or you can change your point in triangle code and call that. If you change the point in triangle equation, make sure to run the unit test again to ensure everything still works.

Onto implementing barycentric coordinates! Watch this video:

{% youtube %}https://www.youtube.com/watch?v=EZXz-uPyCyA{% endyoutube %}

### Special Plane Raycast

Next up, the raycast plane function we have will not work. This is because that function takes the planes normal into consideration. I suggest making a new private function:

```cs
private static bool RaycastNoNormal(Ray ray, Plane plane, out float t) {
```

Inside the plane raycast code, we do a dot product of plane normal and ray normal, store the result in nd. We later test if the normals are pointing in the same direction by doing this bit:

```cs
if (nd >= 0f) {
    t = -1;
    return false;
}
```

Thats where the problem is, the only time we want the ray and the plane to not intercept is when they are parallel, that is when nd is 0. Remember, use an epsilon. This will take the direction of the plane and ray needing to be the same out of the equation.

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static bool Raycast(Ray ray, Triangle triangle, out float t) {
    // Implement your code here
}

// Conveniance method, returns t without an out param
// If no collision happened, will return -1
public static float Raycast(Ray ray, Triangle triangle) {
    float t = -1;
    if (!Raycast(ray, triangle, out t)) {
        return -1;
    }
    return t;
}

// Conveniance method, returns the point of intersection
public static bool Raycast(Ray ray, Triangle triangle, out Point p) {
    float t = -1;
    bool result = Raycast(ray, triangle, out t);
    p = new Point(ray.Position.ToVector() + ray.Normal * t);
    return result;
}
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/3DModels.rar) the samples for this chapter to see if your result looks like the unit test.

A trianlge and a few rays are rendered. Any ray intersecting the triangle is red. All rays not intersecting the triangle are blue. Green dots are rendered at the points of intersection. The constructor of this unit test will spit out errors if it finds any 

![UNIT](unit_triangle_ray_intersection.png)

```cs
//#define ROBUST_SAT
using System;
using Math_Implementation;
using CollisionDetectionSelector.Primitives;

// https://graphics.stanford.edu/~mdfisher/Code/Engine/Plane.cpp.html
// http://www.had2know.com/academics/equation-plane-through-3-points.html

class Collisions {
    public static bool PointInSphere(Sphere sphere, Point point) {
        Vector3 distance = sphere.Position.ToVector() - point.ToVector();
        return Vector3.LengthSquared(distance) < sphere.Radius * sphere.Radius;
    }

    public static bool PointInAABB(AABB aabb, Point point) {
        return  (point.X >= aabb.Min.X && point.X <= aabb.Max.X) &&
                (point.Y >= aabb.Min.Y && point.Y <= aabb.Max.Y) &&
                (point.Z >= aabb.Min.Z && point.Z <= aabb.Max.Z);
    }

    public static bool TempSameSide(Vector3 p1, Vector3 p2, Vector3 a, Vector3 b) {
        Vector3 cp1 = Vector3.Cross(b - a, p1 - a);
        Vector3 cp2 = Vector3.Cross(b - a, p2 - a);
        if (Vector3.Dot(cp1, cp2) >= 0) {
            return true;
        }
        return false;
    }

    public static bool PointInTriangle(Triangle triangle, Point point) {
        // Let the point be represented by vector p
        Vector3 p = point.ToVector();
        // and the triangle be represented by vectors A, B and C
        Vector3 a = triangle.p0.ToVector();
        Vector3 b = triangle.p1.ToVector();
        Vector3 c = triangle.p2.ToVector();


        /// TEMP
        /*Plane plane = new Plane(triangle.p0, triangle.p1, triangle.p2);
        return Collisions.PointOnPlane(point, plane) && 
            TempSameSide(p, a, b, c) && 
            TempSameSide(p, b, a, c) && 
            TempSameSide(p, c, a, b);*/
        /// END TEMP

        // Translate the triangle ABC so that it is centered around P
        a -= p;
        b -= p;
        c -= p;

        // Compute the normal for traignle PAB
        Vector3 u = Vector3.Cross(b, c);
        // Compute the normal for triangle PBC
        Vector3 v = Vector3.Cross(c, a);

        // Make sure normals point in same direction
        if (Vector3.Dot(u, v) < 0.0f) {
            return false;
        }

        // Compute the normal vector for triangle PCA
        Vector3 w = Vector3.Cross(a, b);
        // Make sure it points in same direction as first two
        if (Vector3.Dot(u, w) < 0.0f) {
            return false;
        }

        // Otherwise p is in or on the triangle!
        return true;
    }

    public static bool PointOnPlane(Point point, Plane plane) {
        float result = plane.Normal.X * point.X + plane.Normal.Y * point.Y + plane.Normal.Z * point.Z;
        return Math.Abs(plane.Distance - result) < 0.00001f;
    }

    public static bool PointOnLine(Point point, Line line) {
        float m = (line.end.Y - line.start.Y) / (line.end.X - line.start.X);
        float b = line.start.Y - m * line.start.X;
        return (Math.Abs(point.Y - (m * point.X + b)) < 0.00001f);
    }

    public static bool PointOnRay(Point point, Ray ray) {
        if (point.ToVector() == ray.Position.ToVector()) {
            return true;
        }
        Vector3 newNorm = point.ToVector() - ray.Position.ToVector();
        newNorm.Normalize();
        float d = Vector3.Dot(newNorm, ray.Normal);
        return Math.Abs(1f - d) < 0.000001f; // SUPER SMALL EPSILON!
    }

    // Should return a positive number, a negative number or 0
    public static float DistanceFromPlane(Point point, Plane plane) {
        return plane.Normal.X * point.X + plane.Normal.Y * point.Y + plane.Normal.Z * point.Z - plane.Distance;
    }

    public static Point ClosestPoint(Triangle triangle, Point point) {
        Plane plane = new Plane(triangle.p0, triangle.p1, triangle.p2);
        point = ClosestPoint(plane, point);

        if (PointInTriangle(triangle, point)) {
            return new Point(point.ToVector());
        }

        Line AB = new Line(triangle.p0, triangle.p1);
        Line BC = new Line(triangle.p1, triangle.p2);
        Line CA = new Line(triangle.p2, triangle.p0);

        Point c1 = ClosestPoint(AB, point);
        Point c2 = ClosestPoint(BC, point);
        Point c3 = ClosestPoint(CA, point);

        float mag1 = (point.ToVector() - c1.ToVector()).LengthSquared();
        float mag2 = (point.ToVector() - c2.ToVector()).LengthSquared();
        float mag3 = (point.ToVector() - c3.ToVector()).LengthSquared();

        float min = Math.Min(mag1, mag2);
        min = Math.Min(min, mag3);

        if (min == mag1) {
            return c1;
        }
        else if (min == mag2) {
            return c2;
        }
        return c3;
    }

    public static Point ClosestPoint(Ray r, Point c) {
        Line ab = new Line(r.Position, new Point(r.Position.ToVector() + r.Normal));
        Vector3 a = r.Position.ToVector();
        Vector3 b = r.Position.ToVector() + r.Normal;
        float t = Vector3.Dot(c.ToVector() - a, ab.ToVector()) / Vector3.Dot(ab.ToVector(), ab.ToVector());
        t = Math.Max(t, 0f);
        return new Point(a + r.Normal * t);
    }

    public static Point ClosestPoint(Line ab, Point c, out float t) {
        Vector3 a = ab.start.ToVector();
        Vector3 b = ab.end.ToVector();
        t = Vector3.Dot(c.ToVector() - a, ab.ToVector()) / Vector3.Dot(ab.ToVector(), ab.ToVector());
        t = Clamp(t, 0f, 1f);
        return new Point(a + ab.ToVector() * t);
    }

    public static Point ClosestPoint(Line ab, Point c) {
        float t = 0f;
        return ClosestPoint(ab, c, out t);
    }

    public static Point Intersection(Plane p1, Plane p2, Plane p3) {
        Vector3 m1 = new Vector3(p1.Normal.X, p2.Normal.X, p3.Normal.X);
        Vector3 m2 = new Vector3(p1.Normal.Y, p2.Normal.Y, p3.Normal.Y);
        Vector3 m3 = new Vector3(p1.Normal.Z, p2.Normal.Z, p3.Normal.Z);
        Vector3 d = new Vector3(p1.Distance, p2.Distance, p3.Distance);

        Vector3 u = Vector3.Cross(m2, m3);
        Vector3 v = Vector3.Cross(m1, d);

        float denom = Vector3.Dot(m1, u);

        if (Math.Abs(denom) < 0.00001f) {
            // Planes don't actually intersect in a point
            // Throw exception maybe?
            return new Point(0, 0, 0);
        }

        return new Point(
            Vector3.Dot(d, u) / denom,
            Vector3.Dot(m3, v) / denom,
            -Vector3.Dot(m2, v) / denom
        );
    }

    public static Point ClosestPoint(Plane plane, Point point) {
        return new Point(point.ToVector() - plane.Normal * DistanceFromPlane(point, plane));
    }

    public static Point ClosestPoint(Sphere sphere, Point point) {
        Vector3 sphereToPoint = point.ToVector() - sphere.Position.ToVector();
        sphereToPoint = Vector3.Normalize(sphereToPoint);
        sphereToPoint *= sphere.Radius; ;
        Vector3 worldPoint = sphere.Position.ToVector() + sphereToPoint;
        return new Point(worldPoint.X, worldPoint.Y, worldPoint.Z);
    }

    public static Point ClosestPoint(AABB aabb, Point point) {
        Point result = new Point(point.X, point.Y, point.Z);
        result.X = Clamp(result.X, aabb.Min.X, aabb.Max.X);
        result.Y = Clamp(result.Y, aabb.Min.Y, aabb.Max.Y);
        result.Z = Clamp(result.Z, aabb.Min.Z, aabb.Max.Z);
        return result;
    }

#if ROBUST_SAT
    protected class AxisPrototype {
        Vector3 A;
        Vector3 B;
        Vector3 C;
        Vector3 D;

        public AxisPrototype(Vector3 a, Vector3 b, Vector3 c, Vector3 d) {
            A = a;
            B = b;
            C = c;
            D = d;
        }

        public Vector3 AB {
            get {
                return A - B;
            }
        }

        public Vector3 CD {
            get {
                return C - D;
            }
        }

        public Vector3 CA {
            get {
                return C - A;
            }
        }

        public bool GetAxis(out Vector3 result) {
            result = Vector3.Cross(AB, CD);

            if (result.LengthSquared() < 0.0001f) {
                Vector3 n = Vector3.Cross(AB, CA);
                result = Vector3.Cross(AB, n);
                if (result.LengthSquared() < 0.0001f) {
                    return false;
                }
            }

            return true;
        }
    }

    public static bool Intersects(Triangle triangle1, Triangle triangle2) {
        Vector3 t1_v0 = triangle1.p0.ToVector();
        Vector3 t1_v1 = triangle1.p1.ToVector();
        Vector3 t1_v2 = triangle1.p2.ToVector();

        Vector3 t2_v0 = triangle2.p0.ToVector();
        Vector3 t2_v1 = triangle2.p1.ToVector();
        Vector3 t2_v2 = triangle2.p2.ToVector();

        AxisPrototype[] axisToTest = new AxisPrototype[] {
            new AxisPrototype(t1_v1, t1_v0, t1_v2, t1_v1),
            new AxisPrototype(t2_v1, t2_v0, t2_v2, t2_v1),

            new AxisPrototype(t2_v1, t2_v0, t1_v1, t1_v0),
            new AxisPrototype(t2_v1, t2_v0, t1_v2, t1_v1),
            new AxisPrototype(t2_v1, t2_v0, t1_v0, t1_v2),

            new AxisPrototype(t2_v2, t2_v1, t1_v1, t1_v0),
            new AxisPrototype(t2_v2, t2_v1, t1_v2, t1_v1),
            new AxisPrototype(t2_v2, t2_v1, t1_v0, t1_v2),

            new AxisPrototype(t2_v0, t2_v2, t1_v1, t1_v0),
            new AxisPrototype(t2_v0, t2_v2, t1_v2, t1_v1),
            new AxisPrototype(t2_v0, t2_v2, t1_v0, t1_v2),
        };

        foreach(AxisPrototype axis in axisToTest) {
            if (TriangleTriangleSAT(triangle1, triangle2, axis)) {
                return false;
            }
        }

        return true;

    }

    protected static bool TriangleTriangleSAT(Triangle triangle1, Triangle triangle2, AxisPrototype axisProto) {
        Vector3 axis = new Vector3();
        if (!axisProto.GetAxis(out axis)) {
            return false; // Not a seperating axis
        }

        Vector2 range1 = GetTriangleInterval(triangle1, axis);
        Vector2 range2 = GetTriangleInterval(triangle2, axis);

        float min1 = range1.X;
        float max1 = range1.Y;
        float min2 = range2.X;
        float max2 = range2.Y;

        if (max1 < min2 || max2 < min1) {
            return true;
        }

        /*if (range1.Y < range2.X || range2.Y < range1.X) {
            return true;
        }*/

        return false;
    }
#else
    public static bool Intersects(Triangle triangle1, Triangle triangle2) {
        Vector3 t1_v0 = triangle1.p0.ToVector();
        Vector3 t1_v1 = triangle1.p1.ToVector();
        Vector3 t1_v2 = triangle1.p2.ToVector();

        Vector3 t1_f0 = t1_v1 - t1_v0; // B - A
        Vector3 t1_f1 = t1_v2 - t1_v1; // C - B
        Vector3 t1_f2 = t1_v0 - t1_v2; // A - C

        Vector3 t2_v0 = triangle2.p0.ToVector();
        Vector3 t2_v1 = triangle2.p1.ToVector();
        Vector3 t2_v2 = triangle2.p2.ToVector();

        Vector3 t2_f0 = t2_v1 - t2_v0; // B - A
        Vector3 t2_f1 = t2_v2 - t2_v1; // C - B
        Vector3 t2_f2 = t2_v0 - t2_v2; // A - C


        Vector3[] axisToTest = new[] {
            Vector3.Cross(t1_f0, t1_f1),
            Vector3.Cross(t2_f0, t1_f1),

            Vector3.Cross(t2_f0, t1_f0),
            Vector3.Cross(t2_f0, t1_f1),
            Vector3.Cross(t2_f0, t1_f2),

            Vector3.Cross(t2_f1, t1_f0),
            Vector3.Cross(t2_f1, t1_f1),
            Vector3.Cross(t2_f1, t1_f2),

            Vector3.Cross(t2_f2, t1_f0),
            Vector3.Cross(t2_f2, t1_f1),
            Vector3.Cross(t2_f2, t1_f2),
        };

        foreach (Vector3 axis in axisToTest) {
            if (TriangleTriangleSAT(triangle1, triangle2, axis)) {
                return false;
            }
        }

        return true;
    }

    protected static bool TriangleTriangleSAT(Triangle triangle1, Triangle triangle2, Vector3 raw_axis) {
        Vector3 axis = Vector3.Normalize(raw_axis);

        Vector2 range1 = GetTriangleInterval(triangle1, axis);
        Vector2 range2 = GetTriangleInterval(triangle2, axis);

        float min1 = range1.X;
        float max1 = range1.Y;
        float min2 = range2.X;
        float max2 = range2.Y;

        if (max1 < min2 || max2 < min1) {
            return true;
        }

        return false;
    }
#endif

    protected static Vector2 GetTriangleInterval(Triangle triangle, Vector3 axis) {
        Vector3[] vertices = new Vector3[] {
            triangle.p0.ToVector(),
            triangle.p1.ToVector(),
            triangle.p2.ToVector()
        };
        float min = Vector3.Dot(axis, vertices[0]);
        float max = min;

        for (int i = 0; i < vertices.Length ; ++i) {
            float value = Vector3.Dot(axis, vertices[i]);
            min = Min(min, value);
            max = Max(max, value);
        }

        return new Vector2(min, max);
    }


    public static bool Intersects(Triangle triangle, AABB aabb) {
        // Get the triangle points as vectors
        Vector3 v0 = triangle.p0.ToVector();
        Vector3 v1 = triangle.p1.ToVector();
        Vector3 v2 = triangle.p2.ToVector();

        // Convert AABB to center-extents form
        Vector3 c = aabb.Center.ToVector();
        Vector3 e = aabb.Extents;

        // Translate the triangle as conceptually moving the AABB to origin
        // This is the same as we did with the point in triangle test
        v0 -= c;
        v1 -= c;
        v2 -= c;

        // Compute the edge vectors of the triangle  (ABC)
        // That is, get the lines between the points as vectors
        Vector3 f0 = v1 - v0; // B - A
        Vector3 f1 = v2 - v1; // C - B
        Vector3 f2 = v0 - v2; // A - C

        // Compute the face normals of the AABB, because the AABB
        // is at center, and of course axis aligned, we know that 
        // it's normals are the X, Y and Z axis.
        Vector3 u0 = new Vector3(1.0f, 0.0f, 0.0f);
        Vector3 u1 = new Vector3(0.0f, 1.0f, 0.0f);
        Vector3 u2 = new Vector3(0.0f, 0.0f, 1.0f);

        // There are a total of 13 axis to test!

        // We first test against 9 axis, these axis are given by
        // cross product combinations of the edges of the triangle
        // and the edges of the AABB. You need to get an axis testing
        // each of the 3 sides of the AABB against each of the 3 sides
        // of the triangle. The result is 9 axis of seperation
        // https://awwapp.com/b/umzoc8tiv/

        // Compute the 9 axis
        Vector3 axis_u0_f0 = Vector3.Cross(u0, f0);
        Vector3 axis_u0_f1 = Vector3.Cross(u0, f1);
        Vector3 axis_u0_f2 = Vector3.Cross(u0, f2);

        Vector3 axis_u1_f0 = Vector3.Cross(u1, f0);
        Vector3 axis_u1_f1 = Vector3.Cross(u1, f1);
        Vector3 axis_u1_f2 = Vector3.Cross(u2, f2);

        Vector3 axis_u2_f0 = Vector3.Cross(u2, f0);
        Vector3 axis_u2_f1 = Vector3.Cross(u2, f1);
        Vector3 axis_u2_f2 = Vector3.Cross(u2, f2);

        // Project all 3 vertices of the triangle onto the Seperating axis
        float p0 = Vector3.Dot(v0, axis_u0_f0);
        float p1 = Vector3.Dot(v1, axis_u0_f0);
        float p2 = Vector3.Dot(v2, axis_u0_f0);
        // Project the AABB onto the seperating axis
        float r = e.X * Math.Abs(Vector3.Dot(u0, axis_u0_f0)) +
                    e.Y * Math.Abs(Vector3.Dot(u1, axis_u0_f0)) +
                    e.Z * Math.Abs(Vector3.Dot(u2, axis_u0_f0));
        // Now do the actual test
        if (Max(-Max(p0, p1, p2), Min(p0, p1, p2)) > r) {
            return false;
        }

        if (!SATTest(v0, v1, v2, e, axis_u0_f1)) {
            return false;
        }
        if (!SATTest(v0, v1, v2, e, axis_u0_f2)) {
            return false;
        }

        if (!SATTest(v0, v1, v2, e, axis_u1_f0)) {
            return false;
        }
        if (!SATTest(v0, v1, v2, e, axis_u1_f1)) {
            return false;
        }
        if (!SATTest(v0, v1, v2, e, axis_u1_f2)) {
            return false;
        }

        if (!SATTest(v0, v1, v2, e, axis_u2_f0)) {
            return false;
        }
        if (!SATTest(v0, v1, v2, e, axis_u2_f1)) {
            return false;
        }
        if (!SATTest(v0, v1, v2, e, axis_u2_f2)) {
            return false;
        }

        // Next, we have 3 face normals from the AABB
        // for these tests we are conceptually checking if the bounding box
        // of the triangle intersects the bounding box of the AABB
        // that is to say, the seperating axis for all tests are axis aligned:
        // axis1: (1, 0, 0), axis2: (0, 1, 0), axis3 (0, 0, 1)

        if (!SATTest(v0, v1, v2, e, new Vector3(1.0f, 0.0f, 0.0f))) {
            return false;
        }
        if (!SATTest(v0, v1, v2, e, new Vector3(0.0f, 1.0f, 0.0f))) {
            return false;
        }
        if (!SATTest(v0, v1, v2, e, new Vector3(0.0f, 0.0f, 1.0f))) {
            return false;
        }

        // Finally, we have one last axis to test, the face normal of the triangle
        // We can get the normal of the triangle by crossing the first two line segments
        if (!SATTest(v0, v1, v2, e, Vector3.Cross(f0, f1))) {
            return false;
        }

        // Passed testing for all 13 seperating axis that exist!
        return true;
    }

    public static bool SATTest(Vector3 v0, Vector3 v1, Vector3 v2, Vector3 extents, Vector3 axis) {
        // These are a constant
        Vector3 u0 = new Vector3(1.0f, 0.0f, 0.0f);
        Vector3 u1 = new Vector3(0.0f, 1.0f, 0.0f);
        Vector3 u2 = new Vector3(0.0f, 0.0f, 1.0f);

        // Project all 3 vertices of the triangle onto the Seperating axis
        float p0 = Vector3.Dot(v0, axis);
        float p1 = Vector3.Dot(v1, axis);
        float p2 = Vector3.Dot(v2, axis);
        // Project the AABB onto the seperating axis
        float r = extents.X * Math.Abs(Vector3.Dot(u0, axis)) +
                  extents.Y * Math.Abs(Vector3.Dot(u1, axis)) +
                  extents.Z * Math.Abs(Vector3.Dot(u2, axis));
        // Now do the actual test
        if (Max(-Max(p0, p1, p2), Min(p0, p1, p2)) > r) {
            return false;
        }
        return true;
    }

    public static float Max(float f1, float f2, float f3) {
        float max = f1;
        if (f2 > max) {
            max = f2;
        }
        if (f3 > max) {
            max = f3;
        }
        return max;
    }

    public static float Min(float f1, float f2, float f3) {
        float min = f1;
        if (f2 < min) {
            min = f2;
        }
        if (f3 < min) {
            min = f3;
        }
        return min;
    }

    public static bool Intersects(AABB aabb, Triangle triangle) {
        return Intersects(triangle, aabb);
    }

    public static bool Intersects(Triangle triangle, Sphere sphere) {
        Point closest = ClosestPoint(triangle, sphere.Position);
        Vector3 diff = closest.ToVector() - sphere.Position.ToVector();

        return diff.LengthSquared() <= sphere.Radius * sphere.Radius;
    }

    public static bool Intersects(Sphere sphere, Triangle triangle) {
        return Intersects(triangle, sphere);
    }

    public static bool Intersects(Sphere s1, Sphere s2) {
        Vector3 d = s1.Position.ToVector() - s2.Position.ToVector();
        float squaredDistance = d.LengthSquared();

        float radiusSum = s1.Radius + s2.Radius;
        float squaredRadii = radiusSum * radiusSum;

        return squaredDistance <= squaredRadii;
    }

    public static bool Intersects(Sphere sphere, AABB aabb) {
        Vector3 closestPoint = ClosestPoint(aabb, sphere.Position).ToVector();
        Vector3 differenceVec = sphere.Position.ToVector() - closestPoint;

        float distanceSquared = Vector3.LengthSquared(differenceVec);
        float radiusSquared = sphere.Radius * sphere.Radius;

        return distanceSquared < radiusSquared;
    }

    public static bool Intersects(AABB aabb, Plane plane) {
        Vector3 c = (aabb.Max.ToVector() + aabb.Min.ToVector()) * 0.5f;
        Vector3 e = aabb.Max.ToVector() - c;
        float r = e[0] * Abs(plane.Normal[0]) + e[1] * Abs(plane.Normal[1]) + e[2] * Abs(plane.Normal[2]);
        float s = Vector3.Dot(plane.Normal, c) - plane.Distance;
        return Abs(s) <= r;
        /*Vector3 min = aabb.Min.ToVector();
        Vector3 max = aabb.Max.ToVector();
        Vector3[] points = new Vector3[] {
            // Min side
            new Vector3(min.X, min.Y, min.Z),
            new Vector3(max.X, min.Y, min.Z),
            new Vector3(min.X, max.Y, min.Z),
            new Vector3(min.X, min.Y, max.Z),
            // Max side
            new Vector3(max.X, max.Y, max.Z),
            new Vector3(min.X, max.Y, max.Z),
            new Vector3(max.X, min.Y, max.Z),
            new Vector3(max.X, max.Y, min.Z)
        };

        float side = DistanceFromPlane(new Point(min), plane);
        for (int i = 0; i < points.Length; ++i) {
            float dist = DistanceFromPlane(new Point(points[i]), plane);
            if (side < 0.0f && dist > 0.0f) {
                return true;
            }
            else if (side > 0.0f && dist < 0.0f) {
                return true;
            }
        }

        return false;*/
    }

    public static bool Intersects(Plane plane, AABB aabb) {
        return Intersects(aabb, plane);
    }

    public static float Abs(float f) {
        return Math.Abs(f);
    }

    public static bool Intersects(AABB a, AABB b) {
        return (a.minX <= b.maxX && a.maxX >= b.minX) &&
         (a.minY <= b.maxY && a.maxY >= b.minY) &&
         (a.minZ <= b.maxZ && a.maxZ >= b.minZ);
    }

    public static bool Intersects(Sphere sphere, Plane plane) {
        Point closestPoint = ClosestPoint(plane, sphere.Position);
        Vector3 diff = sphere.Position.ToVector() - closestPoint.ToVector();
        return Vector3.LengthSquared(diff) <= sphere.Radius * sphere.Radius;
    }

    // Conveniance function
    public static bool Intersects(Plane plane, Sphere sphere) {
        return Intersects(sphere, plane);
    }

    public static float SquareDistance(AABB aabb, Point point) {
        float sqDist = 0.0f;
        Vector3 minVec = aabb.Min.ToVector();
        Vector3 maxVec = aabb.Max.ToVector();

        for (int i = 0; i < 3; i++) {
            // For each axis count any excess distance outside box extents
            float v = point.ToVector()[i];
            if (v < minVec[i]) {
                sqDist += (minVec[i] - v) * (minVec[i] - v);
            }
            if (v > maxVec[i]) {
                sqDist += (v - maxVec[i]) * (v - maxVec[i]);
            }
        }
        return sqDist;
    }

    public static bool Intersects(Triangle triangle, Plane plane) {
        float side1 = DistanceFromPlane(triangle.p0, plane);
        float side2 = DistanceFromPlane(triangle.p1, plane);
        float side3 = DistanceFromPlane(triangle.p2, plane);

        if (side1 == side2 && side2 == side3 && side1 == 0.0f) {
            return true;
        }

        if (side1 > 0f && side2 > 0f && side3 > 0f) {
            return false;
        }

        if (side1 < 0f && side2 < 0f && side3 < 0f) {
            return false;
        }

        return true;
    }

    public static bool Intersects(Plane plane, Triangle triangle) {
        return Intersects(triangle, plane);
    }

    public static bool Intersects(AABB aabb, Sphere sphere) {
        return Intersects(sphere, aabb);
    }

    public static bool Intersects(Plane p1, Plane p2) {
        Vector3 d = Vector3.Cross(p1.Normal, p2.Normal);

        return (Vector3.Dot(d, d) > 0.0001f);
    }

    public static bool LineTest(Line line, Plane plane, out Point result) {
        Ray ray = new Ray();
        ray.Position = line.start;
        ray.Normal = (line.end.ToVector() - line.start.ToVector());

        float t = -1;
        if (!Raycast(ray, plane, out t)) {
            result = new Point(0f, 0f, 0f);
            return false;
        }

        if (t < 0) {
            result = new Point(line.start.ToVector());
            return false;
        }
        else if (t * t > line.LengthSquared) {
            result = new Point(line.end.ToVector());
            return false;
        }

        result = new Point(ray.Position.ToVector() + ray.Normal * t);
        return true;
    }

    public static bool LineTest(Line line, AABB aabb, out Point result) {
        Ray ray = new Ray();
        ray.Position = line.start;
        ray.Normal = (line.end.ToVector() - line.start.ToVector());

        float t = -1;
        if (!Raycast(ray, aabb, out t)) {
            result = new Point(0f, 0f, 0f);
            return false;
        }

        if (t < 0) {
            result = new Point(line.start.ToVector());
            return false;
        }
        else if (t * t > line.LengthSquared) {
            result = new Point(line.end.ToVector()); 
            return false;
        }

        result = new Point(ray.Position.ToVector() + ray.Normal * t);
        return true;
    }

    public static bool LineTest(Line line, Sphere sphere, out Point result) {
        // First, create a ray from p0 to p1
        Ray ray = new Ray();
        ray.Position = line.start; // by reference, that's ok
        // Normal points from start to end, by value.
        // The Normal setter will automatically normalize this
        ray.Normal = (line.end.ToVector() - line.start.ToVector());

        // Not do the actual ray cast
        float t = -1;
        if (!Raycast(ray, sphere, out t)) {
            // If the raycast returns false, the point was never on the line!
            result = new Point(0f, 0f, 0f);
            return false;
        }

        // If t is less than 0, the point is behind the start point
        if (t < 0) {
            // By value! Call new to make a new point, don't want to assigne a reference
            result = new Point(line.start.ToVector());
            return false;
        }
        // Optimize by checking length squared instead of length!
        // if t is greater than the length of the line, intersection is after start point
        else if (t * t > line.LengthSquared) {
            result = new Point(line.end.ToVector()); // Again, by value!
            return false;
        }

        // If we made it here, the line intersected the sphere
        result = new Point(ray.Position.ToVector() + ray.Normal * t);
        return true;
    }

    public static bool Raycast(Ray ray, Triangle triangle, out float t) {
        Plane plane = new Plane(triangle.p0, triangle.p1, triangle.p2);

        if (!RaycastNoNormal(ray, plane, out t)) {
            t = -1;
            return false;
        }

        Point result = new Point(ray.Position.ToVector() + ray.Normal * t);

        if (PointInTriangle(triangle, result)) {
            return true;
        }

        t = -1;
        return false;
    }

    // Conveniance method, returns t without an out param
    // If no collision happened, will return -1
    public static float Raycast(Ray ray, Triangle triangle) {
        float t = -1;
        if (!Raycast(ray, triangle, out t)) {
            return -1;
        }
        return t;
    }

    // Conveniance method, returns the point of intersection
    public static bool Raycast(Ray ray, Triangle triangle, out Point p) {
        float t = -1;
        bool result = Raycast(ray, triangle, out t);
        p = new Point(ray.Position.ToVector() + ray.Normal * t);
        return result;
    }

    // Conveniance method, returns t without an out param
    // If no collision happened, will return -1
    public static float Raycast(Ray ray, Sphere sphere) {
        float t = -1;
        if (!Raycast(ray, sphere, out t)) {
            return -1;
        }
        return t;
    }

    // Conveniance method, returns the point of intersection
    public static bool Raycast(Ray ray, Sphere sphere, out Point p) {
        float t = -1;
        bool result = Raycast(ray, sphere, out t);
        p = new Point(ray.Position.ToVector() + ray.Normal * t);
        return result;
    }

    public static bool Raycast(Ray ray, Sphere sphere, out float t) {
        Vector3 p0 = ray.Position.ToVector();
        Vector3 d = ray.Normal;
        Vector3 c = sphere.Position.ToVector();
        float r = sphere.Radius;

        Vector3 e = c - p0;
        float lenghtESq = Vector3.LengthSquared(e);
        float a = Vector3.Dot(e, d);
        float b = (float)System.Math.Sqrt(((lenghtESq) - (a * a)));
        float f = (float)System.Math.Sqrt((r * r) - (b * b));

        // No collision
        if (r * r - lenghtESq + a * a < 0f) {
            t = 0.0f;
            return false;
        }
        // Ray is inside
        else if (lenghtESq < r * r) {
            t = a + f;
        }
        else {
            t = a - f;
        }
        return t >= 0f;
    }

    // RAY AABB
    // Conveniance method, returns t without an out param
    // If no collision happened, will return -1
    public static float Raycast(Ray ray, AABB aabb) {
        float t = -1;
        if (!Raycast(ray, aabb, out t)) {
            return -1;
        }
        return t;
    }

    // Conveniance method, returns the point of intersection
    public static bool Raycast(Ray ray, AABB aabb, out Point p) {
        float t = -1;
        bool result = Raycast(ray, aabb, out t);
        p = new Point(ray.Position.ToVector() + ray.Normal * t);
        return result;
    }

    // http://gamedev.stackexchange.com/questions/18436/most-efficient-aabb-vs-ray-collision-algorithms
    // https://izzofinal.wordpress.com/2012/11/09/ray-vs-box-round-1/
    public static bool Raycast(Ray ray, AABB aabb, out float t) {
        float t1 = (aabb.minX - ray.Position.X) / ray.Normal.X;
        float t2 = (aabb.maxX - ray.Position.X) / ray.Normal.X;
        float t3 = (aabb.minY - ray.Position.Y) / ray.Normal.Y;
        float t4 = (aabb.maxY - ray.Position.Y) / ray.Normal.Y;
        float t5 = (aabb.minZ - ray.Position.Z) / ray.Normal.Z;
        float t6 = (aabb.maxZ - ray.Position.Z) / ray.Normal.Z;

        float tmin = Max(Max(Min(t1, t2), Min(t3, t4)), Min(t5, t6));
        float tmax = Min(Min(Max(t1, t2), Max(t3, t4)), Max(t5, t6));

        // if tmax < 0, ray (line) is intersecting AABB, but whole AABB is behing us
        if (tmax < 0) {
            t = -1;
            return false;
        }

        // if tmin > tmax, ray doesn't intersect AABB
        if (tmin > tmax) {
            t = -1;
            return false;
        }

        t = tmin;
        if (t < 0) {
            t = tmax;
        }
        return true;
    }

    // RAYCAST
    // Conveniance method, returns t without an out param
    // If no collision happened, will return -1
    public static float Raycast(Ray ray, Plane plane) {
        float t = -1;
        if (!Raycast(ray, plane, out t)) {
            return -1;
        }
        return t;
    }

    // Conveniance method, returns the point of intersection
    public static bool Raycast(Ray ray, Plane plane, out Point p) {
        float t = -1;
        bool result = Raycast(ray, plane, out t);
        p = new Point(ray.Position.ToVector() + ray.Normal * t);
        return result;
    }

    public static bool RaycastNoNormal(Ray ray, Plane plane, out float t) {
        float nd = Vector3.Dot(ray.Normal, plane.Normal);
        float pn = Vector3.Dot(ray.Position.ToVector(), plane.Normal);

        if (Math.Abs(0.0f - nd) <= 0.00001f) {
            t = -1;
            return false;
        }

        t = (plane.Distance - pn) / nd;

        return t >= 0f;
    }

    public static bool Raycast(Ray ray, Plane plane, out float t) {
        float nd = Vector3.Dot(ray.Normal, plane.Normal);
        float pn = Vector3.Dot(ray.Position.ToVector(), plane.Normal);

        if (nd >= 0f) {
            t = -1;
            return false;
        }

        t = (plane.Distance - pn) / nd;

        return t >= 0f;
    }

    static float Max(float x, float y) {
        if (x > y) {
            return x;
        }
        return y;
    }

    static float Min(float x, float y) {
        if (x < y) {
            return x;
        }
        return y;
    }

    public static float Clamp(float test, float min, float max) {
        if (test < min) {
            test = min;
        }
        else if (test > max) {
            test = max;
        }
        return test;
    }
}
```