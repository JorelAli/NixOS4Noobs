# Nix Expressions

The `configuration.nix` file is written as a _Nix Expression_, which is basically the "programming language" that Nix uses. In this chapter, we will cover the basics[^1] of writing Nix expressions, specifically for the NixOS configuration.

[^1]: In particular, we will cover the things that you're likely to discover in the `configuration.nix` file. We will not cover _all_ of the technicalities (e.g. function definitions), but we will cover enough to get you through the configuration.

-----

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
