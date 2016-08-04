#Scene Management

We've covered scenes before with other games, but tehy are important, so we are going to re-visit them. A basic scene is just a hierarchy of transform matrices. We have two options to implement this. 

We can use a component style and have a generic component type of system, each component would have a reference to a mesh or some other. But for the sake of this chapter we're going to make each ```OBJ``` object into a node.

This is a very non-generic way to handle a scene, but we only need this scene structure to demonstrate a few core concepts.

### Changine the OBJ class into nodes

Having a tree is all about having child and parent nodes. The first thing we're going to do is add a Parent reference, and a list of children to the OBJ class.

```cs
public OBJ Parent = null;
public List<OBJ> Children = new List<OBJ>();
```

__TODO: __ Now, adjust the Render, RenderBVH and RenderDebug functions to loop trough all children of the OBJ and call the appropriate Render function. Also, we're going to have OBJ's with a null model, be sure to add null checks. If an OBJ's internal model variable is null it will not render, but all of its children still should.