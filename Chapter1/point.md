# Getting Started

Troughout this course, i'm going to be providing you with skeleton code for classes. __THIS CODE WILL NOT COMPILE__. You don't even have to use my code for implementation! The code is just there to give you an idea of what a class needs to have. It's pseudo-code.

__I will render it for you__. The difference being the ```Render``` function. Because this course is about collisions, not rendering, i will provide the rendering code for each primitive in full. 

# Point

The point is the simplest 3D primitive we will use. That makes it a good primitive to start with. A point is just a 3D touple, a collection of 3 numbers. Therefore, it makes sense to use a Vector3 to represent a point.

Remember, this layout is to give you an idea of WHAT the class is and should do. You don't HAVE to implement the class like this, just be sure to understand what the clas does.

```cs
class Point {
  protected Vector3 position = new Vector3();
  
  public Vector3 Position {
    get; // TODO (Make new vector)
    set; // TODO (Make new vector)
  }
  
  public float X { get; set; } // TODO: get, set
  public float Y { get; set; } // TODO: get, set
  public float Z { get; set; } // TODO: get, set
  
  public Point(); // TODO
  public Point(float x, float y, float z); // TODO
  public Point(Vector3 v); // TODO (Make new)
  
  public Vector3 ToVector(); // TODO (Return new)
  public FromVector(Vector3 v); // TODO (Make new)
  
  public void Render() {
  
  }
  
  public override string ToString() {
    return "(" + X + ", " + Y + ", " + Z + ")";
  }
}
```

The actual Vector3 that is the position is protected. All getters and setters should more often than not need to make a new internal vector. I've noted in comments which ones i think need to do this? Why? Let's take a look at the constructor that takes a vector.

Let's say we implement this constructor wrong, without creating a new vecotr:

```cs
public Point(Vector3 v) {
    position = v;
}
```

Now if we run this code:

```cs
void DoPointUnitTest() {
    Vector3 v = new Vector3(2, 3, 4);
    Point unitPoint1 = v;
    Console.WriteLine("p1: " + unitPoint1.ToString());
    v.X = 7;
    Point unitPoint2 = v;
    Console.WriteLine("p1: " + unitPoint1.ToString());
    Console.WriteLine("p2: " + unitPoint2.ToString());
}
```

The output of the unit test will be:

```
p1: (2, 3, 4)
p1: (7, 3, 4)
p2: (7, 3, 4)
```

As you can see, the X value of p1 would get changed. This is wrong. It happens because by doing a simple assignment like that, we're setting the ```position``` vector as a reference to the ```v``` argument. Essentially both variables point to the same vector!

Instead the constructor needs to create a NEW vector, using the values of ```v```

```cs
public Point(Vector3 v) {
    position = new Vector3(v.X, v.Y, v.Z);
}
```

This works better. The vector that ```position``` is assigned to was made right there on the spot. If we run the same unit test as above, the new output will be:

```
p1: (2, 3, 4)
p1: (2, 3, 4)
p2: (7, 3, 4)
```

And that is what we'd expect. The takeaway here is to ALWAYS pay attention to what you're assigning, a reference, a new object or a struct. And know what ramifications that assignement might have! When can you get away with a reference assign, and when do you need to do a deep copy that duplicates data into a new object. It's a lot to think about!

Go ahead and implement the rest of this class on your own. There isn't much to do, just implement the stubs (Or figure out how you would implement it and do that, like i said you don't have to follow my pseudo-code i give, but you certainly can).

## Unit Test

Finally unit tests. From here on out i'll be providing you with unit tests. Just make a new .cs file and paste the unit test verbatim in it. I'll provide a guide as to the console output you should see and what the visual scene should look like.

For the humble point, the unit test is this:

TODO