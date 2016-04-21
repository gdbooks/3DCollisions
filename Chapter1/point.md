# Point

The point is the simplest 3D primitive we will use. That makes it a good primitive to start with. A point is just a 3D touple, a collection of 3 numbers. Therefore, it makes sense to use a Vector3 to represent a point.

Troughout this course, i'm going to be providing you with skeleton code for classes. __THIS CODE WILL NOT COMPILE__. You don't even have to use my code for implementation! The code is just there to give you an idea of what a class needs to have. It's pseudo-code.

__I will render it for you__. The difference being the ```Render``` function. Because this course is about collisions, not rendering, i will provide the rendering code for each primitive in full. 

```cs
class Point {
  protected Vector3 position = new Vector3();
  
  public Vector3 Position {
    get; // TODO
    set; // TODO
  }
  
  public float X { get; set; } // TODO: get, set
  public float Y { get; set; } // TODO: get, set
  public float Z { get; set; } // TODO: get, set
  
  public Point(); // TODO
  public Point(float x, float y, float z); // TODO
  public Point(Vector3 v); // TODO
  
  public Vector3 ToVector(); // TODO
  public FromVector(Vector3 v); // TODO
  
  public void Render() {
  
  }
}
```