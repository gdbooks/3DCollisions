# Time Slicing

Tunneling is inherantly a problem of time. If your delta time is lower, the less chance you have of tunneling! Becuase of this, some games solve the tunneling issue by breaking up their update loop by seperating collision detection / resolution from update steps:

```cs
void Update(float deltaTime) {
    foreach(GameObject go in world) {
      go.Update(deltaTime);
    }
    
    foreach(GameObject go in world) {
      go.ResolveCollisions(deltaTime);
    }
    
    foreach(GameObject go in world) {
      go.Render();
    }
}
```

Now you are probably thinking this doesn't solve the problem, and as presented above, it does not. The key is, by breaking the collision code out of update, you can slice in more collision frames.

Think of it like this, if your delta time is 0.66, you can update collisions once with a slice of 0.66, __twice__ with a slice of 0.33, or __6 times every frame__ with a slice of 0.11!

```cs
float sliceMax = 0.033f;
void Update(float deltaTime) {
    foreach(GameObject go in world) {
      go.Update(deltaTime);
    }
    
    float thisSlice = deltaTime;
    foreach(GameObject go in world) {
      while (thisSlice > 0) {
        go.ResolveCollisions(thisSlice);
        thisSlice -= sliceMax;
      }
    }
    
    foreach(GameObject go in world) {
      go.Render();
    }
}
```

Notice how the collision resolution will be called multiple times per frame now! Of course this is not an end-all solution, sometimes you still need to querry collisions during the update phase, which will throw this whole system out of whack.

Fun fact, this is how Bethesda games do physics, and the problem of querrying collisions outside the collision resolution method is why they have some famous physics bugs!