# Setting up services

The NixOS configuration also allows you to manage services that run in the background, in addition to programs. 

## Installing NixOS services

Services can be found using the [NixOS options search website](https://nixos.org/nixos/options.html#services.) to find services that have been defined by the NixOS community. Services start with the prefix `services.` and can be enabled and configured in your `configuration.nix` file.

For example, to enable the `mpd` (Music Player Daemon) service, you can add it to your `configuration.nix` as follows:

```nix
services = {
	mpd.enable = true;
};
```
