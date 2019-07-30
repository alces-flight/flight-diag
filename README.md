# Flight Diagnostics

Provides diagnostic tools to aid HPC site engineers in their quest to
diagnose common issues encountered by users and to perform common
management tasks for management of user HPC workloads.

## Overview

Flight Diagnostics provides a single point of access to common
diagnostic utilities along with a set of conventions for defining
diagnostic tools in a safe and structured way, even when such tools
require superuser or other administrative privileges.

The tool allows HPC environment managers to give access to diagnostic
utilities that would require superuser access, while eliminating the
need for costly auditing and management procedures to ensure that the
underlying consistency and robustness of the HPC environment is not
compromised.

## Installation

```
yum install ruby
git clone https://github.com/alces-flight/flight-diag /opt/flight-diag
```

Add some `sudo` configuration, i.e. allow all users in `siteadmin`
group to run `diag` under `sudo` without a password:

```
cat <<EOF > /etc/sudoers.d/flight-diag
%siteadmin ALL=(root) NOPASSWD:/opt/flight-diag/bin/diag
EOF
```

## Configuration

When operating under `sudo`, log files are written to
`$APPROOT/var/log` by default. Modify `etc/config.yml` to change this
to a different location.

User-level log files are written to `$HOME/.flight-diag.log`.

Install additional diagnostic tools in `$APPROOT/libexec`. Tool
scripts in this directory have preamble that defines their
requirements, descriptive information, help text etc. See the default
tool scripts for the required format.

## Operation

Interactive operation:

```
[myuser@login1 ~]$ /opt/flight-diag/bin/diag
flight diag> help
Usage: COMMAND [OPTION...] [ARG...]
Perform diagnostic activities.

User commands:
  dmesg                Print the kernel ring buffer
  ls                   Access file listings via ls
  ps                   Access process listings via ps
  top                  Access process list via top
  whoami               Print user information for user commands

Privileged commands:
  logtail              (root) Tail a file from /var/log
  logview              (root) View a file from /var/log
  ukill                (root) Kill user processes
  whoami-su:USER       (supplied user) Print user information for privileged commands

Interactive commands:
  exit                 Exit the shell
  history              Show command history

For more help on a particular command run:
  help COMMAND

Please report bugs to <flight@alces-flight.com>
Alces Flight home page: <https://alces-flight.com/>
flight diag> whoami
Running as user:
  id: uid=1000(myuser) gid=1000(myuser) groups=1000(myuser)
  groups: myuser

Passed arguments:
  (None)
flight diag>
```

Batch operation:

```
[myuser@login1 ~]$ /opt/flight-diag/bin/diag whoami
Running as user:
  id: uid=1000(myuser) gid=1000(myuser) groups=1000(myuser)
  groups: myuser

Passed arguments:
  (None)
```

Note that access to privileged commands is granted by running the tool
using `sudo`.

# Contributing

Fork the project. Make your feature addition or bug fix. Send a pull
request. Bonus points for topic branches.

Read [CONTRIBUTING.md](CONTRIBUTING.md) for more details.

# Copyright and License

Eclipse Public License 2.0, see [LICENSE.txt](LICENSE.txt) for details.

Copyright (C) 2019-present Alces Flight Ltd.

This program and the accompanying materials are made available under
the terms of the Eclipse Public License 2.0 which is available at
[https://www.eclipse.org/legal/epl-2.0](https://www.eclipse.org/legal/epl-2.0),
or alternative license terms made available by Alces Flight Ltd -
please direct inquiries about licensing to
[licensing@alces-flight.com](mailto:licensing@alces-flight.com).

Flight Diagnostics is distributed in the hope that it will be
useful, but WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, EITHER
EXPRESS OR IMPLIED INCLUDING, WITHOUT LIMITATION, ANY WARRANTIES OR
CONDITIONS OF TITLE, NON-INFRINGEMENT, MERCHANTABILITY OR FITNESS FOR
A PARTICULAR PURPOSE. See the [Eclipse Public License 2.0](https://opensource.org/licenses/EPL-2.0) for more
details.
