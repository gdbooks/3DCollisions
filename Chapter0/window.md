#Window

Making a windowed sample is as easy as making a console one. If you want a window, you probably don't need to override the ```Main``` method, but you will need to override one or more of the following:

* Intialize(int width, int height)
* Resize(int width, int height)
* Update(float deltaTime)
* Render()
* Shutdown()

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using OpenTK.Graphics.OpenGL;
using Math_Implementation;

namespace CollisionDetectionSelector.Samples {
    class WindowedSample : Application {
        public override void Render() {
            Matrix4 lookAt = Matrix4.LookAt(new Vector3(0.0f, 0.0f, 30.0f), new Vector3(0.0f, 0.0f, 0.0f), new Vector3(0.0f, 1.0f, 0.0f));
            GL.LoadMatrix(lookAt.OpenGL);

            GL.ShadeModel(ShadingModel.Smooth);

            GL.Begin(PrimitiveType.Triangles);
            GL.Color3(1.0f, 0.0f, 0.0f);
            GL.Vertex3(-10.0f, -10.0f, -5.0f);//red
            GL.Color3(0.0f, 1.0f, 0.0f);
            GL.Vertex3(20.0f, -10.0f, -5.0f);//green
            GL.Color3(0.0f, 0.0f, 1.0f);
            GL.Vertex3(-10.0f, 20.0f, -5.0f);//blue
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
