#Point in Triangle

## Same Side Test

The easyest way to test if a point is in a triangle is commonly refered to as the [Same Side](http://www.blackpawn.com/texts/pointinpoly/) technique. I suggest reading that article, only the Same Side portion, not the barycentric one. The only problem is, the method presented is a 2D method. In addition to the 3 side tests you must also check if the test point is on the plane formed by the triangle.

Given the above paragraph and the extra condition i gave you you could build a 3D point in triangle test, but it would be an expensive one. Instead, we can do the test much cheaper.

## Testing Normals

There is another intersection test we can perform, this one is easyer to read but harder to visualize. 

Given a triangle ABC and point P, we translate triangle ABC in a way that P lies on it's origin. The test now becomes checking if world origin is contained in the translated triangle (Still refered to as ABC).

P lies in the triangle ABC only if PAB, PBC and PCA are all clockwise, or counterclockwise. Because P is at the origin, this is now just a matter of checking cross products.

* Move the triangle so that the point becomes the triangle's origin space
* Create 3 new triangles between the triangle and the point (Making a pyramid)
* Check to see if all sides of the pyramid point in the same direction (Relative to world origin)

Let's see an example, we have a triangle and a point:

![PIT_1](pit_ex_1.png)

First, we translate the triangle so that the test point is it's origin:

![PIT_2](pit_ex_2.png)

Next we construct triangles PAB, PBC and PCA (You can only see the red and green ones, the blue one shares the same lines so it's not visible)

![PIT_3](pit_ex_3.png)

Now we check the normals of these 3 triangles

![PIT4](pit_sample_4.png)

Because all of the normals are facing the same way, the triangles are wound the same. Because of this, the point is inside the triangle!

This is what the same steps look like when the point is not in a triangle:

![BAD](nocol.png)

Notice how the normals in the final image all face out! This is the same image enlarged:

![PIT_5](pit_ex_5.png)

## The Algorithm

All of the above might sound a bit complicated, but its all really simple

```cs
bool PointInTriangle(Point p, Triangle t) {
  // Lets define some local variables, we can change these
  // without affecting the references passed in
  Vector3 p = point;
  Vector3 a = t.p0;
  Vector3 b = t.p1;
  Vector3 c = t.p2;
  
  // Move the triangle so that the point becomes the 
  // triangles origin
  a -= p;
  b -= p;
  c -=p;
  
  // Compute the normal vectors for triangles:
  // u = PAB
  // v = PBC
  // w = PCA
  
  Vector3 u = Cross(b, c);
  Vector3 v = Cross(c, a);
  Vector3 w = Cross(a, b);
  
  // Test to see if the normals are facing 
  // the same direction, return false if not
  if (Dot(u, v) < 0f) {
      return false;
  }
  if (dot(u, w) < 0.0f) {
      return false;
  }
  
  // All normals facing the same way, return true
  return true;
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
code
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/SAMPLE.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```
