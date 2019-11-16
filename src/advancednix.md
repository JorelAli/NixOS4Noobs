# Advanced Nix

So far, we've covered some very simple things that you can do with NixOS. For example:

- Using the `configuration.nix` file to configure the system
- Using the `nix-shell` command to create small shell sandboxes
- Using NixOS in general (clearing the garbage, managing channels etc.)
- Creating derivations

This chapter will cover the much more advanced topics of NixOS, in particular:

- Using _Modules_ to extend the `configuration.nix` file. Have you ever wanted to know where all of these options such as `xserver.displayManager.sddm.enable` came from? We will explain how to create your own custom configuration options that can be used in your `configuration.nix` file.
- Overriding packages by using _Overlays_. Spoiler alert: This basically lets you add custom packages into your configuration. These are one of the most complicated concepts to grasp, but once you get it, it's the most rewarding thing. Luckily, this is NixOS4Noobs and it's explained in such a way that anyone can understand it first time.
- Using the Nix Shell to its full potential by utilizing the `shell.nix` file. This lets us create Nix environments wherever to compile projects and run local programs.