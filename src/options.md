# NixOS configuration options

The NixOS configuration file also contains a section for other options to manage the system. This includes, but not limited to:

- Configuring the hardware setup
- Configuring TTYs
- Configuring security systems
- Configuring the way Nix builds your configuration
- Configuring the boot setup

Searching for these options is a little more tricky, but here are various ways to go about doing so:

## Method 1: Using the online options search

> This method is best for finding new options and finding examples of alternative values for options

The [Search NixOS options](https://nixos.org/nixos/options.html#) website is the best way to search for options. It includes descriptions of each option, along with its default value and sometimes an example of alternate values. Using options is as simple as declaring the **Option name** in your `configuration.nix` file and assigning a value to it:

```nix
hardware.bluetooth.enable = true;
```

## Method 2: Using the `nixos-option` command

> This method is best for finding where your current system's configuration settings are declared

The `nix-option` command lets you browse the options by name. Compared to using the website, this is a lot more tedious and slow.

```
$ nixos-option
This attribute set contains:
_module
appstream
assertions
boot
...

$ nixos-option hardware
This attribute set contains:
bladeRF
bluetooth
brightnessctl

$ nixos-option hardware.bluetooth
This attribute set contains:
enable
extraConfig
package
powerOnBoot
```

If you reach the final "leaf" node of an option, the `nixos-option` command will provide information about that option, as well as the current assigned value in your current system configuration. This is the best method for searching for options that are defined in your current system configuration, especially if you have your configuration split over multiple files.

```
$ nixos-option hardware.bluetooth.enable
Value: 
true

Default:
false

Example:
true

Description:
"Whether to enable support for Bluetooth.."

Declared by:
"/nix/var/nix/profiles/per-user/root/channels/nixos/nixos/modules/services/hardware/bluetooth.nix"

Defined by:
"/etc/nixos/configuration."
```

## Method 3: Using the Nix repl

> This method is best for finding the values of your current system's configuration

Opening a Nix repl with the `'<nixpkgs/nixos>'` parameter allows you to browse your current `configuration.nix` file as a parsed Nix expression. Compared to `nixos-option`, the Nix repl is a much faster way of traversing the set of system configuration options with its tab completion facility.

_(In the example below, wherever you see `[Tab]`, this means press the tab button)_

```nix
$ nix repl '<nixpkgs/nixos>'

nix-repl> config.[TAB]
config._module          config.i18n             config.programs
config.appstream        config.ids              config.security
config.assertions       config.jobs             config.services
...
```

You can also use the Nix repl to view the current values of your current system configuration:

```
$ nix repl '<nixpkgs/nixos>'

nix-repl> config.hardware.bluetooth.enable
true
```

## Method 4: Using `man configuration.nix`

The `man configuration.nix` command displays all options that is available for the `configuration.nix` file. It's basically the same as Method 1, except searching for certain options is a little bit more tricky and technical. Using the `/` key, it is possible to search the man page for a specific option.

## Method 5: Using other people's `configuration.nix` file

One of the best ways of figuring out how to use various NixOS configuration options is to look at other NixOS user's configuration files. [This website](https://search.tx0.co/?q=&i=nope&files=configuration.nix&repos=) is dedicated to a set of repositories of NixOS users and can be used to search for certain attributes in their specific configuration files by using the _File Path_ setting in the Advanced section below the main textbox.

This also includes links to their repositories to where the code is declared. In addition, [JorelAli's configuration](https://github.com/JorelAli/nixos/blob/master/configuration.nix) is a pretty good resource that is well documented.
