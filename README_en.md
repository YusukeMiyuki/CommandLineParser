# CommandLineParser

Original command line parser.  
You can set the Command line option, help message and so on by using Attribute.  

## Kind of Attribute

* CommandLineTargetClass
    This is used to Class.  
    Meaning: This Class has Command line option.

* CommandLineAttr
    This is used to Fields or Properties.  
    You can set the option name, whether it is required, and the optional help message by arguments. 
    Meaning: Fields or Properties are set Command line option arguments.  
    
## The Implemented Function
The implemented functions are as follows.

* Whether there are required options.
* Whether there is a help option.
* Whether there is an error.

## How to
The sample code is as follows.

```cs
[CommandLineTargetClass]
class Program
{
    [CommandLineAttr("i", true, "help: input file")]
    public static string InputFile { get; set; }

    [CommandLineAttr("o", false, "help: output file")]
    public static string OutputFile { get; set; } = @"C:\temp\outputFile.txt";

    static void Main()
    {
        var parser = new CommandLineParser(typeof(Program));

        if (parser.IsHelp)
        {
            Console.WriteLine(parser.HelpMessage);
            return;
        }

        if (parser.IsError)
        {
            Console.WriteLine(parser.ErrorMessage);
            return;
        }

        // main process start ...
    }
}
```

## The Help Message Format

ToolName Version Company Copyright  
The description of Tool

usage: ToolName RequiredOption { param } ... [ options ... ]

Option1\t: The help message of Option1.  
Option2\t: The help message of Option2.  
...

Display this message.  
-h  
-help


## Note
There are Limits as follows.

* The prefix when option is specified is fixed to "-".
* It only checks the required options.
* Does nothing to check the value of the specified optional arguments.
* Options cannot be specified more than once.
* Option name cannot be empty.
* Only "int", "double", "string", "bool" can store values in fields or properties.
* Help message command line option is fixed in "option name length order".
