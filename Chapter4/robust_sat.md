# Robust SAT

The reason that our triangle - triangle collision has a false negative is because the two triangles are on the exact same plane. When this happens, the cross product of any of the vectors yields 0! 

We can actually fix this issue. First, we have to know if the result of the cross product is 0. Remember, floating point errors tend to creep up, so we have to check against an epsilon. The easyest way to do this is to check the square magnitude of the vector

```cs
bool isZeroVector = crossVector.LengthSquared() < 0.0001f;
```

## Constructing a new axis