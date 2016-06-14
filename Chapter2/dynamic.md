# Dynamic Intersections

Finding the collision between dynamic moving objects is a bit tougher than finding the collision between static objects. This is because of tunneling. Read [this article](https://www.aorensoftware.com/blog/2011/06/01/when-bullets-move-too-fast/) for a bit more in depth information on the problem.

Let's take a look at the problem using this image:

![TUNNEL](tunneling.jpg)

The object is moving a consistent amount of space each frame. The problem is, it moves right past the plane! So a Sphere to Plane intersection (the way we know it) will not catch this collision! 

Now, imagine turning that image 90 degrees to the left, the plane is the ground plane and the sphere is falling. It never hits the ground!

### Special Case

The sphere tunneling test has a simple solution, but the more complex the shape, the harder the solution. __In this chapter__ we are going to cover dynamic collisions of specific shapes that have 


At some point you have to start getting crazy!

### Generic Case