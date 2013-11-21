---
layout: post
title:  Setup lxc on debian wheezy
categories: jekyll updates
---
# How to setup lxc on debian wheezy
If u have a debian wheezy server and would like to set up
[lxc](http://wwwhttp://linuxcontainers.org/) on it for
virtualization, you are going to have to do some extra steps before it works
fine. `LXC` package included in debian stable does not actually work so to resolve
the problem we are going to have to install the package from `jessie`, testing
version at this moment. 

To do so we first add the testing repository tou our `/etc/apt/sources.list`

    vim /etc/apt/sources.list

Add to the sources file testing repository, in this example i use german
servers, but you can add whatever suits you best.

    deb http://ftp.de.debian.org/debian/ testing main contrib

After this we need to run update and install packages we need.

    aptitude update
    aptitude install lxc bridge-utils libvirt-bin

Once this is done we can remove the testing repository from our sources file so
we don't mess up our server installation. Just remove or comment out the line
that we added previously.

There are more elegant ways to add testing repository, use it only for `lxc`
package and leave it there for updates of the package by using
[AptPreferences](http://wiki.debian.org/AptPreferences) that i will try to
explain in a future post.

# Finishing the installation

To have a complete working installation of `lxc` we need to prepare the host. We
need to add mount point for cgroup in `/etc/fstab`. Open it with your favorite
editor and add this line at the end

    cgroup /sys/fs/cgroup cgroup defaults 0 0

Once added to `fstab` we can either reboot our system or mount the newly added
`cgroup`

    mount /sys/fs/cgroup

If we want we can control our configuration by running

    lxc-checkconfig

At this moment everything should result ok in the check, only thing that might
be missing is `user namespace` but `lxc` works fine even if your config is
missing this parameter.

You can find extensive tutorials on how to create new containers so i will not
go in detail in this guide, but i will try to add a new post that explains how
to get a `debian wheezy` container, as the default template installs
`squeeze`.
