# Your First Program

This tutorial will teach you how to write your first program. It assumes you have already followed the instructions on installing the compiler, which can be found [here](Installation.md).

## Setting up your workspace
Like many other languages, Cloth runs on its own VM (virtual machine). This means that you need to set up a workspace to store your programs.
To do this, run the following command in your terminal:

```bash
cloth init [name]
```

<note>Replace <emphasis>[name]</emphasis> with the name of your workspace!</note>

## Workspace layout
You will be presented with two files, <emphasis>src folder</emphasis> and <emphasis>Shuttle.toml</emphasis>.

The <emphasis>src folder</emphasis> contains your source code, while the <emphasis>Shuttle.toml</emphasis> file contains the configuration for your workspace.
Most of the time, you won't need to edit Shuttle.toml, as the build system will take care of it for you.

## Hello World!
Let's write our first program!

In the <emphasis>src folder</emphasis>, create a file called <emphasis>main.co</emphasis>.
<note>The file extension <emphasis>.co</emphasis> stands for Cloth Object.</note>

### Mods
In the <emphasis>main.co</emphasis> file, and all other files, you must define the location of your file in relation to the workspace root.
This is done by using the <emphasis>mod</emphasis> keyword.

```
mod src
```

<note>You do not always have to include src in the mod declaration. Since main.co is placed directly into the src folder, its mod is src.
You can change the location of this file, for example to <emphasis>hello_world</emphasis>, which would make the mod hello_world. Each identifier must
correlate to a folder, seperated by an ellipsis.</note>

### Main
Cloth, like Java, requires you to have a <emphasis>main</emphasis> method to run your program. The main method takes in two arguments, argc and argv.
**argv** is an array of strings, which contains the command line arguments passed to the program. **argc** is the number of arguments, which is a 32-bit integer.
The main method must return a 32-bit integer, which is the exit code of the program.

```
mod src

pub func main(argc: i32, argv: []string): i32 {
    return 0
}
```
If you were to run the program, you would get a successful exit code of 0. This is because the program does nothing and returns 0 as its exit code.

### Output
To print something to the console, you can use the <emphasis>println</emphasis> function. This function takes in any object or primitive type, and prints it to the console.
To use this function, you must import the <emphasis>cloth.io</emphasis> module.

You can do this in three different ways:

| Method                            | Description                             |
|-----------------------------------|-----------------------------------------|
| `import cloth.io.Io`              | Imports the entire cloth.io.Io module   |
| `import cloth.io.Io::{ println }` | Imports only the println function       |
| Fully qualified class name        | Use the library call without importing. |

#### Method 1:
```
mod src

import cloth.io.Io

pub func main(argc: i32, argv: []string): i32 {
    Io.println("Hello World!")
    return 0
}

```

#### Method 2:
```
mod src

import cloth.io.Io::{ println }

pub func main(argc: i32, argv: []string): i32 {
    println("Hello World!")
    return 0
}

```

#### Method 3:
```
mod src

pub func main(argc: i32, argv: []string): i32 {
    cloth.io.Io.println("Hello World!")
    return 0
}

```
All three methods will print "Hello World!" to the console and return 0 as its exit code. Each one achieves the same thing by
calling the println function from the cloth.io.Io module. The fully qualified class name is the only one that does not require importing.

## Running your program
To run your program, run the following command in your terminal:
```bash
cloth run

```
This will compile your program and run it, automatically finding the main method.

<warning>If you have multiple main methods, the first one it finds will be treated as the main method. To override this, you must declare the fully qualified path in the Shuttle.toml file!</warning>

## Conclusion
Congratulations! You have just written your first Cloth program! Cloth has many more features, and you can find out more about them in this documentation.
