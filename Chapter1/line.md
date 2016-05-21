#Line

Technically a line is the intersection of 2 planes. It runs infinatley in all directions. What we're going to calla  line is actually a __Line Segment__. It's a line between two points. Unlike a line, a line segment does have a length.

Given the above definition, defining a line is pretty simple:

```cs
class Line {
    public Point start;
    public Point end;
}
```

There is really no magic to a line. For example if you want to get the length of a line, it's the same as getting the length of a vector! You get a vector by subtracting start from end, then find the length!