# AABB

Our model now has a triangle soup. We can raycast against this, or do collision test, whatever works. But, like we discussed that's really not optimal! so, instead let's make a function that creates an Axis Aligned Bounding Box around the model.

## Creating the AABB

We already have an array of triangles added to ```OBJLoader.cs```, now lets add a new member variable to it for the AABB of the mesh.

```cs
protected AABB containerAABB = null;
```