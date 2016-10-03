# Static Intersections

Testing if objects in rest intersect objects in motion is not the same. When objects are in motion, tunneling might occur. Non-the-less, it's important to know how to test for static intersections as the test are simpler.

Also, at some points even if objects are in motion, static intersection testing might be appropriate. A lot of times the most efficient way to test for a collison is to use a combination of ray-casting and static collision tests. This can be cheaper than full on dynamic collision testing.

For these reasons, let's go ahead and explore how to check for collisions between the primitive types we've declared.