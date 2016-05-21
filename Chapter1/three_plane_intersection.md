# 3 Plane Intersection

Planes have a pretty special property. Given 3 unique planes, they intersect at exactly one point!

![SAMPLE](3plane_int.gif)

Any 3 dimensional cordinate system has 3 axis (x, y, z) which can be represented by 3 planes. Where those axis meet is considered (0, 0, 0) or the origin of the coordinate space.

Other than debug visualization, i've never used this intersection test in production. But it's useful to know in order to visualize some debug properties.

### The algorithm