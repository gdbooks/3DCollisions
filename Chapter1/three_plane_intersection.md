# 3 Plane Intersection

Planes have a pretty special property. Given 3 unique planes, they intersect at exactly one point!

![SAMPLE](3plane_int.gif)

Any 3 dimensional cordinate system has 3 axis (x, y, z) which can be represented by 3 planes. Where those axis meet is considered (0, 0, 0) or the origin of the coordinate space.

Other than debug visualization, i've never used this intersection test in production. But it's useful to know in order to visualize some debug properties.

### The algorithm

I don't fully understand this algorithm, usually i copy it out of a book. In that spirit, i'm just going to go ahead and provide it:

```cs
Point IntersectPlanes(Plane p1, Plane p2, Plane p3) {
    Vector m1 = new Vector(p1.Normal.X, p2.Normal.X, p3.Normal.X);
    Vector m2 = new Vector(p1.Normal.Y, p2.Normal.Y, p3.Normal.Y);
    Vector m3 = new Vector(p1.Normal.Z, p2.Normal.Z, p3.Normal.Z);
    Vector d = new Vector(p1.Distance, p2.Distance, p3.Distance);
    
    Vector u = Cross(m2, m3);
    Vector v =Cross (m1, d);
    
    float denom = Dot(m1, u);
    
    if (Abs(denom) < EPSILON) {
        // Planes don't actually intersect in a point
        // Throw exception maybe?
        return new Point(0, 0, 0);
    }
       
    return new Point(
        Dot(d, u) / denom,
        Dot(m3, v) / denom,
        -Dot(m2, v) / denom
    );
}
```

Of course, there are ways for 3 planes to intersect that do not end in a point:

![P](plane_intersection_npot.png)

In that case we just return 0, but it might be appropriate to throw an exception. Or it might not, depends on what you use this function for!

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static Point Intersection(Plane p1, Plane p2, Plane p3);
```

And provide an implementation for it!

### Unit Test

#TODO: Download

The following code is visual only, if you make any mistakes no error is printed!

The red and blue planes render just fine, the green plane will not. This is because of a minor flaw in the render code of the plane class. The intersection of the 3 planes renders in a light blue color.

```cs
using OpenTK.Graphics.OpenGL;
using Math_Implementation;
using CollisionDetectionSelector.Primitives;

namespace CollisionDetectionSelector.Samples {
    class ThreePlaneIntersectionSample : Application {
        protected Vector3 cameraAngle = new Vector3(120.0f, -10f, 20.0f);
        protected float rads = (float)(System.Math.PI / 180.0f);

        Plane plane1 = new Plane(new Vector3(1f, 0f, 0f), 0f);
        Plane plane2 = new Plane(new Vector3(0f, 1f, 0f), 0f);
        Plane plane3 = new Plane(new Vector3(0f, 0f, 1f), 0f);

        public override void Intialize(int width, int height) {
            GL.Enable(EnableCap.DepthTest);
            GL.PointSize(5f);
        }

        public override void Render() {
            Vector3 eyePos = new Vector3();
            eyePos.X = cameraAngle.Z * -(float)System.Math.Sin(cameraAngle.X * rads * (float)System.Math.Cos(cameraAngle.Y * rads));
            eyePos.Y = cameraAngle.Z * -(float)System.Math.Sin(cameraAngle.Y * rads);
            eyePos.Z = -cameraAngle.Z * (float)System.Math.Cos(cameraAngle.X * rads * (float)System.Math.Cos(cameraAngle.Y * rads));

            Matrix4 lookAt = Matrix4.LookAt(eyePos, new Vector3(0.0f, 0.0f, 0.0f), new Vector3(0.0f, 1.0f, 0.0f));
            GL.LoadMatrix(Matrix4.Transpose(lookAt).Matrix);

            DrawOrigin();

            GL.Color3(1f, 0f, 0f);
            plane1.Render();
            GL.Color3(0f, 1f, 0f);
            plane2.Render();
            GL.Color3(0f, 0f, 1f);
            plane3.Render();

            GL.Disable(EnableCap.DepthTest);
            GL.Color3(0f, 1f, 1f);
            Point midPoint = Collisions.Intersection(plane1, plane2, plane3);
            midPoint.Render();
            GL.Enable(EnableCap.DepthTest);
        }

        public override void Update(float deltaTime) {
            cameraAngle.X += 45.0f * deltaTime;
        }

        protected void DrawOrigin() {
            GL.Begin(PrimitiveType.Lines);
            GL.Color3(1f, 0f, 0f);
            GL.Vertex3(0f, 0f, 0f);
            GL.Vertex3(1f, 0f, 0f);
            GL.Color3(0f, 1f, 0f);
            GL.Vertex3(0f, 0f, 0f);
            GL.Vertex3(0f, 1f, 0f);
            GL.Color3(0f, 0f, 1f);
            GL.Vertex3(0f, 0f, 0f);
            GL.Vertex3(0f, 0f, 1f);
            GL.End();
        }

        public override void Resize(int width, int height) {
            GL.Viewport(0, 0, width, height);
            GL.MatrixMode(MatrixMode.Projection);
            float aspect = (float)width / (float)height;
            Matrix4 perspective = Matrix4.Perspective(60, aspect, 0.01f, 1000.0f);
            GL.LoadMatrix(Matrix4.Transpose(perspective).Matrix);
            GL.MatrixMode(MatrixMode.Modelview);
            GL.LoadIdentity();
        }
    }
}
```