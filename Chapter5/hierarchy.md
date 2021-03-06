# Transform hiearchy

Our scene is going to consist of only OBJ objects. They have something to display, and they already have a sense of "position". We're going to change the OBJ class into a tree. Essentially, this will give us a transform hierarchy.

### Changine the OBJ class into nodes

Having a tree is all about having child and parent nodes. The first thing we're going to do is add a Parent reference, and a list of children to the OBJ class.

```cs
public OBJ Parent = null;
public List<OBJ> Children = new List<OBJ>();
```

__TODO: __ Now, adjust the Render, RenderBVH and RenderDebug functions to loop trough all children of the OBJ and call the appropriate Render function. Do this loop after the PopMatrix, each child has it's own world matrix, and will set it for its-self.

Also, we're going to have OBJ's with a null model, be sure to add null checks. If an OBJ's internal model variable is null it will not render, but all of its children still should.

### Transform hierarchy

We're also going to make the OBJ's form a transform hierarchy. So, if you move one of the parent obj's all of it's children will move. In order to do this, the first thing we need to do is clear up what is in world-space and what is in object space.

The __WorldMatrix__ getter is obviously in world space. It is an absolute position, relative to world 0, regardless of the parent of the node.

The __Position__, __Rotation__ and __Scale__ getters / setters are going to be in local space. That is, they are relative to the node's parent. If the parent's position is ```0, 1, 2``` and the node's local position is ```1, 0, 1``` then the world position of the node will be ```1 1 3```

Because we've now introducted the concept of an "Empty" OBJ IE: Once without a mesh, let's add a getter to check if an OBJ is empty or not

```cs
public bool IsEmpty {
    get {
        return model == null;
    }
}
```

Because we work in world and local space now, when the transform of a parent node changes, all children must be updated. We almost have a method of doing this already, each object has a dirty flag.

All we have to do is make the dirty flag recursive. If a node is marked as dirty, all it's children will need to be marked dirty as well.

Change

```cs
protected bool dirty = true;
```

To

```cs
protected bool dirtySelf = true;

protected bool dirty {
    get {
        return dirtySelf;
    }
    set {
        dirtySelf = value;
        if (value) {
            foreach(OBJ child in Children) {
                child.dirty = true;
            }
        }
    }
}
```

With the use of a protected getter we made the dirty flag recursive. That is, whenever an OBJ is marked as dirty, all it's children, and all of that nodes children will be marked dirty as well. This will force every child of the node to re-calculate it's own world matrix, the next time render is called.

### Applying the transform

The last thing we need to do here is make sure the world matrix takes the parents world matrix into account. We can achieve this, by multiplying the world matrix by the parent nodes world matrix


```cs
public Matrix4 WorldMatrix {
    get {
        if (dirty) {
            Matrix4 translation = Matrix4.Translate(position);

            Matrix4 pitch = Matrix4.XRotation(rotation.X);
            Matrix4 yaw = Matrix4.YRotation(rotation.Y);
            Matrix4 roll = Matrix4.ZRotation(rotation.Z);
            Matrix4 orientation = roll * pitch * yaw;

            Matrix4 scaling = Matrix4.Scale(scale);

            worldMatrix = translation * orientation * scaling;

            if (Parent != null) {
                worldMatrix = Parent.WorldMatrix * worldMatrix;
            }
            
            // DO NOT FORGET THIS! The first version
            // I wrote, i forgot to clear the dirty flag
            // and my game stopped running!
            dirty = false;
        }
        return worldMatrix;
    }
}
```

As you can see, not much has actually changed, we just added another matrix multiplication. Important to note, we are multiplying by the parent's world matrix getter ```Parent.WorldMatrix```, using a capital W!