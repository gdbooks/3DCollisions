# Scene

Currently we have everything set up to create a transform hierarchy, but not a scene. A transform hierarchy is just a part of a scene. The scene provides a single interface for managing this crazy hierarchy. Let's make a new ```Scene``` class and implement a basic scene that will render it's transform hierarchy and perform a ray cast on it.

```cs
using OpenTK.Graphics.OpenGL;
using System.Collections.Generic;
using Math_Implementation;
using CollisionDetectionSelector.Primitives;

namespace CollisionDetectionSelector {
    class Scene {
        public OBJ RootObject = new OBJ(null);

        public void Render() {
            RootObject.Render();
        }

        public OBJ Raycast(Ray ray, out float t) {
            return RecursiveRaycast(RootObject, ray, out t);
        }

        public OBJ Raycast(Ray ray) {
            float t = 0.0f;
            return RecursiveRaycast(RootObject, ray, out t);
        }
}
```

We have a null object at the root of the scne. This is conveniant because now every single object will form a tree. The rendering function is fairly straight forward,  Because ```OBJ.Render``` is already recursive, so we just call it on the root object.

Raycast however, we are not so lucky with. Implement the 

```cs
protected OBJ RecursiveRaycast(OBJ current, Ray ray, out float t) {
 ```
 
helper function on your own. It should perform a ray cast against the current Obj being passed in. If the result of that ray cast is true, return the ```OBJ```. If the result of that ray cast is false, recursively call this function on all children of the ```OBJ```. If any of the children hit something, return the ```OBJ``` which was hit. If nothing was hit, return ```null```;
 
 We're going to have to modify the raycast obj function, in Collisions.cs, this one:
 
 ```cs
  public static bool Raycast(Ray ray, OBJ model, out float t) {
```

Change the function so it first checks if the model is empty ```model.IsEmpty```, if it is, return false by default.