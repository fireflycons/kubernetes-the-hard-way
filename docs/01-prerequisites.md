# Prerequisites

## VM Hardware Requirements

8 GB of RAM (Preferably 16 GB)
50 GB Disk space

## Virtual Box

Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) on any one of the supported platforms:

 - Windows hosts
 - OS X hosts
 - Linux distributions
 - Solaris hosts

## Vagrant

Once VirtualBox is installed you may chose to deploy virtual machines manually on it.
Vagrant provides an easier way to deploy multiple virtual machines on VirtualBox more consistenlty.

Download and Install [Vagrant](https://www.vagrantup.com/) on your platform.

- Windows
- Debian
- Centos
- Linux
- macOS

## Running commands in parallel with tmux

In situations where you need to run exactly the same commands on both masters or both workers (comes up in labs 7, 8), this enables you to enter the commands only once and they will be executed on both hosts. If you're not comfortable with tmux then in those labs you should run all the commands at the first host, then ssh to the second host and run the same commands again.

**The tmux way:**

Create a tmux configuration file

```
cat << EOF > ~/.tmux.conf
set -g default-shell /bin/bash
set -g mouse on
bind -n C-x setw synchronize-panes
EOF
```

To run a set of commands on two hosts simultaneously:

1. Run tmux
1. Hit `CTRL-B` followed by `"` to split the window horizontally
1. Select a pane using the mouse, and in that pane ssh to the other host (e.g. master-2)
1. Hit `CTRL-X` to enter synchronize-panes mode - now whatever you type or paste at one command prompt will be echoed at the other - i.e. the command is executed at both hosts.
1. To exit synchronize-panes mode, hit `CTRL-X` again

Next: [Provisioning Compute Resources](./02-compute-resources.md)

