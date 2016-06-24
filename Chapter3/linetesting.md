#Line Testing

When we talk about lines, especially in the context of line testing; we are actually talking about line segments. A line segment is a line that has a start point and an end point.

These tests are essentially capped ray-casts. Assume a line goes from p0 to p1. It has a length of L. If you do a ray cast from p0 to the direction of P1, and the returned t value is less than L, the collision is on the line segment. If t is less than 0 or greater than L, the collision is on the line, but outside the segment we are testing. 

The most common approach line testing is to simply create wrapper functions around our existing ray-cast functions. However, there are often early out cases that exist in a line test, which don't exist in a ray cast.

For this reason, sometimes line tests are implemented as code of their own. But, given how much of the logic the two tests share, there is no reason to re-invent the wheel. For this reason, we will be going with the simpler approach of creating wrapper functions.