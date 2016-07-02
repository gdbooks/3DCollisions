#3D Models

The basic primitive shapes will only get you so far in games. At some point you might need to figure out collisions against 3D models. Tough, it's considered best practice to avoid using mesh colliders, sometimes (Like environment collision) you absoluteley need them.

#Import Some Code

Before we begin implementing a 3D model, let's go ahead and add a way to load 3D models to the project. Pull in the [OBJLoader](https://github.com/Mszauer/OpenGL1X/blob/master/GameApplication/OBJLoader.cs) from the [OpenGL1X](https://github.com/Mszauer/OpenGL1X) project. Make sure to place the class in the correct name space. 

Once you have the class pulled in, go ahead and [Download susane.obj](). Here is some code to load up and display the model. Remember to set your working directory accordingly.

```cs

```