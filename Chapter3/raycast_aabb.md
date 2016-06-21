# Raycast AABB

Raycasting an AABB is often done trough an algorithm called __Cyrus-Beck clipping__. You can read more about it [here](https://izzofinal.wordpress.com/2012/11/09/ray-vs-box-round-1/) and [here](http://gamedev.stackexchange.com/questions/18436/most-efficient-aabb-vs-ray-collision-algorithms). Because a ray that enters a box will intersect it twice, this algorithm gives both T1 and T2. We only care about the closer t value.

## The Algorithm

```cs
code
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

![UNIT](raycast_aabb_sample.png)

```cs
code
```