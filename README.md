mafia
=====

> Provides protection against cabal swindling, robbing, injuring or
> sabotaging people with chopsticks.

![mafia](img/mafia.jpg)

Fork Notes
----------

This is a fork of haskell-mafia/mafia. It isn't intended to stay in
sync, the point of an opinionated wrapper is that you like the
opinions, and these are mine.

A Note On Cabal vs Stack vs Mafia
---------------------------------

Mafia isn't intended as another haskell build tool (it isn't even one,
it uses Cabal/cabal-install for building, and it would not be a huge
stretch to have a stack port so it can use stack or cabal for
building).

Mafia is a set of conventions around haskell projects that make it
easier to develop against rigid workflows appropriate for building
production code (and not having that workflow or code bitrot). If you
try to use it for _arbitrary haskell project_ you won't have much
fun. If someone suggests that you use mafia over stack or cabal they
aren't even making a coherent statement.

The conventions allows: submodules for inline development of
dependencies (very useful in organisations where you have a number of
repositories, or you want to patch an upstream dependency and need to
incorporate it before it is released); More flexible usage of ghci,
cabal/stack repl's don't allow anything close to the same workflow,
this means that I can reload files across dependencies and multi
module projects with ease, but it requires disciplined practices to
ensure that all haskell modules are complete and don't need
information from cabal to build (extension flags etc...); allows
external caching of dependencies and sharing of dependencies across
sandboxes, this is less important out of rigid CI infrastructure where
you can take advantage of it, but it can help, cabal new-build could
potentially help with this but at the time of writing mafia's is a bit
more battle tested and doesn't break as much (or at all for my
situations), ultimately it would be nice to remove this from mafia if
cabal's can reach parity.

The point is that, mafia is a wrapper around a way of developing
haskell, if you want to develop in the same way it is a useful tool,
if you don't it isn't and it will get in your way. I don't want to see
anyone claim anything more or less.

Overview
--------

Mafia is a light weight but opinionated wrapper around Cabal that makes working
on Haskell projects fun and easy.

The central idea is that upon cloning a project, you should only have to
run `mafia build` to get up and running for development. This will pull
down any git submodules, create a cabal sandbox, install dependencies
and then build everything, tests and benchmarks included.

Cabal packages in the same git repository, or in git submodules, are
discovered automatically and made available as source dependencies. This
is particularly useful if you have many internal dependencies that are
not published to Hackage. It allows for development of a package and its
dependencies all at once, without having to publish intermediate builds
of libraries and without having to resort to a monorepo.

All dependencies are cached globally in `$HOME/.mafia/packages`
using a nix-like hashing system so that for a given set of transitive
dependencies, a package is only ever built once. Source dependencies are
also cached, using their dependencies and source code as the hash. This
is critical for Haskell development at scale. At Ambiata we have around
150 Haskell packages which are being developed at a rapid pace, so it's
crucial that we don't need to think about curating blessed sets of
packages that we can cache as a single snapshot.


* [Using Mafia For Your Own Projects](docs/using-mafia.md).
* [Mafia Command Reference](docs/command-reference.md).


System configuration
--------------------

Mafia expects both GHC and Cabal to be installed and on the `PATH`.

Follow the guides below to configure your system correctly:

- [Installing GHC](docs/installing-ghc.md)
- [Installing Cabal](docs/installing-cabal.md)
