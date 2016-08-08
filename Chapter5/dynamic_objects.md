#Octree Node

A tree is made up of many nodes, the octree is no different. There is no specific ```Octree``` class, just an ```OctreeNode``` within our scene that servers as the root of the tree.

### Create Node

Let's start off with the definition of a node. A node contains an Axis Aligned Bounding Box, this is the area of the node. A reference to it's parent (We are in a tree after all), and a list of it's children. The node also contains a list of objects it can potentially hold.

If a node has children, it will not have contents. If a node has contents, it will not have children. This makes determining what is and isn't a leaf node fairly simple. Leaf nodes have no children, therefore ```Children``` will be null.

The constructor will, by default make each node a leaf node. It isn't until later, when we split the node that it can become a non-leaf.

```cs
using System.Collections.Generic;
using Math_Implementation;
using CollisionDetectionSelector.Primitives;

namespace CollisionDetectionSelector {
    class OctreeNode {
        public AABB Bounds = null;
        public OctreeNode Parent = null;
        public List<OctreeNode> Children = null;
        public List<OBJ> Contents = null;

        public OctreeNode(Point position, float halfSize, OctreeNode parent) {
            Bounds = new AABB(
                new Point(position.X - halfSize, position.Y - halfSize, position.Z - halfSize),
                new Point(position.X + halfSize, position.Y + halfSize, position.Z + halfSize)
            );
            Parent = parent;
            Contents = new List<OBJ>();
            // Keep children null, this is a leaf node until the "Split" function is called
        }
    }
}
```
      

### Split Node

The split node function will invalidate the current octree. Therefore it MUST be called BEFORE any objects are inserted into the tree. Much like the BVH version, our split method is going to be recursive.

Unlike the BVH however, the user will have control over how many times a node gets split! The function takes a single integer, the number of times to split the node. This integer is decremented by 1 for each recursive call.

```cs
public void Split(int level) {
    // Bounds.Extens.x is the same as y and z, all nodes are square
    // We want each child node to be half as big as this node
    float splitSize = Bounds.Extents.X / 2.0f;

    // Also, the constructor of the Octree node takes in the CENTER
    // position of the new node, therefore we want to create new
    // nodes from the center of this node + or - 1/4 the size of
    // this node. This is similar to BVH, but different in size.
    Vector3[] childPattern = new Vector3[] {
        new Vector3(+1f, -1f, -1f), // Right, Top, Front
        new Vector3(+1f, -1f, +1f), // Right, Top, Back
        new Vector3(+1f, +1f, -1f), // Right, Bottom, Front
        new Vector3(+1f, +1f, +1f), // Right, Bottom, Back
        new Vector3(-1f, -1f, -1f), // Left, Top, Front
        new Vector3(-1f, -1f, +1f), // Left, Top, Back
        new Vector3(-1f, +1f, -1f), // Left, Bottom, Front
        new Vector3(-1f, +1f, +1f), // Left, Bottom, Back
    };

    // Make the actual child nodes
    Children = new List<OctreeNode>();
    Contents = null; // We are no longer a leaf node!
    foreach (Vector3 offset in childPattern) {
        // Remember to account for the center of the current node
        Point position = new Point(Bounds.Center.ToVector() + offset * splitSize);
        OctreeNode child = new OctreeNode(position, splitSize, this);
        Children.Add(child);
    }

    // If we have not reached the max split depth yet, 
    // go ahead and recursivley call this method for all children
    if (level > 1) {
        foreach(OctreeNode child in Children) {
            child.Split(level - 1);
        }
    }
}
```

### Insert Object

### Remove Object

### Update Object