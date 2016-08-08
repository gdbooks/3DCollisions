#Picking

Object picking is one of those problems that's fairly easy to solve, but for some reason no one ever knows how. What is picking? Click on a 3D scene, and the object you click on gets selected. So how to we accompolish this?

The user clicks on the screen. We find the pixel they selected on the near and far clip planes of the view frustum. We use these two points to construct a ray, then we do a raycast into the world using this ray. Whatever object collides with the ray (and has the lowest t value) is the object the user clicked on!

![IM8](image008.jpg)

The entire point of the OpenGL graphics pipeline is to take a world-space point and transform it to screen space coordinates. This is done using a variety of matrix multiplications. For picking we have to do the reverse! We have to take a screen space point and apply the reverse of the graphics pipeline to get two world space points.

Going from world space to screen space (What openGL does) is called __Projection__. Going from screen space to world space (What we need to do) is __Un-Projection__. Let's take a look at the projection pipeline: