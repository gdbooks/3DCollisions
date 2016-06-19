# Ray Against Sphere

Given the following image, we have :

![IMG1](raycast_image_1.png)

* Ray __R__, defined by:
  * point __p0__
  * normal __d__
* Sphere __S__, defined by:
  * center point __c__ 
  * radius __r__

Given this information, we want to find out if ray __R__ intersects sphere __S__. If the ray intersects the sphere, it's going to be along the normal. In the image below, __t__ is the length at which the normal intersects the sphere:

![IMG2](raycast_image_2.png)

An intersection happens at time __t__, along ray __R__. Another way to express this would be that the intersection happens at __R(t)__. To find __t__, we need to first find  __a__ and __f__. The formula for __t__ is:

```
t = length(a) - length(f)
```

In order to find __a__ and __f__, we need one more vector, vector __e__. Vector __e__ i the vector from __p0__ to __c__.

![IMG4](raycast_image_4.png)

In this image, we have two triangles we care about. 
* Triangle __f__, __b__, ___r__
* Triangle __a__, __e__, __b__

Given these two triangles, we can figure out the following

* Vector __e__ is a vector going to __p0__ to __c__
* Vector __a__ is vector __e__ projected onto vector __d__
* Vector __b__ is given by the pathegroen therom
  * $$A^2+B^2=C^2$$ : Therom
  * $$a^2+b^2=e^2$$ : Triangle __a__, __e__, __b__
  * $$b^2=e^2-a^2$$ : Re-arranged
  * $$b = sqrt(e*e - a*a)$$ :final
* Vector __f__ is given by the pathegroen therom
  * $$f^2+b^2=r^2$$ : Triangle __f__, __b__, __r__
  *  $$f^2=r^2-b^2$$ : Re-arranged
* Now we can solve for __t__
  * $$t=a-f$$ : algorithm for __t__
  * $$t=a - sqrt(r^2- b^2)$$ : Expanded __f__  
  * $$t=a - sqrt(r^2- e^2 + a^2)$$ : Expanded __b__, final

With that, we have the value of t! What happens when the ray doesn't hit the sphere? The expression inside the square root $$sqrt(r^2- e^2 + a^2)$$ will be negative. Therefore, we have to make an early out test for that!

## The Algorithm

```cs
void Raycast(Ray r, Sphere s, out float t) {
    // Known variables
    Vector p0 = r.Position;
    Vector d = r.Direction;
    Vector c = s.Position;
    Vector r = s.Radius;
    
    // Solve for a, b, f and e
    Vector e = c - p0;
    // Project e onto d
    Vector a = d * Dot(e, d);
    Vector b = e - a; 
    Vector f = r - b;
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
code
```

And provide an implementation for it!

### Unit Test

You can [Download](../Samples/Raycast.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```