#Octree Node

A tree is made up of many nodes, the octree is no different. There is no specific ```Octree``` class, just an ```OctreeNode``` within our scene that servers as the root of the tree.

### Create Node

Let's start off with the definition of a node. A node contains an Axis Aligned Bounding Box, this is the area of the node. A reference to it's parent (We are in a tree after all), and a list of it's children. The node also contains a list of objects it can potentially hold.

If a node has children, it will not have contents. If a node has contents, it will not have children. This makes determining what is and isn't a leaf node fairly simple. Leaf nodes have no children, therefore ```Children``` will be null.

The constructor by default creates each node as a leaf node.

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

### Insert Object

### Remove Object

### Update Object