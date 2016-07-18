# Model AABB Intersection

We take a similar approach to the model AABB collision that we took to the model sphere collision. First, you find the inverse world matrix of the model. Next, construct a new AABB that is translated by this inverse matrix. Then do the collision waterfall:

* If the new AABB does not collide with the model's bounding box, there is no collision
* If the new AABB does not collide with the models bounding sphere, there is no collision
* If the new AABB collides with one of the models triangles, there is a collision
* By default, there is no collision

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