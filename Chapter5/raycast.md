# Raycasting

This is where we see the acceleration power of the octree! Like with the BVH, we're going to re-direct raycasting against a scene to actually raycast against the octree. Also like BVH, this is the only intersection test we're going to accelerate. But do keep in mind, in a AAA engine, all intersections would happen trough the octree, not just raycasting.