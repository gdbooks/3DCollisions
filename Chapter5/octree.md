#Octree

An octree is a tree in which every node is either a leaf or has 8 children. It's very similar to the BVH acceleration structure we implemented for model raycasting. Like the BVH, the octree is also an acceleration structure.

Unlike the BVH structure, Octree's are not usually sparse. That means we don't shake the tree. Every node has 8 children or no children.