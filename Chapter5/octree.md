#Octree

An octree is a tree in which every node is either a leaf or has 8 children. It's very similar to the BVH acceleration structure we implemented for model raycasting. Like the BVH, the octree is also an acceleration structure.

Unlike the BVH structure, Octree's are not usually sparse. That means we don't shake the tree. Every node has 8 children or no children. There are many octree variations out there, we are going to implement the simplest one.

There are schemes where an object belongs to multiple octre nodes, schemes where octree node boundries are fuzzy, and some really crazy implementations i can't follow. What makes our implementaiton simple is that each object will belong to exactly 1 octree node. This is not the most efficient octree implementation, but it's the basic one.

### Acceleration structure

We're going to explore how the octree can be used as an acceleration structure to speed up raycasting into our scene. The idea is almost the same as the BVH we created earlyer. 

We do a raycast, if the ray hits one of the octree nodes. If the ray hits a node, we raycast against all models inside the node. If any of those models where hit by the ray, we return the model. If not, we recursivley call raycast on all children nodes of the octree node.

### Updating

Whenever an object changes position (The "Is Dirty" flag is set) there is a chance that it migrated into a new node of the octree. Should this happen, we have to remove the object from the node it currently belongs to, and try to insert it into the parent of that node. If that insertion fails, we recursivley walk the tree and keep trying to insert into a new parent until we reach the top of the tree.