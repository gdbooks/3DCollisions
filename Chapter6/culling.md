# Culling

We can now test robust enough intersections to perform vibility culling on a scene level! Whoo!!!! To achieve this, we're going to have to move the ```Rendering``` responsibility out of the ```OBJ``` class and into the ```OctreeNode``` class.

Add a new method to ```OctreeNode```. Call this method ```Render```. It will have the following signature

### Unit Test

You can [Download](../Samples/SAMPLE.rar) the samples for this chapter to see if your result looks like the unit test.

description of unit test

![UNIT](image)

```cs
code
```