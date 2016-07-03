# Closest point to Triangle

Now that we can test if a point is inside a triangle or not, finding the closest point to a triangle becomes relativley simple. The process is as follows:

* Is the point inside the triangle?
  * If so, that point is the closest point
* Convert each edge of the triangle to a line
* Get the closest point on line
* Check the distance between the test point and all 3 closest points
* Return the point with the shortest distance.  

Given that a triangle already consists of 3 points ABC (p0, p1 and p2), getting the 3 lines of AB, BC and CA becomes very simple. Especially when you consider that the line constructor takes two points.

Checking which point is closest is easy too, consider the following:

```cs
Point GetClosest(Point test, Point test1, Point test2, Point test2) {
  // Get 3 vectors, going from the test point to all potential candidates
  Vector3 diff1 = test - test1; // Vector from test point to potentially closest 1
  Vector3 diff2 = test - test2; // Vector from test point to potentially closest 2
  Vector3 diff3 = test - test3; // Vector from test point to potentially closest 3
  
  // Find out how far away each point is using the difference
  // vectors computed above
  float distSq1 = diff1.LengthSq();  
  float distSq2 = diff2.LengthSq();
  float distSq3 = diff3.LengthSq();
  
  // Find the shortest one
  float min = Min(distSq1, distSq2);
  min = Min(max, distSq3);
  
  // Finally, return the result
  if (min == distSq1) {
    return test1;
  }
  else if (min == distSq2) {
    return test2;
  }
  return test3;
}

```