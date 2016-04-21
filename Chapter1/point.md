# Point

The point is the simplest 3D primitive we will do 

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