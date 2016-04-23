# Console

Making a new console application is pretty easy. All you have to to is subclass ```Application``` and override the ```Main``` method. When you do this, make sure that you don't call ```base.Main```! ```base.Main``` is what would otherwise create the OpenTK window!

```cs
using System;

namespace CollisionDetectionSelector.Samples {
    class ConsoleSample : Application {
        public override void Main(string[] args) {
            Console.WriteLine("Testing, testing");
            Console.WriteLine("No window should pop up");
            Console.WriteLine("Press any key to quit");
            Console.ReadKey();
        }
    }
}
```