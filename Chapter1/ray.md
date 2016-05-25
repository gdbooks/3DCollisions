#Ray 

A ray is similar to a line (not line segment). A ray is a single point in space with a direction. It extends infinatley far away in that single direction (as opposed to a line, which extends in both directions).

With the above definition, the code for a ray is pretty simple:

```cs
class Ray {
    public Point position;
    public Vector3 direction;
}
```

Much like line segments, rays are fairly straight forward objets. Because i ray is a directed line (not line segment tough), a lot of the collision code is going to be very similar to that of a line.

### Code Guide

The ```Ray``` class is straight forward. You can follow this guide for implementation, or make your own. Notably, i made the direction a private member called ```_normal```. I do this to ensure that it is normalized at all times. You might want to do that differently, this means you can't to this in my code:

```
ray.Normal.X = 7;
```

The reason i'm ok with that is you rarley set only a single component of a normal! More often than not you must set the whole normal anyway.

```cs

```