# Nix shell sandboxes

Say you want to test out a program, but don't want to go through all of the effort of adding it to your system configuration and rebuilding it. This is where Nix shell comes in[^1].


TODO:

- How to create them using $ nix-shell -p
- How to create them using shell.nix
- How to create pure environments using --pure
- A note about garbage collection and downloading
- https://github.com/haslersn/any-nix-shell

-----

[^1]: The Nix shell is capable of much more advanced things than just sandboxes, but for the sake of a NixOS for Noobs, we'll just assume it's used for sandboxes.
