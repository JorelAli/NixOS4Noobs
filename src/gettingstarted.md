# Getting Started

## Editing the system configuration

As of NixOS 19.03 (Koi), you can open the system configuration in the default system editor with the following command:

```
$ sudo nixos-rebuild edit
```

Alternatively, you can edit the file located at `/etc/nixos/configuration.nix`


## Building the system configuration

In order to apply any changes to the `configuration.nix` file, the NixOS configuration needs to be _rebuilt_.

The `nixos-rebuild` command is used to rebuild the system configuration. In general, there are two commands you may want to run in order to properly build the system:

### Rebuild + Switch

```
$ sudo nixos-rebuild switch
```

Running this command performs the following:
- Rebuilds the current `configuration.nix` file
- Downloads any new packages
- Adds a new entry to the boot menu
- Applies the changes of the configuration right away (whilst you're still using the system. It's basically seamless!)

Any new packages can be used right away, any new services will be started right away, however _environmental variables will not be activated until the next reboot_.

### Rebuild (No switch)

```
$ sudo nixos-rebuild boot
```

This basically does everything that `sudo nixos-rebuild switch` does, except it does not apply the changes of the configuration right away. When the system is rebooted, the newly built configuration will be used.
