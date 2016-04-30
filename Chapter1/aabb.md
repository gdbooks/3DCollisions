#WIP - This section is being worked on

#AABB - Axis Aligned Bounding Box

An AABB (Axis Aligned Bounding Box) is a 3D box. It's width / height / depth don't have to be equal, but the width is always aligned to the X axis, the height to the Y axis and depth to the Z axis. That is to say, this box can't be rotated.

AABB's are pivotal to spacial partitioning, they let us cut a section of 3D space up into smaller sections of 3D space. AABB's also serve as a great acceleration structure for hit detection, but more on that later.

The first question we have to ask is how to represent an AABB? There are two common ways, by storing the leftmost and rightmost corners, OR by storing the center point and a vector of how far the box extends on each side.

![Types](AABB_Types.png)

Most mathematical texts describe an AABB as a min and max point. Because of this, i like to design my AABB classes as such. However, there is a bit of a problem, what if a user sets the min.X of the AABB to be greater than the max.X? For this reason i usually include 2 bits of code:

```cs
// THIS BLOCK IS JUST SAMPLE CODE, DON'T COPY IT!
public bool IsValid {
    get {
        return Min.X < Max.X && Min.Y < Max.Y && Min.Z < Max.Z;
    }
}

public void Fix() {
    if (Min.X > Max.X) {
        // SWAP
    }
    if (Min.Y > Max.Y) {
        // Swap
    }
    if (Min.Z > Max.Z) {
        // Swap
    }
}
```

At the end of the day, your representation doesn't matter much. If you represent your AABB as a min and max internally, you can expose getter only ```Center``` and ```Extents``` properties.

### Code Guide

```cs
using System;
using OpenTK.Graphics.OpenGL;
using Math_Implementation;

namespace CollisionDetectionSelector.Primitives {
    class AABB {
        public Point Min = new Point();
        public Point Max = new Point();

        public Point Center {
            get {
                // FIGURE OUT CENTER POINT
            }
            // NO SET!
        }

        public Vector3 Extents {
            get {
                // FIGURE OUT EXTENTS
            }
            // NO SET!
        }

        public bool IsValid {
            // TODO: Implement, getter only, no setter!
        }

        public void Fix() {
            // TODO: Implement
        }

        public AABB() {
            // TODO: Make a unit AABB
            // Min = -1, max = +1 on all axis
        }

        public AABB(Point min, Point max) {
            // TODO: Set Min and Max
            // Don't forget, could be invalid
        }

        public AABB(Point center, Vector3 extents) {
            // TODO: Set Min and Max
        }

        public void Render() {
            GL.Begin(PrimitiveType.Quads);

            GL.Vertex3(Min.X, Min.Y, Max.Z);
            GL.Vertex3(Max.X, Min.Y, Max.Z);
            GL.Vertex3(Max.X, Max.Y, Max.Z);
            GL.Vertex3(Min.X, Max.Y, Max.Z);

            GL.Vertex3(Max.X, Min.Y, Max.Z);
            GL.Vertex3(Max.X, Min.Y, Min.Z);
            GL.Vertex3(Max.X, Max.Y, Min.Z);
            GL.Vertex3(Max.X, Max.Y, Max.Z);

            GL.Vertex3(Min.X, Max.Y, Max.Z);
            GL.Vertex3(Max.X, Max.Y, Max.Z);
            GL.Vertex3(Max.X, Max.Y, Min.Z);
            GL.Vertex3(Min.X, Max.Y, Min.Z);

            GL.Vertex3(Min.X, Min.Y, Min.Z);
            GL.Vertex3(Min.X, Max.Y, Min.Z);
            GL.Vertex3(Max.X, Max.Y, Min.Z);
            GL.Vertex3(Max.X, Min.Y, Min.Z);

            GL.Vertex3(Min.X, Min.Y, Min.Z);
            GL.Vertex3(Max.X, Min.Y, Min.Z);
            GL.Vertex3(Max.X, Min.Y, Max.Z);
            GL.Vertex3(Min.X, Min.Y, Max.Z);

            GL.Vertex3(Min.X, Min.Y, Min.Z);
            GL.Vertex3(Min.X, Min.Y, Max.Z);
            GL.Vertex3(Min.X, Max.Y, Max.Z);
            GL.Vertex3(Min.X, Max.Y, Min.Z);

            GL.End();
        }

        public override string ToString() {
            string result = "Min: (" + Min.X + ", " + Min.Y + ", " + Min.Z + "), ";
            result += "Max: ( " + Max.X + ", " + Max.Y + ", " + Max.Z + ")";
            return result;
        }
    }
}
```
