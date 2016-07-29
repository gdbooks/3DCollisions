#Shake

Right now, every node in our BVH has exactly 8 or 0 children. This structure is called an __Octree__. This octree structure is not optimal, 7 of those 8 children might not actually contain any triangles.

We can eliminate any empty leaf nodes, meaning a BVH node will have 0, 1, 2, 3, 4, 5, 6, 7 or 8 children. This modified structure is called a __sparse octree__. The process of removing empty leaf nodes from the tree is called shaking. You have to shake the tree, because programmers have a real sense of humor.

### Count the triangles

The first piece of information we need to know about a BVH node in order to shake the tree is how many triangles the node contains. This isn't as simple as returning the count of the triangle list. 

We need to know how many children the node, and all of it's children and all of their children contain. This means that getting the child count is going to be a recursive function.

```cs
 public int TriangleCount {
    get {
        int totalCount = 0;
        if (Children != null) {
            foreach (BVHNode child in Children) {
                totalCount += child.TriangleCount;
            }
        }
        else if (Triangles != null) {
            totalCount += Triangles.Count;
        }

        return totalCount;
    }
}
```