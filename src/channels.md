# Nix channels

Nix channels are basically "where the downloaded packages come from". By default, on NixOS, packages are downloaded from the `nixos` channel (Is that really a surprise?)

In general, when it comes to channels, there are 3[^1] channels that are available:

| Channel | Example channels |
| ------- | ---------------- |
| Stable channels | `nixos-19.03` |
| Unstable channels (bleeding-edge) | `nixos-unstable` |
| Old channels | `nixos-18.03`, `nixos-17.09` |

## Using different channels in your configuration

There are two major methods of using different channels in your `configuration.nix` file. Which one you choose is entirely up to you, each have their own perks and drawbacks.

> **Warning**
>
> There are multiple different "defined" NixOS channels out there. Some are named `nixos`, whereas others are named `nixpkgs`. **Do not use nixpkgs channels on NixOS**. The `nixos` channels have different tests to the `nixpkgs` channels and are designed for the operating system as a whole, unlike the `nixpkgs` channels.

### Method 1: Using `nix-channel`

The `nix-channel` command is designed to manage multiple nix channels on your system. It's easy to update channels to the latest versions and add or remove channels.

* To list all channels that are available in your system, use the `--list` flag:
  ```
  $ sudo nix-channel --list
  ```

* To add a new channel (for example `nixos-unstable`), use the `--add` flag, followed by the URL of the channel and a name to identify it by. The list of channel URLs can be found at [https://nixos.org/channels/](https://nixos.org/channels/).

  ```
  $ sudo nix-channel --add https://nixos.org/channels/nixos-unstable unstable
  ```

* To update a channel, use the `--update` flag, followed by the channel name that you declared when you added the channel:

  ```
  $ sudo nix-channel --update unstable
  ```

Once you've added your desired channels, you can then add them to your `configuration.nix` file. This can be done using the Nix `let` expression, as followed:

```nix
{ config, pkgs, ... }:

let
	unstable = import <unstable> {};
in {
	# The rest of your configuration here

	environment.systemPackages = with pkgs; [
		# To use unstable packages, precede package names with unstable.
		# as defined in the "let ... in" section above
		unstable.hello
	];

};
```
### Method 2: Using git revisions

This method gives you much more control over what specific version of Nixpkgs you want to use on your system, however it means that updating channels (if you choose to do so) will also have to be done manually.

The only difference is that you require the following code structure:

```nix
import (builtins.fetchGit {
	name = "NAME";
	url = https://github.com/nixos/nixpkgs/;
	rev = "REV";
}) {};
```

The _NAME_ is some sort of descriptive name for your own benefit, and the _REV_ refers to the Git commit hash of your choosing (This is pretty easy to find on GitHub, it's shown as a 7-character string on the [Nixpkgs repository's commits](https://github.com/NixOS/nixpkgs) - clicking on the clipboard icon next to it copies the full hash to your clipboard)

An example of using this would be as follows:

```nix
{ config, pkgs, ... }:

let
	myNixpkgs = import (builtins.fetchGit {
		name = "nixos-unstable-2018-09-12";
		url = https://github.com/nixos/nixpkgs/;
		rev = "ca2ba44cab47767c8127d1c8633e2b581644eb8f";
	}) {};
in {
	# The rest of your configuration here

	environment.systemPackages = with pkgs; [
		# Exactly the same as the above example
		myNixpkgs.hello
	];
};
```

Alternatively, you can easily change all packages declared in the system packages to use your chosen git revision by changing the packages that are brought into scope after the `with` keyword:

```nix
environment.systemPackages = with myNixpkgs; [
	hello
];
```

## Finding information about current nix channels

The [NixOS channel update website](http://howoldis.herokuapp.com/) provides insights on when the various Nix channels were last updated, including the link to the latest commit that was included for that update. It's the quickest and easiest way for you to browse through the repository to find whether a specific package has been updated or not.

-----

[^1]: Technically there are 4 channels: Stable, Unstable, Old and _Small_. Small channels have much less packages, such as no desktop packages or packages for certain programming languages, as well as less tests for actual operating system usage. They're designed for servers running NixOS, where you want speedy updates. They normally build more stuff from source compared to other NixOS channels.
