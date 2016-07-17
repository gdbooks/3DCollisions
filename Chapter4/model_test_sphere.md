#Model Sphere Collision

The actual testing of collisions gets a bit complicated, the sphere is the perfect primitive to explore this with. 

The first ting to know is that the model is in world-space, but it's collision primitives are in model space. That is, to test the AABB of the OBJ against a sphere accuratley, we must move the AABB into world space. We could do this by multiplying the min and max of the AABB by the ```worldMatrix``` of the OBJ.