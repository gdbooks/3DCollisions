#Bounding Volume Hierarchy

We're going to develop a BVH around our model class. The actual BVH will be a part of the OBJLoader class, but it's methods will be exposed trough the OBJ class as well.

I guess it's worth noting, every time i've built one of these BVH containers it came out different than the last one. This is why tree construction is a common interview question.

Because a bounding volume hierarchy is a tree, it is made up of nodes. The root of the tree will just be a regular node. So, before anything else, let's develop a class for the bvh node

###BVHNode

For now we're going to divide the BVH tree at most 3 times, this is going to be a protected static variable. 

A BVHNode is either a leaf node or not. If it is a leaf node, it will contain triangles and have no children. If it is not a leaf node, it will have children but no triangles. A node can have triangles or children, never both. We will expose this in a getter.

Finally, each BVH node is a box, an AABB box. As such, each node will need to have an AABB member. Also, when a new node is made, we assume it is going to be a leaf node.

```cs
public class BVHNode {
    protected static int maxDepth = 3;

    public List<BVHNode> Children = null;
    public List<Triangle> Triangles = null;
    public AABB AABB = null;

    public BVHNode(AABB aabb) {
        // Store AABB by value, not reference
        AABB = new AABB(aabb.Min, aabb.Max);
        // Assume this is leaf node by default
        Triangles = new List<Triangle>();
        Children = null;
    }
    
    public bool IsLeaf {
        get {
            return Children == null && Triangles != null;
        }
    }
}
```

Let's add a BVHNode object to the ```OBJLoader``` class:

```cs
protected BVHNode bvhRoot = null;

public BVHNode BvhRoot {
    get {
        return bvhRoot;
    }
}
```