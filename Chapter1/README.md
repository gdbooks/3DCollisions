##New Repo Time!

Now that we're done with OpenGL it's time to start a new repo on github. Let's call this one "CollisionDetection". Make the repo, clone it, add the appropriate .gitignore, commit the changes. Make a new Visual Studio solution, inside the cloned repo. The solution only needs to have 1 project in it.

Once you have a solution and project set up, put the math libary we wrote into it. Collisions are going to be math heavy. Also set up OpenTK. Even tough we will not focus on rendering, we will want to visualize this stuff.

##Shapes & Points

We're going to start out by implementing some basic shapes (We're going to make classes for each shape) and methods to test if a point is inside any of the shapes. This may seem simple but it's the basis of collision detection. In later chapters we're going to add functionality to our existing shape classes.

##Collision class
 
Aside from shapes and points, we're going to make a helper class called ```Collision```. This class will hold the actual collision logic. It's going to contain only public static methods. This is an example of how collisions will work when we're done:

```cs
// Integrate function causes moving sphere to rest on plane
void Integrate(Sphere sphere, Plane plane, float velocity) {
    // CollisionResult is a helper class, it contains information
    // about a collision that could happen
    CollisionResult collision = Collision.MovingSpherePlane(sphere, plane, velocity);
    if (collision.happened) {
        // Collision happened, offset the sphere to be above the plane
        sphere.position = collision.point - collision.incident;
    }
}
```

So, let's get started!

* Add a new ```Collision.cs``` file to the project
* For now, this class is going to be empty

##Main

I'm going to provide you with a main class, place the following code into __Program.cs__. It will let you run whatever unit test you need to.

TODO