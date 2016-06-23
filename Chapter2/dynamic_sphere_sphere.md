#Linear Impact Search

To solve the tunneling problem we can take intermediate steps into account. The brute force solution
would be testing the object stepwise displaced by fractions of the full movement.


The number of tests rises proportionally with speed. This is no problem for slow objects. Whereas
fast objects need many tests to avoid tunneling. Imagine a bullet fired at a wall. The high speed
would result in a great number of collision tests. Now imagine a full battalion firing at that wall. The
game's frame rate would start to crawl.
