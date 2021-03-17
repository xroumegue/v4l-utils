# v4l-utils

Linux utilities and libraries to handle media devices (TV devices, capture
devices, radio devices, remote controllers).

You can always find the latest development v4l-utils in the git repo:
[http://git.linuxtv.org/v4l-utils.git](http://git.linuxtv.org/v4l-utils.git).

Those utilities follow the latest Linux Kernel media API, as documented at:
[http://linuxtv.org/downloads/v4l-dvb-apis/](http://linuxtv.org/downloads/v4l-dvb-apis/).

Any questions/remarks/patches can be sent to the linux-media mailinglist. See
[https://linuxtv.org/lists.php](https://linuxtv.org/lists.php) for more
information about the mailinglist.

There is also a wiki page for the v4l-utils:
[https://linuxtv.org/wiki/index.php/V4l-utils](https://linuxtv.org/wiki/index.php/V4l-utils).

## Building

Temporarily, both meson and autotools build systems are supported in parallel.
*NOTE*: Packagers should be aware that autotools support will be dropped soon,
to be fully replaced by meson.

A number of packages is required to fully build v4l-utils. The first step is to
install those packages. The package names are different on each distro.

On Debian and derivated distributions, you need to install the following
packages with `apt-get` or `aptitude`:

```
debhelper dh-autoreconf doxygen graphviz libasound2-dev libtool libjpeg-dev
qtbase5-dev qt5-default libqt5opengl5-dev libudev-dev libx11-dev pkg-config
udev gcc git
```

On Fedora, the package list for a minimal install with `dnf` or `yum` is:

```
git libtool gcc gcc-c++ which perl gettext-devel
```

(git is only requiried if you're cloning from the main git repository at
linuxtv.org).

And, to be able to compile it with all usual functionality with qt5, you'll need
also:

```
alsa-lib-devel doxygen libjpeg-turbo-devel qt5-qtbase-devel libudev-devel
mesa-libGLU-devel
```

### Meson build

Extra packages required for meson:

```
# On Debian/Ubuntu
apt-get install meson ninja-build

# On Fedora
dnf install meson ninja-build
```

After downloading and installing the needed packages on your distribution, you
should run:

```
meson build/
ninja -C build/
```

And, to install on your system:

```
sudo ninja -C build/ install
```

### Autotools build

Extra packages required for autotools:

```
# On Debian/Ubuntu
apt-get install autotools-dev autoconf-archive make

# On Fedora
dnf install automake autoconf make
```

After downloading and installing the needed packages on your distribution, you
should run:

```
./bootstrap.sh
./configure
make
```

If `./configure` exit with some errors try:

```
autoreconf -i --force
./configure
```

And, to install on your system:

```
sudo make install
```

### Optional features

Please notice that there's an extra feature to add an extra table to decode
Japanese DVB tables via iconv. This is meant to be used when the iconv itself
doesn't come with the *ARIB-STD-B24* and *EN300-468-TAB00* tables.

That requires not only the gconv package, but it also needs to be manually
enabled running autoconf with `--enable-gconv` option.

In meson, gconv is an auto feature, so it will be auto-enabled in case the
dependencies are satisfied. However, the gconv feature can be forced to enabled
by running the following command during configuration step:

```
meson configure -Dgconv=enabled build/
```

## Versioning

The v4l-utils doesn't quite follow the release versioning defined at
[semver.org](https://semver.org/).

Instead, since version 1.0, it uses `MAJOR.MINOR.PATCH`. Where:

* `MINOR` - an odd number means a development version. When the development is
closed, we release an even numbered version and start a newer odd version;

* `MAJOR` - It is incremented when `MINOR` number starts to be too big. The last
change occurred from 0.9.x to 1.0.

* All numbers start with 0.

All versions have their own tags, except for the current development version
(with uses the master branch at the git tree).

The `PATCH` meaning actually depends if the version is stable or development.

* For even `MAJOR.MINOR` versions (1.0, 1.2, 1.4, 1.6, ...): `PATCH` is
incremented when just bug fixes are added;

* For odd `MAJOR.MINOR` versions (1.1, 1.3, 1.5, 1.7, ...): `PATCH` is
incremented for release candidate versions.

### API/ABI stability:

There should not have any API/ABI changes when `PATCH` is incremented.

When `MAJOR` and/or `MINOR` are incremented, the API/ABI for the libraries might
change, although we do all the efforts for not doing it, except when inevitable.

The `TODO` files should specify the events that will generate API/ABI breaks.

## Media libraries

There are currently three media libraries defined at `lib/` directory, meant to
be used internally and by other applications.

### libv4l

This library is meant to be used by applications that need to talk with V4L2
devices (webcams, analog TV, stream grabbers).

It can be found on the following directories:

```
lib/libv4l1/
lib/libv4l2/
lib/libv4l-mplane/
lib/libv4lconvert/
```

See `README.libv4l` for more information on libv4l.

The libv4l is released under the GNU Lesser General Public License.

### libdvbv5

This library is meant to be used by digital TV applications that need to talk
with media hardware.

Full documentation is provided via Doxygen. It can be built, after configuring
the package with autotools, running:

```
make doxygen-run
```

In meson, Doxygen documentation is enabled by the auto feature: `doxygen-doc`.
If enabled, it will be built within the project.

It is possible to generate documentation in html, man pages and pdf formats.

The documentation is also available via web, at:
[http://linuxtv.org/docs/libdvbv5/](http://linuxtv.org/docs/libdvbv5/).

It can be found on the following directory `lib/libdvbv5/`.

The libdvbv5 is released under GPL version 2.

### libv4l2rds

This library provides support for RDS radio applications.

It can be found on the following directory `lib/libv4l2rds/`.

The libv4l is released under the GNU Lesser General Public License.

## Utilities

The utilities are stored under `utils/` directory.

The (for now for v4l-utils private use only) libv4l2util library is released
under the GNU Lesser General Public License, all other code is released under
the GNU General Public License.

v4l-utils includes the following utilities:

### decode\_tm6000

Decodes tm6000 proprietary format streams.

Installed under `<prefix>/bin`.

### ir-keytable

Dump, Load or Modify ir receiver input tables. The ir tables for remotes which
are known by the kernel (and loaded by default depending on dvb card type) can
be found under `utils/keytable/keycodes`.

v4l-keytable does not get installed during the install step.

### ir-ctl

A swiss-knife tool to handle raw IR and to set lirc options.

### qv4l2

QT v4l2 control panel application.

Installed under `<prefix>/bin`.

### rds-saa6588

Poll i2c RDS receiver [Philips saa6588].

rds-saa6588 does not get installed during the install step.

### v4l2-compliance

Tool to test v4l2 API compliance of drivers.

Installed under `<prefix>/bin`.

### v4l2-ctl

Tool to control v4l2 controls from the cmdline.

Installed under `<prefix>/bin`.

### v4l2-dbg

Tool to directly get and set registers of v4l2 devices, this requires a
*kernel >= 2.6.29* with the `ADV_DEBUG` option enabled. This tool can only be
used by root and is meant for development purposes only!

Installed under `<prefix>/sbin`.

### v4l2-sysfs-path

*FIXME* add description.

Installed under `<prefix>/bin`.

### xc3028-firmware

Xceive XC2028/3028 tuner module firmware manipulation tool.

xc3028-firmware does not get installed during the install step.

## Syncing with Kernel

There are a number of files on this package that depends on the Linux Kernel.

In order to make easier to keep it in sync, there's a target on this package to
do the synchronism.

For the sync to work, you need to run it on with 64 bits userspace and be sure
that glibc has the development package for 32 bits.

For Fedora, this is provided via this package: *glibc-devel.i686*

There are some steps required:

1. At the Kernel git tree:

    We need to sanitize the headers to be installed. To do that, you should run:

    ```
    make headers_install INSTALL_HDR_PATH=usr/
    ```

    This will create the dir `usr/` inside the Kernel tree.

1. Be sure that you have installed both glibc development packages for 32 and 64
bits, as otherwise the next step will fail.

1. At v4l-utils tree:

    ```
    ./sync-with-kernel.sh location/of/the/kernel/tree
    ```

    Alternatively, steps 1 to 3 can be replaced with:

    ```
    KERNEL_DIR=location/of/the/kernel/tree && (cd $KERNEL_DIR && make headers_install INSTALL_HDR_PATH=usr/) && ./sync-with-kernel.sh $KERNEL_DIR
    ```

1. Remove the `usr/` from the Kernel tree.
