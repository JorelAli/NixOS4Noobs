# Unsafe NixOS

> **Note:**
>
> The contents of this chapter are heavily advised _against_. These contain a list of poor practices for NixOS systems and are advised as a temporary measure - not to be used permanently. Use your own judgement of the situation before checking if these solutions are for you.

## Using the FHS

Sometimes, things _just don't work_ on NixOS. For example, say a developer has written a program that uses the an executable in the directory `/usr/bin/someExecutable`. Unfortunately, say this developer also hardcoded this directory path in their code. This means that we cannot change it, or patch it using normal means (such as creating a wrapper for it).

In order to bypass this, we can create a sandbox in NixOS that uses the Unix "Filesystem Hierarchy Standard" (FHS[^1]).

There are two methods of using the FHS:

### Declaring a FHS environment in the configuration

You can declare a FHS environment in your `configuration.nix` file. This allows you to enter this sandboxed environment using a command in any directory. This is done by using the `pkgs.buildFHSUserEnv` function to create a sandbox derivation.

#### Example: A C++ execution environment for LLVM

In this example, we use `pkgs.buildFHSUserEnv` to create a sandbox to aid in C++ development for other Linux systems. We include various debugging tools, such as `gdb` and `valgrind`, as well as the required libraries that we'll need, such as `llvm`.

```nix
environment.systemPackages = with pkgs; [
    (pkgs.buildFHSUserEnv {
        name = "cppfhs";
        runScript = "bash";
        targetPkgs = pkgs: with pkgs; [
        	clang_8 gdb llvm_8 valgrind
        ]; 
    })
];
```

We name this derivation `cppfhs`, which allows us to use the following command in any shell to quickly enter this environment:

```
$ cppfhs
```

-----

[^1]: The Filesystem Hierarchy Standard specifies directories such as `/usr/bin`, `/usr/lib` and `/bin`. On NixOS, these directories exist, but aren't in use as you'd expect with regular Unix systems.