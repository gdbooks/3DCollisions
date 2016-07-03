# Sphere triangle test

Testing for the intersection of a sphere and a trianlge is amazingly simple. Get the closest point on the triangle to the center of the sphere. Check the distance (squared) between the center of the sphere and this point. If the distance is less than the sphere's radius (squared), then there is a collision.