# Other structures

Scenes can be orgonized into a veriety of structures. Octree's are a good general purpose sturcture, but many games use more specific structures that are more optimized for the type of game they are. For example, Doom3 doesn't have an octree, it uses portals. That's because the entire game is indoors, with tight corners and actual doors. Let's explore some alternatives on a high level.

### PVS

[PVS or Potential Visibility Set](https://en.wikipedia.org/wiki/Potentially_visible_set) is a method that was very popular in the early days of PC-games, most of the Quake games used it. Some indie games today still use it, for it's ease of implementation. 

Building a PVS is pretty simple, you divide the world into chunks using arbitrary cutting planes. Then, you place the camera at random points within this chunk and spin it around. Whatever other chunks where visible are added to the visibility set of this chunk.

Once you have built up all chunks, and PVS sets for each chunk at runtime it's just a matter of checking which chunk the camera is in, and then rendering all chunks within the PVS of the current one.

### Kd-Tree

A [kd-tree, short for k-dimensional tree](https://en.wikipedia.org/wiki/K-d_tree) is often used for large outdoor games (Like grand theft auto or far cry).

In practice, a kd-tree works very similar to the BVH structure we created, just on a larger scene scale, like the Octree. The kd-tree is shaken, so the number of children is not exact. There is one major difference between the kd-tree and things we implemented. Unlike our things, the KD-tree is not evenly split.

Instead of getting split down the middle, like an octree you split a kd tree according to some heuristic. Usually, polygon count. This leaves the tree divided heavily where it counts, and not very much where it doesn't.

![KD-Tree](KDtree.png)

### Portals