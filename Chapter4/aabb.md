# AABB

Our model now has a triangle soup. We can raycast against this, or do collision test, whatever works. But, like we discussed that's really not optimal! so, instead let's make a function that creates an Axis Aligned Bounding Box around the model.

## Creating the AABB

We already have an array of triangles added to ```OBJLoader.cs```, now lets add a new member variable to it for the AABB of the mesh.

```cs
protected AABB containerAABB = null;
```

We're going to have to populate this AABB in the constructor of the mesh. Again, the ```vertexData``` array is our friend. 

First, assign ```containerAABB``` to be a new AABB object. Both the min and max point of this object should be the first vertex of ```vertexData```. Remember, the first vertex is elements 0, 1 and 2.

```cs
containerAABB = new AABB(new Point(vertices[0], vertices[1], vertices[2]),
                new Point(vertices[0], vertices[1], vertices[2]));
```

Now loop trough vertexData one vertex at a time. You will have to adjust containerAABB one component at a time. If a vertex has a smaller y than min-y assign a new y. For eaxample:

```cs
// ...
if (vertexData[i + 0] < containerAABB.minX) {
    containerAABB.minX = vertexData[i + 0];
}
if (vertexData[i + 0] > containerAABB.maxX) {
    containerAABB.maxX = vertexData[i + 0];
}
if (vertexData[i + 1] < containerAABB.minY) {
    containerAABB.minY = vertexData[i + 0];
}
if (vertexData[i + 1] > containerAABB.maxY) {
    containerAABB.maxY = vertexData[i + 1];
}
if (vertexData[i + 2] < containerAABB.minZ) {
    containerAABB.minZ = vertexData[i + 0];
}
if (vertexData[i + 2] > containerAABB.maxZ) {
    containerAABB.maxZ = vertexData[i + 2];
}
// ...
```

## Debug Render

Now, let's add a public getter for the aabb:

```cs
public AABB BoundingBox {
    get {
        return containerAABB;
    }
}
```

And modify the debug-render method to draw this box:

```cs
public void DebugRender() {
    containerAABB.Render();
    foreach (Triangle trianlge in collisionMesh) {
        trianlge.Render();
    }
}
```

## Unit Test