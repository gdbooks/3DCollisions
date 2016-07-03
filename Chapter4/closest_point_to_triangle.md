# Closest point to Triangle

Now that we can test if a point is inside a triangle or not, finding the closest point to a triangle becomes relativley simple. The process is as follows:

* Is the point inside the triangle?
  * If so, that point is the closest point
* Convert each edge of the triangle to a line
* Get the closest point on line
* Check the distance between the test point and all 3 closest points
* Return the point with the shortest distance.  
