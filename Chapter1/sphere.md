# Sphere

Other than a point, the next most basic 3D  shape. A sphere consists of a point and a radius. Because we're not going to actually expose a lot of the data underneath the sphere, i'm going to store the point as a ```Vector3```, and the radius as a ```float```, but you could store the point as a ```Point```

![SPHERE](sphere.jpg)


```cs
class Sphere {
    Vector3 point = new Vector3();
    float radius = 1f;
    
    public Point Position { get; set; } // Gets / Sets point vector
    public float Radius { get; set; } // Self explanatory
    
}
```