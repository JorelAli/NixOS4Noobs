# Nix shell sandboxes

Say you want to test out a program, but don't want to go through all of the effort of adding it to your system configuration and rebuilding it. This is where the Nix shell comes in.

## Temporarily using programs

To temporarily bring a Nix package into the scope of your current terminal, you can use the `nix-shell -p` command. This lets you specify Nix packages which will be included in the "nix shell" environment, _**as well as your current packages**_.

```
$ nix-shell -p hello

[nix-shell:~]$ hello
Hello, world!
```

You are still able to modify the environment around you (e.g. create folders, edit files, open programs that are currently installed on your system). When you exit the nix shell (Using the `exit` command), that package is not present on your current system. For example, if I use `nix-shell -p hello`, the `hello` command is _only_ present in the nix shell even if it wasn't installed with my main configuration.

To add multiple packages, include their names (as if you were adding it to your `configuration.nix` file) with a space between each one:

```
$ nix-shell -p hello gnome3.gnome-mahjongg vitetris
```

## Pure Nix shell environments

By adding the `--pure` tag to the `nix-shell` command, you are able to create an environment which contains **none** of the packages installed on your current system. 

For example, if you have the following in your `configuration.nix` file:

```nix
environment.systemPackages = with pkgs; [
	hello
];
```

Running the `hello` command in a pure nix shell that doesn't have the _hello_ package will produce the following result:

```
$ nix-shell --pure -p

[nix-shell:~]$ hello
The program 'hello' is currently not installed. It is provided by several packages. You can install it by typing one of the following:
	nix-env -iA nixos.hello
	nix-env -iA nixos.mbedtls
	nix-env -iA nixos.perkeep
```

## Garbage collection of Nix shells

When a package is downloaded temporarily for use in the Nix shell, it is stored in the Nix store (in `/nix/store`) until garbage collected. Until it is garbage collected, opening the same Nix shell for downloaded programs will be instant (as they're already downloaded).

As stated in the chapter on garbage collection, running the Nix garbage collector will remove any temporarily installed programs. Once garbage collected, in order to use them again, they will be redownloaded when a Nix shell is opened that requires packages which aren't in the Nix store.

## Nix shells with `fish` or `zsh`

When using Nix shells, it can sometimes be useful to keep track of what packages are currently installed in that instance of the Nix shell. A helpful tool is the [any-nix-shell](https://github.com/haslersn/any-nix-shell), which shows the information of temporarily installed packages when using the fish shell or the z shell.
