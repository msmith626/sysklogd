Kernel and System Logging Daemons
=================================
[![License Badge][]][License] [![Travis Status][]][Travis]

Table of Contents
-----------------

* [Introduction](#introduction)
* [Build & Install](#build--install)
* [Building from GIT](#building-from-git)
* [Origin & References](#origin--references)


Introduction
------------

This is the continuation of the original Debian syslog daemon package
by [Martin Schulze][], it implements two system log daemons:

The `syslogd` daemon is an enhanced version of the standard Berkeley
utility program.  It is responsible for providing logging of messages
received from programs and facilities on the local host as well as from
remote hosts.

The `klogd` daemon listens to kernel message sources and is responsible
for prioritizing and processing operating system messages.  The `klogd`
daemon can run as a client of `syslogd` or optionally as a standalone
program.  `klogd` can now be used to decode EIP addresses if it can
determine a `System.map` file.

Main differences from the original sysklogd are:

- Built-in log-rotation support, with compression by default, useful for
  embedded systems.  No need for cron and a separate logrotate daemon
- FreeBSD socket receive buffer size patch
- Avoid blocking `syslogd` if console is backed up
- Touch PID file on `SIGHUP`, for integration with [Finit][]
- GNU configure & build system to ease porting/cross-compiling
- Support for configuring remote syslog timeout


Build & Install
---------------

The GNU Configure & Build system use `/usr/local` as the default install
prefix.  In many cases this is useful, but this means the configuration
files and cache files will also use that same prefix.  Most users have
come to expect those files in `/etc/` and `/var/run/` and configure has
a few useful options that are recommended to use:

    $ ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var
    $ make -j5
    $ sudo make install-strip

You may want to remove the `--prefix=/usr` option.


Building from GIT
-----------------

If you want to contribute, or just try out the latest but unreleased
features, then you need to know a few things about the [GNU build
system][buildsystem]:

- `configure.ac` and a per-directory `Makefile.am` are key files
- `configure` and `Makefile.in` are generated from `autogen.sh`,
  they are not stored in GIT but automatically generated for the
  release tarballs
- `Makefile` is generated by `configure` script

To build from GIT you first need to clone the repository and run the
`autogen.sh` script.  This requires `automake` and `autoconf` to be
installed on your system.

    git clone https://github.com/troglobit/sysklogd.git
    cd sysklogd/
    ./autogen.sh
    ./configure && make

GIT sources are a moving target and are not recommended for production
systems, unless you know what you are doing!


Origin & References
-------------------

This is the continuation of the original sysklogd by [Martin Schulze][].
Now maintained by [Joachim Nilsson][].  Please file bug reports, or send
pull requests for bug fixes and proposed extensions at [GitHub][].

[Martin Schulze]:   http://www.infodrom.org/projects/sysklogd/
[Joachim Nilsson]:  http://troglobit.com
[Finit]:            https://github.com/troglobit/finit
[GitHub]:           https://github.com/troglobit/sysklogd
[buildsystem]:      https://airs.com/ian/configure/
[License]:          https://en.wikipedia.org/wiki/GPL_license
[License Badge]:    https://img.shields.io/badge/License-GPL%20v2-blue.svg
[Travis]:           https://travis-ci.org/troglobit/sysklogd
[Travis Status]:    https://travis-ci.org/troglobit/sysklogd.png?branch=master
