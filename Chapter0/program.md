#Program

The nice thing about this ```Program``` class is that it's automated. It scans your executable (assembly) for subclasses of ```Application```, and dynamically creates a selected subclass of ```Application```.

This magic is done trough reflection, using the built-in ```Assembly.GetTypes``` method. Because this class will never be edited, i'm just going to give you the code listing. If you are interested in how it works, ask me or do some google-ing on any code you don't recognize.

You select the subclass of ```Application``` based on class name of the subclasses!

### The code

The one thing we do here that we haven't done before is that the ```Program``` class is not in a specific namespace! This is not an error. C# has an anonymous global namespace. If you fail to specify a namespace for a class, it gets put into this global namespace. 

Both ```Program``` and ```Application``` live in the global namespace.

```cs
using System.Linq;

// Lives in global namespace
class Program {
    static void Main(string[] args) {
        System.Type[] samples = FindAndPrintApplicationChildClasses();
        System.Type selectedSampleType = null;

        if (samples == null || samples.Length == 0) {
            System.Console.WriteLine("No samples found, press any key to quit");
            System.Console.ReadKey();
            return;
        }

        if (args.Length > 0) {
            selectedSampleType = ParseSampleFromNumber(samples, args[0]);
        }

        System.Console.WriteLine("Select a sample, or q to quit");
        while (selectedSampleType == null) {
            System.Console.Write("Enter a sample number: ", System.ConsoleColor.White);
            string input = System.Console.ReadLine();
            if (!string.IsNullOrEmpty(input) && input.ToLower()[0] == 'q') {
                return;
            }
            selectedSampleType = ParseSampleFromNumber(samples, input);
        }

        Application game = (Application)System.Activator.CreateInstance(selectedSampleType);
        game.Main(args);
    }

    static System.Type[] FindAndPrintApplicationChildClasses() {
        System.Type[] samples = typeof(Application).Assembly.GetTypes().Where(t => t.IsSubclassOf(typeof(Application)) && t != typeof(Application)).ToArray();
        int blockSize = samples.Length / 2;
        for (int i = 0; i < blockSize; i++) {
            System.Console.WriteLine(string.Format("{2,2}. {0,-30}{3,2}. {1,-30}", samples[i].Name, samples[i + blockSize].Name, i, i + blockSize), System.ConsoleColor.DarkGray);

        }
        if (blockSize * 2 < samples.Length) {
            System.Console.WriteLine($"{samples.Length - 1,36}. {samples[samples.Length - 1].Name}", System.ConsoleColor.DarkGray);
        }
        if (samples.Length != 0) {
            System.Console.WriteLine();
        }

        return samples;
    }

    static System.Type ParseSampleFromNumber(System.Type[] samples, string input) {
        int number;
        if (!int.TryParse(input, out number)) {
            System.Console.WriteLine("Invalid format.", System.ConsoleColor.Red);
            return null;
        }

        if (number >= samples.Length || number < 0) {
            System.Console.WriteLine("Invalid number.", System.ConsoleColor.Red);
            return null;
        }

        return samples[number];
    }
}
```