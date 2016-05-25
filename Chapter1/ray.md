#Ray 

A ray is similar to a line (not line segment). A ray is a single point in space with a direction. It extends infinatley far away in that single direction (as opposed to a line, which extends in both directions).

With the above definition, the code for a ray is pretty simple:

```cs
class Ray {
    public Point position;
    public Vector3 direction;
}
```

Much like line segments, rays are fairly straight forward objets. Because i ray is a directed line (not line segment tough), a lot of the collision code is going to be very similar to that of a line.

### Code Guide

The ```Ray``` class is straight forward. You can follow this guide for implementation, or make your own. Notably, i made the direction a private member called ```_normal```. I do this to ensure that it is normalized at all times. You might want to do that differently, this means you can't to this in my code:

```
ray.Normal.X = 7;
```

The reason i'm ok with that is you rarley set only a single component of a normal! More often than not you must set the whole normal anyway.

```cs
using System;
using OpenTK.Graphics.OpenGL;
using Math_Implementation;

namespace CollisionDetectionSelector.Primitives {
    class Ray {
        public Point Position;
        private Vector3 _normal;

        public Vector3 Normal {
            get {
                // TODO: Returns _normal
                return _normal;
            }
            set {
                // TODO: Sets (and normalizes) _normal
                _normal = value;
                _normal.Normalize();
            }
        }

        public Ray() {
            // Make a ray that is at origin, 
            // pointing down the Z axis
            Position = new Point(0, 0, 0);
            _normal = new Vector3(0, 0, 1);
        }

        public Ray(Point p, Vector3 d) {
            // Don't forget to normalize _normal
            Position = new Point(p.X, p.Y, p.Z);
            _normal = new Vector3(d.X, d.Y, d.Z);
            _normal.Normalize();
        }

        public Ray(Vector3 p, Vector3 d) {
            // Don't forget to normalize _normal
            Position = new Point(p.X, p.Y, p.Z);
            _normal = new Vector3(d.X, d.Y, d.Z);
            _normal.Normalize();
        }

        //Render and toString must be there!
        public void Render() {
            GL.Begin(PrimitiveType.Lines);
            Vector3 start = Position.ToVector();
            Vector3 end = start + _normal * 5000f;
            GL.Vertex3(start.X, start.Y, start.Z);
            GL.Vertex3(end.X, end.Y, end.Z);
            GL.End();
        }

        public override string ToString() {
            string result = "P: (" + Position.X + ", " + Position.Y + ", " + Position.Z + "), ";
            result += "D: ( " + _normal.X + ", " + _normal.Y + ", " + _normal.Z + ")";
            return result;
        }
    }
}
```

## On Your Own

Implement the Line class. You can use the code guide above, or use your own implementation

### Sample / Unit Test

You can [Download](../Samples/CollisionLine.rar) the samples for this chapter to see if your result looks like the unit test.

This example is visual only, no errors will be printed to the console if the code is bad. This is what the final render should look like:

![SAMPLE](lines_sample_01.png)

```cs
using OpenTK.Graphics.OpenGL;
using Math_Implementation;
using CollisionDetectionSelector.Primitives;

namespace CollisionDetectionSelector.Samples {
    class LineSample : Application {
        protected Vector3 cameraAngle = new Vector3(120.0f, -10f, 20.0f);
        protected float rads = (float)(System.Math.PI / 180.0f);

        Line[] origin = new Line[] {
            new Line(new Point(0, 0, 0), new Point(1, 0, 0)),
            new Line(new Point(0, 0, 0), new Point(0, 1, 0)),
            new Line(new Point(0, 0, 0), new Point(0, 0, 1))
        };

        Line[] random = new Line[] {
            new Line(new Point(-7, 5, -3), new Point(8, 5, -10)),
            new Line(new Point(-8, -8, 6), new Point(-9, -2, -10)),
            new Line(new Point(-4, -6, -3), new Point(-9, -1, 5)),
            new Line(new Point(-6, 0, 2), new Point(-1, 1, 3))
        };

        float[][] colors = new float[][] {
            new float[] { 1f, 0f, 1f },
            new float[] { 0f, 1f, 1f },
            new float[] { 1f, 1f, 0f },
            new float[] { 1f, 1f, 1f },
        };

        public override void Intialize(int width, int height) {
            GL.Enable(EnableCap.DepthTest);

            /*System.Random r = new System.Random();
            string output = "new Line(new Point(" + (r.Next() % 20 - 10) + ", " + (r.Next() % 20 - 10) + ", " + (r.Next() % 20 - 10) + "), new Point(" + (r.Next() % 20 - 10) + ", " + (r.Next() % 20 - 10) + ", " + (r.Next() % 20 - 10) + ")),\n";
            for (int i = 0; i < 3; ++i) {
                output += "new Line(new Point(" + (r.Next() % 20 - 10) + ", " + (r.Next() % 20 - 10) + ", " + (r.Next() % 20 - 10) + "), new Point(" + (r.Next() % 20 - 10) + ", " + (r.Next() % 20 - 10) + ", " + (r.Next() % 20 - 10) + ")),\n";
            }
            System.IO.File.WriteAllText(@"C:\Users\WinVPC\Desktop\Lines.txt", output);*/
        }

        public override void Render() {
            Vector3 eyePos = new Vector3();
            eyePos.X = cameraAngle.Z * -(float)System.Math.Sin(cameraAngle.X * rads * (float)System.Math.Cos(cameraAngle.Y * rads));
            eyePos.Y = cameraAngle.Z * -(float)System.Math.Sin(cameraAngle.Y * rads);
            eyePos.Z = -cameraAngle.Z * (float)System.Math.Cos(cameraAngle.X * rads * (float)System.Math.Cos(cameraAngle.Y * rads));

            Matrix4 lookAt = Matrix4.LookAt(eyePos, new Vector3(0.0f, 0.0f, 0.0f), new Vector3(0.0f, 1.0f, 0.0f));
            GL.LoadMatrix(Matrix4.Transpose(lookAt).Matrix);

            GL.Color3(1f, 0f, 0f);
            origin[0].Render();
            GL.Color3(0f, 1f, 0f);
            origin[1].Render();
            GL.Color3(0f, 0f, 1f);
            origin[2].Render();

            for (int i = 0; i < random.Length; ++i) {
                GL.Color3(colors[i][0], colors[i][1], colors[i][2]);
                random[i].Render();
            }
        }

        public override void Update(float deltaTime) {
            cameraAngle.X += 45.0f * deltaTime;
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