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

Inserting an object is fairly straight forward. The object will only insert if the Bounds of the current node intersect it. If that's not the case, none of the children nodes will intersect the object either! 

^ The above statement only holds true if the ABB-OBJ test is implemented as a SAT test. Ours is not, this is why we are going to be testing the bounding sphere, instead of the OBJ its-self. That way, our Octree insertion is a series of Sphere-AABB tests.

```cs
public bool Insert(OBJ obj) {
    if (Collisions.Intersects(obj.BoundingSphere, Bounds)) {
        // We knoe the obj intersects this node, if it's a leaf
        // add it to the object list, if not, recurse!
        if (Children != null) {
            foreach(OctreeNode child in Children) {
                child.Insert(obj);
            }
        }
        else {
            Contents.Add(obj);
        }
        return true;
    }
    return false;
}
```

### Remove Object

Removing an object is super simple. If this node is a leaf node, search trough the objects it contains, and remove the object if it is in the list. If not, recursivley call the function for every node

```cs
public void Remove(OBJ obj) {
    if (Children != null) {
        foreach(OctreeNode child in Children) {
            child.Remove(obj);
        }
    }
    else {
        Contents.Remove(obj);
    }
}
```

### Update Object

Updating an octree is a very hot topic of research. Everyone wants to have the fastest update possible. When an object moves, rotates or scales, we have to update what octree nodes it belongs to.

While there are a LOT of algorithms for this out there, we're going to implement a very basic one! Any time an object rotates or moves we simply remove it from the tree and re-insert it. This is the brute force method, it's not efficient, but it is reliable

```cs
public bool Update(OBJ obj) {
    Remove(obj);
    return Insert(obj);
}
```

### Debug Render

It's useful to be able to visualize what we just implemented! For that reason, i say we add a ```DebugRender``` function. This function is simple, it just recursivley draws the bounding rectangle of each node.

```cs
public void DebugRender() {
    Bounds.Render();
    if (Children != null) {
        foreach (OctreeNode node in Children) {
            node.DebugRender();
        }
    }
}
```

## Scene Integration

Now that we have an octree node, all we have to do is add the root node to the scene, and we have an octree! I'm going to be modifying the existing scene class for this. 

First, add the octree root to the scene. Next, add an initalize function. This function will take in the overall size of the octree (The half-size of it's root node). This function is responsible for creating the tree and splitting it. A split size of 3 will do for now.

```cs
public OctreeNode Octree = null;

public void Initialize(float octreeSize) {
    Octree = new OctreeNode(new Point(0, 0, 0), octreeSize, null);
    Octree.Split(3);
}
```

In larger AAA games, it's not uncommon to see the default octree be of __size 5000__, with a __split__ level of __8__.

Next up, let's modify the render function of the scene to render the debug version of the octree

```cs
public void Render() {
    RootObject.Render();
    
    GL.Disable(EnableCap.Lighting);
    GL.PolygonMode(MaterialFace.FrontAndBack, PolygonMode.Line);
    Octree.DebugRender();
    GL.PolygonMode(MaterialFace.FrontAndBack, PolygonMode.Fill);
    GL.Enable(EnableCap.Lighting);
}
```