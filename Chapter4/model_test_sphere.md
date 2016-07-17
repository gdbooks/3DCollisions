#Model Sphere Collision

The actual testing of collisions gets a bit complicated, the sphere is the perfect primitive to explore this with. 

The first ting to know is that the model is in world-space, but it's collision primitives are in model space. That is, to test the AABB of the OBJ against a sphere accuratley, we must move the AABB into world space. We could do this by multiplying the min and max of the AABB by the ```worldMatrix``` of the OBJ.

The above solution is bad for a number of reasons. first, because all of the OBJ's share one ```OBJLoader```, where would such a transformed AABB live? In the OBJ, or the OBJLoader? Sure you could make the getter of OBJ do the work, but that might get messy.

Second, and more importantly, this method would only be viable for broad-phase primitives. Once you get to narrow-phase collision, multiplying each triangle by the worldMatrix becomes implausable.

A much better solution is to leave the model at the origin of the world, and move the primtiive being tested (in this case the sphere) such that relative to it, the model is at origin. 

We can achieve this by multiplying the position of the sphere by the inverse of the model's world matrix. This works, because if we where to multiply the models' workld position by the inverse of it's work matrix too, it would be at the origin of the world. In theory we translate both objects the same.

## The test

The test is simple, use the inverse world matrix of the model to create a new sphere whos center is translated. Then, do broad phase testing, if neither bounding sphere, nor bounding box intersect no intersection can happen. Finally, loop trough all the triangles, if one of them intersects and intersection did happen. If none of them do, no intersection happened.

I'm providing all the code needed for this one, there is no todo. Add the following test to ```Collisions.cs```

```cs
// Conveniance method
public static bool Intersects(OBJ model, Sphere sphere) {
    return Intersects(sphere, model);
}
    
public static bool Intersects(Sphere sphere, OBJ model) {
    Matrix4 inverseWorldMatrix = Matrix4.Inverse(model.WorldMatrix);

    Vector3 newSpherePos = Matrix4.MultiplyPoint(inverseWorldMatrix, sphere.Position.ToVector());
    // We have to scale the radius of the sphere! This is difficult. The new scalar is the old radius
    // multiplied by the largest scale component of the matrix
    float newSphereRad = sphere.Radius * Math.Max(
        Math.Max(model.WorldMatrix[0, 0], model.WorldMatrix[1, 1]),
        model.WorldMatrix[2, 2]);

    Sphere translatedSphere = new Sphere(newSpherePos, newSphereRad);

    // Broad-phase
    // If the bounding sphere does not intersect, nothing will
    if (!Intersects(model.BoundingSphere, translatedSphere)) {
        return false;
    }

    // Broad-phase
    // If the bounding box does not intersect, nothing will
    if (!Intersects(model.BoundingBox, translatedSphere)) {
        return false;
    }

    // Narrow-phase
    // At least one triangle must intersect!
    for(int i = 0, len = model.Mesh.Length; i < len; ++i) {
        if (Intersects(model.Mesh[i], translatedSphere)) {
            return true;
        }
    }

    // Narrow-phase
    // None of the triangles intersected, no intersection
    return false;
}
```

## Unit Test

You can [Download](../Samples/3DModels.rar) the samples for this chapter to see if your result looks like the unit test.

The constructor only errors out if the bounding sphere is off. Otherwise the rest of the test is visual only. You should see susane in the test

![UNIT](unit_test_obj_object.png)

```cs

```