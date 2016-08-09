#Picking

Object picking is one of those problems that's fairly easy to solve, but for some reason no one ever knows how. What is picking? Click on a 3D scene, and the object you click on gets selected. So how to we accompolish this?

The user clicks on the screen. We find the pixel they selected on the near and far clip planes of the view frustum. We use these two points to construct a ray, then we do a raycast into the world using this ray. Whatever object collides with the ray (and has the lowest t value) is the object the user clicked on!

![IM8](image008.jpg)

The entire point of the OpenGL graphics pipeline is to take a world-space point and transform it to screen space coordinates. This is done using a variety of matrix multiplications. For picking we have to do the reverse! We have to take a screen space point and apply the reverse of the graphics pipeline to get two world space points.

Going from world space to screen space (What openGL does) is called __Projection__. Going from screen space to world space (What we need to do) is __Un-Projection__. Let's take a look at the projection pipeline:

![IM49](Image49.gif)

We just have to do the inverse of this! Instead of object * model * projection, we do object * inverse(model) * inverse(projection). You get the idea!

[This article](http://antongerdelan.net/opengl/raycasting.html) actually does a really, really good job of explaining what we are about to talk about.

##Un-project

It's time to implement an "Unproject" function. I'm adding this to my __Collisions.cs__ file. 

```cs

```