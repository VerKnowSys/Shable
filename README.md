# Shable - Ansible replacement, written in pure Sh


# Requirements:
POSIX compliant system with /bin/sh at least compatible with BSD-4x version from 1989.


# Features:
* Pure Sh implementation using standard base system utils found on any modern OS.
* Support for Ansible inventory input format.
* Support for Ansible templates - `{{ something }}` is supported, but without any logic whatsoever (it's not php to write logic on template side).
* Facts, Environmnet and Inventory values validation (explicit and done BEFORE task is invoked on remote host - since it should never leave remote in "unknown-in-middle-of" state).
* Clear, easy, eyes-friendly colored debugging (just define DEBUG in env and enjoy).
* Faster than anything else (has to be).
* Async processing support.


# Example usage (single remote):
`bin/reign inventory test-reign hostname` - will execute tasks defined under `reigns/test-reign.task` on host `hostname` (defined in Ansible compliant inventory file).


# Example usage (sync, multiple remotes - all defined in inventory file):
`./call-reign-sync test-reign`


# Example usage (async, multiple remotes - all defined in inventory file):
`./call-reign-async test-reign`


# Debugging:
`DEBUG=1 bin/reign …`


# Testing Shable core functionality:
`bin/test` or `DEBUG=1 bin/test`


# Doing private Sibling repo for Shable?

`git clone --recursive https://github.com/VerKnowSys/Shable.Sibling Shable.YourCompany`

Then just define your facts, tasks, templates and stuff.. and have fun :)


# License:
MIT/BSD license.
