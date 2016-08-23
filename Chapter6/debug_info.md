#Debug info

Before moving on, to implement a camera frustum i want to add some debug info to the scene. Nameley, i want to know how many objects are rendered. I'm actually going to provide all the code for this, just follow along. This is actually pretty simple, we start with the most basic thing that's being rendered.

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
public int Render() {
    int result = 0;
    
    GL.PushMatrix();
    GL.MultMatrix(WorldMatrix.OpenGL);
    if (model != null) {
        model.Render();
        result += 1;
    }
    GL.PopMatrix();
    
    result += ChildrenRender(true, false, false);
    return result;
}
```

All this changes is the Render function now keeps track of every time model.Render has been caleed. Of course, this means we now also have to change ```ChildrenRender``` to:

```cs
private int ChildrenRender(bool normal,bool bvh, bool debug) {
    int result = 0;
    if (Children != null) {
        foreach(OBJ child in Children) {
            if (normal) {
                result += child.Render();
            }
            else if (bvh) {
                child.RenderBVH();
            }
            else if (debug) {
                child.DebugRender();
            }
            if(child.Children != null) {
                result += child.ChildrenRender(normal, bvh, debug);
            }
        }
    }
    return result;
}
```

