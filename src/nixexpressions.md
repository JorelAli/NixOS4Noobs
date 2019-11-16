# Nix Expressions

The `configuration.nix` file is written as a _Nix Expression_, which is basically the "programming language" that Nix uses. In this chapter, we will cover the basics[^1] of writing Nix expressions, specifically for the NixOS configuration.

[^1]: In particular, we will cover the things that you're likely to discover in the `configuration.nix` file. We will not cover _all_ of the technicalities (e.g. function definitions), but we will cover enough to get you through the configuration.

-----

## Basic data types

As with any programming language, there are the basic data types, which are as follows:

* **Strings** - Text values which are encased in quotes `"hello"`, or two single quotes for multi-line strings:

  ```nix
  ''
  	This is on line 1
  	This is another line
  ''
  ```

  If you want to use Nix variables inside your strings, you can do so using the `${}` notation, like this[^2]:

  ```nix
  someSet = rec {
      myVar = "hello";
      someString = "${myVar}, world!"
  }
  ```
  
  [^2]: See the section below for recursive sets - since we reference `myVar` in the declaration of `someString`, the set needs to be declared as recursive.

* **Integers & Floats** - Numerical values, such as `2` or `501.23`. Integer and floating point numbers are inferred. If using math with integers, integer division will be used by default.

* **Paths** - Paths to files and directories don't need quotes, they can be written just like `/some/directory/file.txt`

* **Functions** - These basically execute code. These will not be covered in NixOS4Noobs.

* **Lists & Sets** - These are explained in more detail below

## Lists

In Nix expressions, **lists are declared using square brackets**, and use a **space to separate each element**.

### Example: System packages

The system packages section of the configuration file is represented as a list:

```nix
environment.systemPackages = [
	hello
	chromium
];
```

It can also be written like this (Typically a space is put after the `[` and before the `]`, but this is optional):

```nix
environment.systemPackages = [ hello chromium ];
```

Complex elements which would involve spaces require brackets to "contain" them:

```nix
environment.systemPackages = [
	hello
    (import ./someLocalPackage.nix {})
	chromium
];
```

## Sets (Attribute sets)

Sets are similar to tables with key-value pairs. They consist of elements declared by some key, and assigned a value which can be any type. **Sets are declared using curly brackets**, and a **semicolon is used to separate each key-value pair**. Sets are polymorphic (They can have multiple types for _values_, not for keys).

```nix
mySet = {
	name = "Bob";
	otherName = "Jim";
	aNumber = 2;
	isItWednesday = false;
}
```

### Example: System programs

Sets are used to declare programs for the system using the `programs` key:

```nix
programs = {
	adb.enable = true;
	bash.enableCompletion = true;
	ssh.forwardX11 = true;
};
```

### Sets inside sets

Because sets contain elements of keys and values, where values can be of any type, there's nothing stopping you from declaring a set with an element which is also a set.

```nix
mySet = {
	myKey = {
		someValueInInnerSet = 10;
	};
}
```

This can get tedious and overcomplicated, so we can simplify this using the dot notation:

```nix
mySet.myKey = {
	someValueInInnerSet = 10;
}
```

Or even simpler:

```nix
mySet.myKey.someValueInInnerSet = 10;
```

#### Example: Enabling sound in the system configuration

In most configurations, enabling sound is done using the following code:
```nix
sound.enable = true;
```

This can be written as:

```nix
sound = {
	enable = true;
}
```

There's no preference to which one is right and which one is wrong, it's entirely up to you. Some people like to split them up to make sections easier to read.

### Recursive sets (`rec`)

Sets that use variables or values that are declared inside itself need to be declared as a recursive set. This is done using the `rec` keyword, which comes _just before_ the opening curly brackets:

```nix
someSet = rec {
    someValue = 2;
    someOtherValue = 2 + someValue;
}
```

This also applies for anything used in Strings using the `${}` notation:

```nix
someSet = rec {
    myVar = "hello";
    someString = "${myVar}, world!"
}
```