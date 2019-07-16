# Setting environment variables

Environment variables can also be set using your `configuration.nix` file. This is done using the `environment.variables` setting:

```nix
environment.variables = {
	# Environment variables go here
};
```

## Single value environment variables

Environment variables with a single value can be defined using a simple assignment of the environment variable name and its corresponding value:

```nix
environment.variables = {
	XDG_CONFIG_HOME = "$HOME/.config";
};
```

## Multi value environment variables

Environment variables that use a list of values (e.g. `$PATH`, `$XCURSOR_PATH`) can be declared using a Nix Expression list type as shown:

```nix
environment.variables = {
	XCURSOR_PATH = [
		"$HOME/.icons"
		"$HOME/.nix-profile/share/icons"
	];
};
```

## Using the system path

To use the current system path which will be generated when building the NixOS configuration, you can use the Nix Expression string with enclosed Nix expressions:

```nix
environment.variables = {
	XCURSOR_PATH = [
		"${config.system.path}/share/icons"
	];
};
```

This will map `${config.system.path}` to the directory `/run/current-system/sw/` when the system is built.
