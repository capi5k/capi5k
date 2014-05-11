Capi5k
=======

Capi5k is a **proof of concept** for making the deployments on Grid'5000
(and beyond) easier, reusable and shareable.
It is based on the use of modules (like nodejs modules, gems ...),
each module will install and configure something on nodes.
You can write your own modules and publish them so that everyone can reuse them.

It consists in a set of convention over [Capistrano (v2)](https://github.com/capistrano),
and [npm](https://www.npmjs.org/). It also makes use of
[xp5k](https://github.com/pmorillo/xp5k) for job submssion and deployment
on [Grid'5000](https://grid5000.fr).


Prerequisites
=============

* ruby environment. Use rvm or rbenv and bundler.
* npm : nodejs package manager is used to manage modules dependencies
* restfully configured :

```
$) cat ~/.restfully/api.grid5000.fr.yml
base_uri: https://api.grid5000.fr/3.0
username: "###"
password: "###"
```


Your first project
===================

Download the skeleton of a project.
It can be achieve by downloading the tarball / zip or cloning the repository
```capi5k-init```. See the links in the header.

Inside the repository, install the gem dependencies.

First the needed gems, we assume that bundle is installed.
 We encourage to use rvm or rbenv to manage your ruby environment.

$)cd capi5k-init
```

```
$)bundle install
[...]
Using term-ansicolor (1.3.0)
Using xp5k (0.0.5) from https://github.com/msimonin/xp5k.git (at redeploy)
Using bundler (1.5.3)
Your bundle is complete!
Use `bundle show [gemname]` to see where a bundled gem is installed.

```

You're done with the installation.


Setup
=====


Usage
=====


Roles
=====
