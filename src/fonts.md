# Installing fonts

Installing fonts is slightly different compared to installing regular system packages - **fonts must be installed under the fonts attribute**.

The general structure is as follows:

```nix
fonts.fonts = with pkgs; [
	# Fonts go here
];
```

Unfortunately, searching for font packages is _incredibly unintuitive_. Font packages look just like regular packages and adding them to the system packages _does not install them properly_. The 100% best safest way to find fonts is as follows:

1. Search for the font you're looking for on the [NixOS packages search website](https://nixos.org/nixos/packages.html#font)
2. Click on the package on the website (this shows a quick description of the package, along with various other information)
3. Make sure that the package's _Nix expression_ starts with `pkgs/data/fonts`

Then, as similar to installing system packages, add the **Attribute name** to the fonts section:

```nix
fonts.fonts = with pkgs; [
	comic-relief
	font-awesome_4
];
```
