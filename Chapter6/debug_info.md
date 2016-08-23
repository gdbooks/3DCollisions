#Debug info

Before moving on, to implement a camera frustum i want to add some debug info to the scene. Nameley, i want to know how many objects are rendered. This is actually pretty simple, we start with the most basic thing that's being rendered.

In __OBJ.cs__, change the ```Render``` method of the ```OBJ``` class from this (taken from your github):

```cs
public void Render() {
    GL.PushMatrix();
    //always getter
    GL.MultMatrix(WorldMatrix.OpenGL);
    if (model != null) {
        model.Render();
    }
    GL.PopMatrix();
    ChildrenRender(true, false, false);
}
```

to this

```cs
public void Render() {
    GL.PushMatrix();
    //always getter
    GL.MultMatrix(WorldMatrix.OpenGL);
    if (model != null) {
        model.Render();
    }
    GL.PopMatrix();
    ChildrenRender(true, false, false);
}
```