
This line of code uses some LINQ magic to get a ```Type``` for each class that subclasses ```Application```.

```cs
System.Type[] samples = typeof(Application).Assembly.GetTypes().
      Where(t => t.IsSubclassOf(typeof(Application)) 
            && t != typeof(Application)).ToArray();
```

But we can break it down into much simpler code, it can be written as:

```cs
TODO
```

Let's go trough this line by line. TODO