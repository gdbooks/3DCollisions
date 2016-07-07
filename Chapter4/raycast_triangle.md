# Raycast Triangle

Raycsating against a triangle is a simple process

* Create a plane from the 3 points of the triangle
* Raycast against that plane
* Check if the raycast point is inside the triangle

You already have all the fnuctions needed to implement this in a rudimentary way. However, our Point In Triangle test is not optimal! Remember, when we did the point in triangle test, we did not use barycentric coordinates.

It's time to switch that. You can either implement barycentric coordinates straight up in the raycast triangle code, or you can change your point in triangle code and call that. If you change the point in triangle equation, make sure to run the unit test again to ensure everything still works.

Onto implementing barycentric coordinates! Watch this video:

{% youtube %}https://www.youtube.com/watch?v=EZXz-uPyCyA{% endyoutube %}