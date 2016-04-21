# Point

```cs
class Point {
  protected Vector3 position;
  
  public Vector3 Position {
    get;
  }
  
  public float X { get; set; }
  public float Y { get; set; }
  public float Z { get; set; }
  
  public Point();
  public Point(float x, float y, float z);
  public Point(Vector3 v);
  
  public Vector3 ToVector();
  public FromVector(Vector3 v);
  
  public void Render() {
  
  }
}
```