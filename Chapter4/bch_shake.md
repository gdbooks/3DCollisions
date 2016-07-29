#Shake

Right now, every node in our BVH has exactly 8 or 0 children. This structure is called an __Octree__. This octree structure is not optimal, 7 of those 8 children might not actually contain any triangles.

We can eliminate any empty leaf nodes, meaning a BVH node will have 0, 1, 2, 3, 4, 5, 6, 7 or 8 children. This modified structure is called a __sparse octree__. The process of removing empty leaf nodes from the tree is called shaking. You have to shake the tree, because programmers have a real sense of humor.