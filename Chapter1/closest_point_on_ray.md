#Closest point on Ray

Finding the closest point on a ray is similar to finding the closest point on a line! When we check if a point is on a line segment, we do so by projecting the point onto an infinate line. Then, we clamp the projection to a range of 0 to 1. The 0 to 1 range is what makes the check be on a line segment.

A ray goes infinatley in one direction. If we change the formula to only cap to 0, the same formula will work! Becuase a ray has no length, the interpolated t value is useless.

Now you might think to yourself, a point has a start and end, a ray only has a start and direction, how do i get an end point?!?! Well, because we don't cap the interpolation at 1, the end point doesn't matter. It just has to be some point on the ray. You can easily get some point by adding the normal to the origin of the ray.