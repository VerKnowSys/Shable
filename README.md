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


# Example usage:
`bin/reign inventory test-reign hostname` - will execute tasks defined under `reigns/test-reign.task` on host `hostname` (defined in Ansible compliant inventory file).


# Debugging:
`DEBUG=1 bin/reign …`


# Testing Shable core functionality:
`bin/test` or `DEBUG=1 bin/test`


# License:
MIT/BSD license.
