# Unsafe NixOS

> **Note:**
>
> The contents of this chapter are _heavily advised against_. In fact, they are beyond heavily advised against. **Do not** do the things outlined in this chapter. The sole reason this information is in NixOS4Noobs is to basically inform you to **never** do this stuff.

> **Author's Note:**
>
> I read somewhere that one of the best ways of teaching someone something is to basically show them what _not_ to do, before telling them the correct way of doing something. That way, they gain a better understanding of the underlying consequences and know how to mitigate them in future. I thought this principle can apply very well to NixOS, since I have personally done all of these things and have had to deal with all of the consequences.

## Unlocking the Nix Store

The Nix store, located in `/nix/store/` is where all of the Nix magic happens. Since this is basically the main back end for the Nix system, and all isolated installations of components are stored in this location, messing with the Nix store is not a good idea. Hence, it comes pre-built with a lovely lock on it to prevent anyone tampering with it.

Nonetheless, Nix lets you unlock the store by adding the following one line to your `configuration.nix` file:

```nix
nix.readOnlyStore = false;
```

This may seem super useful at first - it lets you fix up little derivations here and there and make everything work exactly as you want if something's a little off. However, this is not the case. Nix assumes that the Nix store is completely immutable and if something's not quite what Nix expects, Nix will basically have a melt down.

### Alternatives to unlocking the Nix Store

So now you know _never to use that option_, the main alternatives are to create derivations or to use Nix expressions that write to the Nix store in a pure manner. You already know how to create derivations according to [Chapter 4. Making Nix packages](./derivations.md).

#### Writing text files to the Nix Store

Using the `builtin` function `toFile`, it's surprisingly easy to create a file in a Nix expression:

```nix
builtins.toFile "filename.txt" ''
  file contents go here!
'';
```

This creates a file called `filename.txt`, with the provided file contents and stores it in the Nix store (fully hashed as you'd expect). The result of this function is the path which is created in the Nix store which refers to the file that was generated.

#### Writing executable text files to the Nix Store

Executable files can be written using the `pkgs`, which can be pulled into scope using the `with import <nixpkgs> {};` expression. The [writeTextFile](https://github.com/NixOS/nixpkgs/blob/master/pkgs/build-support/trivial-builders.nix#L30) function allows you to create a file with extra options such as:

- Declaring the file's folder which it is written to
- Declaring if the file should be executable or not
- Running a check phase (for example, to check for syntax errors)

```nix
with import <nixpkgs> {};
writeTextFile {
    name = "my-file";
    text = ''
        Contents of File
    '';
    executable = true;
    destination = "/bin/my-file";
}
```



