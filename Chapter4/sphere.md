#Sphere

In the last section we built an AABB around out model, this actually gives us everything we need to build a bounding sphere.
The center of the sphere is the same as the center of the AABB. The radius of the sphere is either the min or max, whichever point is further away from the center of the sphere.