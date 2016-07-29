#Bounding Volume Hierarchy

We're going to develop a BVH around our model class. The actual BVH will be a part of the OBJLoader class, but it's methods will be exposed trough the OBJ class as well.

I guess it's worth noting, every time i've built one of these BVH containers it came out different than the last one. This is why tree construction is a common interview question.

Because a bounding volume hierarchy is a tree, it is made up of nodes. The root of the tree will just be a regular node. So, before anything else, let's develop a class for the bvh node

###BVHNode

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