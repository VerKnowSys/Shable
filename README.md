# Shable - Ansible replacement, written in pure Sh


## Requirements:
POSIX compliant system with /bin/sh at least compatible with BSD-4x version from 1989.


## Features:
* Pure Sh implementation using standard base system utils found on any modern POSIX OS.
* Support for [Pass](https://www.passwordstore.org/) vault utility (requires GPG2).
* Support for Pass vault values caching (to prevent "GPG2 password prompt flood" issue for parallel tasks).
* Support for Ansible inventory input format (no group support - processing all inventory entries by default).
* Support for Ansible templates - `{{ something }}` is supported, but without any logic whatsoever (it's not php to write logic on template side).
* Facts, Environment and Inventory values validation (explicit and done BEFORE task is invoked on remote host - since it should never leave remote in "unknown-in-middle-of" state).
* Clear, easy, eyes-friendly colored debugging (just define DEBUG in env and enjoy).
* Faster than anything else (has to be).
* Both synchronous and asynchronous (parallel) task processing support. (Provided helpers for both modes: `./call-reign-sync` and `./call-reign-async`).
* Support for tasks overloading added with v0.8 since Shable repo contains some shared/common base system files for FreeBSD and Linux.


## Usage examples:



Execute reign task on local system.
> "bin/shable" is invoked by "bin/reign" on remote hosts this way.

    bin/shable test-reign



Execute tasks defined under "reigns/test-reign.task" on host "kenny"
> Host params are loaded from Ansible-compliant "inventory" file.

    bin/reign inventory test-reign kenny



Execute synchronous (one by one), for each remote defined in inventory file.
> Shable helper script is called here. Consider such script a "helper".

    ./call-reign-sync test-reign



Execute asynchronously (all at once), for each remote defined in inventory file.
> Asynchronous helper is bit more complicated. It spawns everything and watches log file for each process result. If process fails it will attempts to retry multiple times or return an error(s) in report. In some cases this script will never finish and have to be interrupted.

    ./call-reign-async test-reign



Debugging:
> Shable is using ANSI coloring for output by default. If you don't like it, try disabling TTY feature of your terminal.

    DEBUG=1 bin/reign …


Testing Shable core functionality:

    bin/test
    or
    DEBUG=1 bin/test
    …



### Doing private Sibling repo for Shable?

`git clone --recursive https://github.com/VerKnowSys/Shable.Sibling Shable.YourCompany`

Then just define your facts, tasks, templates and stuff.. and have fun :)


## License:
MIT/BSD license.
