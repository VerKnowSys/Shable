# Shable - Ansible replacement, written in pure Sh


## Requirements:
POSIX compliant system with /bin/sh at least compatible with BSD-4x version from 1989.


## Features:
* Pure Sh implementation using standard base system utils found on any modern POSIX OS.
* Support for [Pass](https://www.passwordstore.org/) vault utility (requires GPG2).
* Support for Pass vault values caching (to prevent "GPG2 password prompt flood" issue for parallel tasks).
* Support for Ansible inventory input format, including groups. (More about groups in examples below)
* Support for Ansible templates - `{{ something }}` is supported, but without any logic whatsoever (it's not php to write logic on template side).
* Facts, Environment and Inventory values validation (explicit and done BEFORE task is invoked on remote host - since it should never leave remote in "unknown-in-middle-of" state).
* Clear, easy, eyes-friendly colored debugging (just define DEBUG in env and enjoy).
* Faster than anything else (has to be).
* Both synchronous and asynchronous (parallel) task processing support. (Provided helpers for both modes: `./call-reign-sync` and `./call-reign-async`).
* Support for tasks overloading added with v0.8 since Shable repo contains some shared/common base system files for FreeBSD and Linux.


## Facts priorities:

1. Load `base.facts` (from Shable main repo first, then from sibling repo)
2. Load `SYSTEM_NAME.facts` (from Shable main repo first, then from sibling repo)
3. Load `facts/cached/local.facts` (if exists)
4. Load cached facts from facts/cached (if exists)
5. Load inventory facts (global, then host specific)

If there would be facts with same name, they will be overriden following this order.


## Few words about inventory files:

Shable supports idea of "global inventory variables" via special: _ like this:

```
_ pi=3.14159
_ phi=1.618033988749894848204586834

[default]
myhost1
myhost2 supports=dtrace
```

For any host you'd like to read values for, each will also get values of "pi" and "phi" injected.


## Usage examples:



Execute reign task on local system.
> "bin/shable" is invoked by "bin/reign" on remote hosts this way.

    bin/shable inventory-test test-reign



Execute tasks defined under "reigns/test-reign.task" on host "kenny" for "default" inventory group:
> Host params are loaded from Ansible-compliant "inventory" file.

    bin/reign inventory-test test-reign kenny



Execute synchronous (one by one), for each remote defined in inventory file.
> Shable helper script is called here. Consider such script a "helper".

    ./call-reign-sync test-reign



Execute asynchronously (all at once), for each remote defined in inventory file.
> Asynchronous helper is bit more complicated. It spawns everything and watches log file for each process result. If process fails it will attempts to retry multiple times or return an error(s) in report. In some cases this script will never finish and have to be interrupted.

    ./call-reign-async test-reign


Calling Reigns and inventory groups:
> Shable is using Ansible inventory format, groups are no different. The only difference is with group calling. Shable is using unix-like notation, for example: `inventory:group`

    # invoke shable reign locally: "test-reign" on local host:
    bin/shable inventory-test test-reign

    # invoke reign: "test-reign" on host: "myhost" of group: "mygroup" from inventory file: "inventory":
    bin/reign inventory:mygroup test-reign myhost


Debugging:
> Shable is using ANSI coloring for output by default. If you don't like it, try disabling TTY feature of your terminal.

    DEBUG=1 bin/reign …

Debugging per Shable function:
> Here we're loading shable functions and calling inner functions "inventory_hosts()" and "inventory_read()" directly:

Example 1:
    ${SHELL}        # load new shell since in case of error
                    # whole environment will be closed by error()
    . lib/shable    # load Shable functions

    # load hosts of group: "anything" from inventory file: "inventory-test" with debugging information:
    DEBUG=1 inventory_hosts inventory-test:anything

Example 2:
    ${SHELL}        # load new shell since in case of error
                    # whole environment will be closed by error()
    . lib/shable    # load Shable functions

    # load and inject values of user: "demo2" of group: "anything" from inventory file: "inventory-test"
    inventory_read inventory-test:anything demo2

    echo "${test3value}" # value from params of host "demo2" from inventory file
    # => 888



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
