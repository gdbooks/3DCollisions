# Further Research

We've covered the bare minimum of game collisions so far. With what we did cover, you should be able to create a basic game. For example, your character can walk on uneven terrain now, if you where to cats a ray from the top of the character down, and get the intersection point with the terrain mesh, you would know to plane the characters feet at that point.

The basiscs are enough to make some simple games, but at some point more advanced knowledge might be needed. I've listed out several other common collision shapes.

The real key here is, when working with a game engine, the work is often done for you. What you need to know is which functions exist and what they do, then you can figure out how to do it in engine.

For example, unity uses PhysX for both it's physics and collision detection. It has a [Raycast](https://docs.unity3d.com/ScriptReference/Physics.Raycast.html) method that already does the raycast against an optimized Octree for you. If you want to pick objects in unity, the engine provides a [Unproject](https://docs.unity3d.com/ScriptReference/Camera.ViewportPointToRay.html) function. You just have to call the built in Unproject and Raycast functions the same way you called your own functions.

The challenge from here on out is going to be figuring out how to achieve what you want to do in unity. What part of the math is implemented by unity for you, and what part you have to make from scratch. Unity will have most things already made tough, so long as you know how to use htem you should be fine.