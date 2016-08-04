#Scene Management

We've covered scenes before with other games, but tehy are important, so we are going to re-visit them. A basic scene is just a hierarchy of transform matrices. We have two options to implement this. 

We can use a component style and have a generic component type of system, each component would have a reference to a mesh or some other. But for the sake of this chapter we're going to make each ```OBJ``` object into a node.

This is a very non-generic way to handle a scene, but we only need this scene structure to demonstrate a few core concepts.

