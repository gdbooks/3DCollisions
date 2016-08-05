# Scene

Currently we have everything set up to create a transform hierarchy, but not a scene. A transform hierarchy is just a part of a scene. The scene provides a single interface for managint this crazy hierarchy. Let's make a new ```Scene``` class and implement a basic scene that will render it's transform hierarchy and perform a raycst on it.

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

Raycast however, we are not so lucky with. Implemen the 

```cs
 protected OBJ RecursiveRaycast(OBJ current, Ray ray) {
 ```
 
 helper function on your own. It should perform