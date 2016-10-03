#Scene Management

We've covered scenes before with other games, but they are important, so we are going to re-visit them. A basic scene is just a hierarchy of transform matrices. We have two options to implement this. 

We can either implement a component system, where we have a base game object that gets components such as meshes and scripts attached.

Or we can hard-code what belong into a scene.

The component system is preferable, but hard-coding is much faster. Back in the Doom days, hard-coding used to be the standard, not so much today.

Non-the-less we are going to hard code our scene, mainly because we're aiming for fast development time, and this isn't code that's going to ship.