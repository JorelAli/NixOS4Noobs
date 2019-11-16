# Overlays

Coming soon:

- Explaining how `self: super: { /* Code here */ }` works
- Using `super.callPackage`
- Using a `default.nix` file as an overlay for a folder
- Probably worth mentioning the difference between `super.callPackage blah {};` and `import blah;` and how `callPackage` works. (Probably including an example of `import blah with pkgs; {blah1, blah2, blah3, blah...}` and why `super.callPackage blah {}` is so much better).