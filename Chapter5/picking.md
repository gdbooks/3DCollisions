#Picking

Object picking is one of those problems that's fairly easy to solve, but for some reason no one ever knows how. What is picking? Click on a 3D scene, and the object you click on gets selected. So how to we accompolish this?

The user clicks on the screen. We find the pixel they selected on the near and far clip planes of the view frustum. We use these two points to construct a ray, then we do a raycast into the world using this ray. Whatever object collides with the ray (and has the lowest t value) is the object the user clicked on!

![IM8](image008.jpg)