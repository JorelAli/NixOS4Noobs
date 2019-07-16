# Introduction

NixOS4Noobs is a tutorial which teaches you how to use NixOS. However, this isn't like most tutorials which teaches you _everything_, it instead just tries to teach a few things which you're very likely to encounter.

## Who this tutorial is for

Due to the large scale of NixOS and how it can be configured and set up, NixOS4Noobs will _only_ focus on **a configuration-based system**. This is where your entire operating system is controlled using the `configuration.nix` file and nothing else.

This tutorial targets the following users:

* Users of a device which only they will access _(not a device for multiple users)_
* Users that want to control NixOS using _just_ the configuration file
* Users that want everything installed system-wide

This tutorial **is not** for the following users:

* Those who use a device with a need for multiple users _(e.g. a family)_
* Users that want a clear separation between system-wide and local programs and services

## Why use a configuration-based system?

Having a configuration based system has various advantages, namely:

* It's easy to configure - everything is in one location
* If something goes wrong, it's easy to pinpoint where the problem is _(e.g. some dodgy installation)_
* It's a reproducible setup _(If you want to replicate a system on another device, just copy the configuration and it'll set it all up exactly as you want it to)_ 