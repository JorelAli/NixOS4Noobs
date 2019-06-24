# Installing programs

There are many different ways of installing programs on NixOS. In this section, we will cover installing programs using the `configuration.nix` file.

## Installing programs using `programs = {}`

_Certain_ programs can be installed by using the following structure in your `configuration.nix` file:

```nix
programs = {

	# Programs go here
	a = true;
};
```
