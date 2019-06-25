# Nix garbage collection

With NixOS, it's important to clear the garbage now and then. Clearing the garbage removes the following things from your system:

- Old boot entries
- Unused derivations
- /nix/store/trash
- Stale symlinks

Clearing the garbage means that certain packages which were downloaded on the fly will be removed _(For example, packages downloaded using `nix-shell -p`)_, and will have to be re-fetched in order to use them again.

## Clearing all of the garbage

> **Warning**
>
> Clearing old boot entries makes rolling back to a previous configuration impossible. Only do this if you are certain that your current system is stable. **This action cannot be undone.**

Using the `nix-collect-garbage` command is used to manage all of your garbage cleaning needs. To remove everything from the Nix store that is not used by the current system (so, this includes old derivations, files, symlinks and boot entries), run the following command:

```
sudo nix-collect-garbage -d
sudo nixos-rebuild switch
```

Rebuilding the system at the end is required to update the set of boot entries.

## Clearing some of the garbage

Instead of clearing absolutely everything, it's often better to just clear the majority of the garbage. This doesn't clear the boot entries and ensures that previous generations are kept safe, so you are able to rollback if needed:

```
sudo nix-collect-garbage
```

## Automatically clearing garbage

NixOS provides a configuration option that allows it to automatically collect the garbage at certain time intervals. This is done using the `nix.gc` option:

```nix
nix.gc = {
	automatic = true;  # Enable the automatic garbage collector
	dates = "03:15";   # When to run the garbage collector
	options = "-d";    # Arguments to pass to nix-collect-garbage
};
```

The format for the `dates` attribute can be found at the [systemd.time man page](https://www.freedesktop.org/software/systemd/man/systemd.time.html).
