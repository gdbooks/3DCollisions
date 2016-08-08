#Octree Node

A tree is made up of many nodes, the octree is no different. There is no specific ```Octree``` class, just an ```OctreeNode``` within our scene that servers as the root of the tree.

### Create Node

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

        public OctreeNode(Point position, float size, OctreeNode parent) {
            Bounds = new AABB(
                new Point(position.X - size, position.Y - size, position.Z - size),
                new Point(position.X + size, position.Y + size, position.Z + size)
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