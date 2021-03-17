# Installation

## Requirements

* meson and a C and C++ compiler
* optionally libjpeg v6 or later
* optionally Qt5 for building qv4l2

## Basic installation

v4l-utils can be built and installed using meson as follows:

```
meson build/
ninja -C build/
sudo ninja -C build/ install
```

## Configuration

You can get a summary of possible configurations running:

```
meson configure build/
```

To change the options values use the `-D` option:

```
meson configure -Doption=newvalue
```

After configuration you need to start the build process with:

```
ninja -C build/
```

More info about meson options:
[https://mesonbuild.com/Build-options.html](https://mesonbuild.com/Build-options.html)

## Installing

If you need to install to a different directory than the install prefix, use
the `DESTDIR` environment variable:

```
DESTDIR=/path/to/staging/area ninja -C build/ install
```

## Cross Compiling

Meson supports cross-compilation by specifying a number of binary paths and
settings in a file and passing this file to `meson` or `meson configure` with
the `--cross-file` parameter.

Below are a few example of cross files, but keep in mind that you will likely
have to alter them for your system.

32-bit build on x86 linux:

```
[binaries]
c = '/usr/bin/gcc'
cpp = '/usr/bin/g++'
ar = '/usr/bin/gcc-ar'
strip = '/usr/bin/strip'
pkgconfig = '/usr/bin/pkg-config-32'
llvm-config = '/usr/bin/llvm-config32'

[properties]
c_args = ['-m32']
c_link_args = ['-m32']
cpp_args = ['-m32']
cpp_link_args = ['-m32']

[host_machine]
system = 'linux'
cpu_family = 'x86'
cpu = 'i686'
endian = 'little'
```

64-bit build on ARM linux:

```
[binaries]
c = '/usr/bin/aarch64-linux-gnu-gcc'
cpp = '/usr/bin/aarch64-linux-gnu-g++'
ar = '/usr/bin/aarch64-linux-gnu-gcc-ar'
strip = '/usr/bin/aarch64-linux-gnu-strip'
pkgconfig = '/usr/bin/aarch64-linux-gnu-pkg-config'
exe_wrapper = '/usr/bin/qemu-aarch64-static'

[host_machine]
system = 'linux'
cpu_family = 'aarch64'
cpu = 'aarch64'
endian = 'little'
```

64-bit build on x86 windows:

```
[binaries]
c = '/usr/bin/x86_64-w64-mingw32-gcc'
cpp = '/usr/bin/x86_64-w64-mingw32-g++'
ar = '/usr/bin/x86_64-w64-mingw32-ar'
strip = '/usr/bin/x86_64-w64-mingw32-strip'
pkgconfig = '/usr/bin/x86_64-w64-mingw32-pkg-config'
exe_wrapper = 'wine'

[host_machine]
system = 'windows'
cpu_family = 'x86_64'
cpu = 'i686'
endian = 'little'
```
