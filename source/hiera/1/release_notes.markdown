---
layout: default
title: "Hiera 1: Release Notes"
---



Hiera 1.3.0
-----

> **Release candidate:** Hiera 1.3.0 hasn't yet been released. RC1 was never published; RC2 was published on November 8, 2013.

Hiera 1.3.0 contains three new features, including Hiera lookups in interpolation tokens. It also contains bug fixes and packaging improvements.

Most of the features contributed to Hiera 1.3 are intended to provide more power by allowing new kinds of value interpolation.

### Feature: Hiera Sub-Lookups from Within Interpolation Tokens

In addition to interpolating variables into strings, you can now interpolate the value of another Hiera lookup. This uses a new lookup function syntax, which looks like `"%{hiera('lookup_key')}"`. [See the docs on using interpolation tokens for more details.](./variables.html#interpolation-tokens)

* [Feature #21367](http://projects.puppetlabs.com/issues/21367): Add support for a hiera variable syntax which interpolates data by performing a hiera lookup

### Feature: Values Can Now Be Interpolated Into Hash Keys

Hashes within a data source can now use interpolation tokens in their key names. This is mostly useful for advanced `create_resources` tricks. [See the docs on interpolating values into data for more details.](./variables.html#in-data)

* [Feature #20220](http://projects.puppetlabs.com/issues/20220): Interpolate hash keys

### Feature: Pretty-Print Arrays and Hashes on Command Line

This happens automatically and makes CLI results more readable.

* [Feature #20755](http://projects.puppetlabs.com/issues/20755): Add Pretty Print to command line hiera output

### Bug Fixes

Most of these fixes are error handling changes to improve silent or unattributable error messages.

* [Bug #17094](http://projects.puppetlabs.com/issues/17094): hiera can end up in and endless loop with malformed lookup variables
* [Bug #20519](http://projects.puppetlabs.com/issues/20519): small fix in hiera/filecache.rb
* [Bug #20645](http://projects.puppetlabs.com/issues/20645): syntax error cause nil
* [Bug #21669](http://projects.puppetlabs.com/issues/21669): Hiera.yaml will not interpolate variables if datadir is specified as an array
* [Bug #22777](http://projects.puppetlabs.com/issues/22777): YAML and JSON backends swallow errors
* [Feature #21211](http://projects.puppetlabs.com/issues/21211): Hiera crashes with an unfriendly error if it doesn't have permission to read a yaml file

### Build/Packaging Fixes and Improvements

We are now building Hiera packages for Ubuntu Saucy, which previously was
unable to use Puppet because a matching Hiera package couldn't be built.
Fedora 17 is no longer supported, and hardcoded hostnames in build_defaults.yaml
were removed.

* [Bug #22166](http://projects.puppetlabs.com/issues/22166): Remove hardcoded hostname dependencies
* [Bug #22239](http://projects.puppetlabs.com/issues/22239): Remove Fedora 17 from build_defaults.yaml
* [Bug #22905](http://projects.puppetlabs.com/issues/22905): Quilt not needed in debian packaging
* [Bug #22924](http://projects.puppetlabs.com/issues/22924): Update packaging workflow to use install.rb
* [Feature #14520](http://projects.puppetlabs.com/issues/14520): Hiera should have an install.rb


## Hiera 1.2.1

Hiera 1.2.1 contains one bug fix.

* [Issue 20137: Test for Puppet robustly](http://projects.puppetlabs.com/issues/20137)

## Hiera 1.2.0

Hiera 1.2.0 contains new features and bug fixes.

### Features

* Deep-merge feature for hash merge lookups. See [the section of this manual on hash merge lookups](./lookup_types.html#hash-merge) for details.
* [(#16644)](http://projects.puppetlabs.com/issues/16644) New generic file cache. This expands performance improvements in the YAML backend to cover the JSON backend, and is relevant to those who write custom backends. It is implemented in the Hiera::Filecache class.
* [(#18718)](http://projects.puppetlabs.com/issues/18718) New logger to handle fallback. Sometimes a logger has been configured, but is not suitable for being used. An example of this is when the puppet logger has been configured, but Hiera is not being used inside Puppet. This adds a FallbackLogger that will choose among the provided loggers for one that is suitable.

### Bug Fixes

* [(#17434)](http://projects.puppetlabs.com/issues/17434) Detect loops in recursive lookup

  The recursive lookup functionality was vulnerable to infinite recursion
  when the values ended up referring to each other. This keeps track of
  the names that have been seen in order to stop a loop from occuring. The
  behavior for this was extracted to a class so that it didn't clutter the
  logic of variable interpolation. The extracted class also specifically
  pushes and pops on an internal array in order to limit the amount of
  garbage created during these operations. This modification should be
  safe so long a new Hiera::RecursiveLookup is used for every parse that
  is done and it doesn't get shared in any manner.
* [(#17434)](http://projects.puppetlabs.com/issues/17434) Support recursive interpolation

  The original code for interpolation had, hidden somewhere in its depths,
  supported recursive expansion of interpolations. This adds that support
  back in.

