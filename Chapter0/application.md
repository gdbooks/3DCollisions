

```cs
using System;
using OpenTK;
using System.Drawing;
using OpenTK.Graphics.OpenGL;

// Lives in global namespace
class Application {
    public static Application Instance {
        get; private set;
    }

    public OpenTK.GameWindow Window = null;

    public Application() {
        Instance = this;
    }

    /// These are the logic driving functions that classes whom subclass application
    /// will need to override. There is no need to call the base versions!
    #region Inheritable Logic
    public virtual void Intialize(int width, int height) {

    }

    public virtual void Resize(int width, int height) {

    }

    public virtual void Update(float deltaTime) {

    }

    public virtual void Render() {

    }

    public virtual void Shutdown() {

    }
    #endregion

    /// Entery point of the application, by default it creates a new window
    /// Your subclasses should not have to override this, but if you want a
    /// sample that does not have a window, you need to. 
    #region Entery Point
    public virtual void Main(string[] args) {
        Window = new OpenTK.GameWindow();
        Window.Load += new EventHandler<EventArgs>(OpenTKInitialize);
        Window.UpdateFrame += new EventHandler<FrameEventArgs>(OpenTKUpdate);
        Window.RenderFrame += new EventHandler<FrameEventArgs>(OpenTKRender);
        Window.Unload += new EventHandler<EventArgs>(OpenTKShutdown);

        Window.Title = "Sample Application";
        Window.ClientSize = new System.Drawing.Size(800, 600);
        Instance.Resize(800, 600);
        Window.VSync = VSyncMode.On;
        Window.Run(60.0f);

        Window.Dispose();
    }
    #endregion

    /// If the default entery point (Main function) is used, these are the OpenTK callbacks
    /// Which are issued, they in turn call member functions that subclasses need to override
    #region OpenTK Callbacks
    private void OpenTKInitialize(object sender, EventArgs e) {
        Console.Clear();
        Console.WriteLine("OpenGL Vendor: " + GL.GetString(StringName.Vendor));
        Console.WriteLine("OpenGL Renderer: " + GL.GetString(StringName.Renderer));
        Console.WriteLine("OpenGL Version: " + GL.GetString(StringName.Version));
        Instance.Intialize(800, 600);
    }

    private void OpenTKUpdate(object sender, FrameEventArgs e) {
        float dTime = (float)e.Time;
        Instance.Update(dTime);
    }

    private void OpenTKRender(object sender, FrameEventArgs e) {
        GL.ClearColor(Color.CadetBlue);
        GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit | ClearBufferMask.StencilBufferBit);
        Instance.Render();
        Window.SwapBuffers();
    }

    private void OpenTKShutdown(object sender, EventArgs e) {
        Instance.Shutdown();
    }

    private void OpenTKResize(EventArgs e) {
        Instance.Resize(Instance.Window.Width, Instance.Window.Height);
    }
    #endregion
}
```