#Point on line

In order to determine if a point is on a line or not we need to know the slope intercept form of a line. That is, we need to express the line as the equation:

```
y = m * x + b
```

Im this equation m and b will be given. We plug in the X and Y position of the point being tested, if the result of the equation is true, the point will be on the line!

However, we express lines by two points, a start point and an end point. Not in slope intercept form. So we have to convert our line into said form. 

```m``` in the above equation is the slope of the line, slope is defined as rise over run, or ```rise / run```; that is, the y of the line divided by it's x. So if we convert the line to a vector (end - start), then the slope is said vectors y divided by it's x.

```
M = (line.end.y - line.start.y) / (line.end.x - line.start.x);
```



```b``` is the y-intercept. That is, it's the x value of the line where it crosses the y axis. We can find this with our slope, by subtracting the slope from the starting points y and multiplying it by the x value of the point

```
b = line.start.y - M * line.start.x;
```

If any of that is unclear, i suggest watching [this khan academy video](https://www.khanacademy.org/math/algebra-basics/core-algebra-graphing-lines-slope/core-algebra-equation-of-a-line/v/equation-of-a-line-3)

### The algorithm

Now that we can express a line in y intercept form, we can write some code to test if a point is on a line:

```
bool IsPointOnLine(Line line, Point point)  {
   float m = (line.end.y - line.start.y) / (line.end.x - line.start.x);
   float b = line.start.y - m * line.start.x;
   
   // At this point, evaluating the equation should be:
   // return point.y == m * point.x + b;
   // But that won't work, because we're working with floats
   // floating point error can accumulate, we have to use an epsilon
   
   // using an epislon, we can compare the distance of the 
   // equations result to 0 by the epsilon
   if (ABS(point.y - (m * point.x + b)) < EPSILON) {
       return true;
   }
   return false;
   
   // EPSILON is just a small number, like 0.001f;
}
```

## On Your Own

Add the following function to the ```Collisions``` class:

```cs
public static bool PointOnLine(Point point, Line line);
```

And provide an implementation for it!