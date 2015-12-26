
Building KTX
============

This document describes how to build `libktx` and the portable
KTX loader tests.

Status
------

Construction of a new build system and portable loader tests is
underway.  At present the Mac OS X, iOS and Windows(GL) builds of
`libktx` and `loadtests` are completed. There are three versions of
loadtests: GL3, ES1 and ES3. GL3 can be built on Mac and Windows.
ES1 can be built on iOS and with the PowerVR emulator on Windows.
ES3 can be built on iOS and with any of the major emulators on
Windows. Builds for for Linux and Android will follow.

Mac OS X and Windows builds of `toktx` are also completed. A build for Linux will follow.

Dependencies
------------

The KTX project uses GYP to generate project files. *You do not need
GYP unless you want to re-generate the supplied projects or generate
additional projects.*

The KTX library, `libktx`, and the KTX loader tests, `loadtests`, use
the _GL Extension Wrangler_ (GLEW) library when built for
OpenGL<sup>&reg;</sup> on Windows.

The MSVS solutions for Windows include OpenGL ES versions of
`loadtests`. To build a complete solution and run the OpenGL ES
version of the load tests you need to install an OpenGL ES emulator.

The KTX loader tests use libSDL 2.0.4. You do not need SDL if you
only wish to build `libktx`.

Binaries of all dependencies are included in the KTX Git repo.

### GL Extension Wrangler

Builds of GLEW are provided in the KTX Git repo.

#### Building GLEW from source

If you want to build GLEW from source you need the OpenGL core profile
friendly version, i.e, 1.13.0+. You can either clone
[the master GLEW repo](https://github.com/nigels-com/glew) and, following
the instructions there, generate the code then build it, or you
can download a pre-generated snapshot from https://glew.s3.amazonaws.com/index.html
and build that following the instructions also found in
[the master GLEW repo](https://github.com/nigels-com/glew).
The snapshot used for the binary included in this repo came from
https://glew.s3.amazonaws.com/index.html?prefix=nigels-com/glew/25/25.1/.

### OpenGL ES Emulator for Windows

The generated projects work with the
[ARM Mali emulator](http://malideveloper.arm.com/resources/tools/opengl-es-emulator/).
Install that before trying to build on Windows.

Projects can be modified to work with any of the major emulators;
[Adreno](https://developer.qualcomm.com/software/adreno-gpu-sdk/tools),
[ANGLE](https://chromium.googlesource.com/angle/angle/)<sup>*</sup>,
[Mali](http://malideveloper.arm.com/resources/tools/opengl-es-emulator/)
or [PowerVR](https://community.imgtec.com/developers/powervr/graphics-sdk/).
To use a different emulator change the selection at the bottom of
`gyp_include/config.gypi' and regenerate the projects. If you want to run
the load tests for OpenGL ES 1.1 you will need to use Imagination
Technologies' PowerVR emulator as that alone supports OpenGL ES 1.1.

<sup>*</sup>You will need to build ANGLE yourself and copy the libs
and dlls to the appropriate directories under `other_lib/win`. Note
that ANGLE's OpenGL ES 3 support is not yet complete.

### SDL

Builds of SDL are provided in the KTX Git repo.

#### Mac OS X

To use SDL on OS X, open a shell and enter the following command

```bash
cp -R other_lib/mac/<configuration>/SDL2.framework /Library/Frameworks
```

replacing `<configuration>` with your choice of `Debug` or `Release`. There
are alternative ways of using the provided SDL2.framework. See
`gyp_include/config.gypi` for details. You will have to regenerate the xcode
project if you wish to use one of these alternatives.

NOTE: xcode crashes when using the SDL 2.0.3 binary distributed by
https://libsdl.org. This is due to an issue with the code signature. The
same problem may affect the SDL2.framework included with KTX. The problem
and solution is described at [stackoverflow]
(https://stackoverflow.com/questions/22368202/xcode-5-crashes-when-running-an-app-with-sdl-2)
and is now supposedly fixed. If the problem occurs, use `codesign` to fix it,
as described in the second answer at stackoverflow.

NOTE: SDL 2.0.3 OS X support has a bug where clicking on a window title
sends an event to the application. This is fixed in SDL 2.0.4 and in
the SDL included with KTX.

#### iOS

Nothing need be done.

#### Windows

Nothing need be done.

#### Building SDL from source

The fork of SDL used by KTX can be found at https://github.com/msc-/SDL.
See the [README](https://github.com/msc-/SDL/README.md) there
for instructions.

Copy the results of your build to the appropriate place under the
`other_lib` directory.

### GYP

All the builds use project or make files generated with a modified version
of [GYP](https://github.com/msc-/gyp). To install GYP, follow the
instructions in the [README](https://github.com/msc-/gyp/blob/master/README.md)
there.

*You do not need GYP unless you want to re-generate the supplied projects
or generate additional projects.*

### Python

To clone the SDL Mercurial repo or to run GYP, you will need Python 2.7.
*You do not need Python otherwise.*

Visit the [Python Downloads](https://www.python.org/downloads/) page
to learn how to install Python for your OS.

On Windows you can use either the [native Windows version](https://www.python.org/downloads/windows/)
(recommended) or the version found in [Cygwin](https://www.cygwin.com).

If you install the native Windows version you must add `<python>` and
`<python>/Scripts` to the PATH environment in Windows. `<python>` represents
your actual install directory which defaults to `C:\Python27`.

### GNU make 3.81+

You need this to run the top-level `GNUmakefile` which runs GYP to generate the
various projects. It is possible to type the GYP commands manually, if you
really do not want to install GNU `make`. However, if you want to have GYP
generate makefiles to build the KTX project, you will need GNU `make` and
a Unix-style shell to run them.

On Linux, GNU make is available through the standard package managers in
most distributions. A suitable shell is standard.

On OS X, GNU make is included in the Xcode Tools available from
[developer.apple.com](http://developer.apple.com/tools/download/).
A suitable shell is standard.

On Windows, if you do not intend to generate makefiles to build KTX, you
can install a native Windows version of GNU make from
[GnuWin32](http://gnuwin32.sourceforge.net/packages/make.htm) and run
`make` in a Command Prompt (`cmd.exe`) window.

To get a Unix-like shell choose one of the following:

* install [Git for Windows](https://msysgit.github.io/) a.k.a `msysgit`
* install [GitHub for Windows](https://windows.github.com/)
* install [Cygwin](https://www.cygwin.com/) making sure to include `make` from
the *development* section.

The first two of these options include a copy of [MinGW](http://www.mingw.org/)
(Minimalist GNU for Windows). Sadly it is not the *same* copy; installing both
tools results in two copies of MinGW on your system. Neither copy includes
GNU `make`. You can download a pre-compiled version from the
MinGW project [32-bit](http://sourceforge.net/projects/mingw/files/MinGW/Extension/make/make-3.82.90-cvs/make-3.82.90-2-mingw32-cvs-20120902-bin.tar.lzma/download) or
[64-bit](http://sourceforge.net/projects/mingw-w64/files/External%20binary%20packages%20%28Win64%20hosted%29/make/make-3.82.90-20111115.zip/download).
Unpack the archive and you'll find a file called `mingw32-make.exe` (32-bit) or `make` (64-bit).

If using the Git for Windows shell (*Git Bash*), copy this to either

`%SystemDrive%\Program Files (x86)\Git\usr\bin\make.exe` (Windows 8.1 and 10)

(omit ` (x86)` if using 64-bit) or

`%USERPROFILE%\AppData\Local\Programs\Git\usr\bin\make.exe` (Windows 7)

:confused: I do not know if the difference in OS caused the different install locations
or if something else is at play.

If using the GitHub for Windows shell (*Git Shell*) copy this to

`%USERPROFILE%\AppData\Local\GitHub\PortableGit*\usr\bin\make.exe`

### Doxygen

You need this if you want to generate the _libktx_ documentation. You can download
binaries and also find instructions for building it from source at
[Doxygen downloads](http://www.stack.nl/~dimitri/doxygen/download.html).

You need to set the environment variable `DOXYGEN_BIN` in order for the project
files to find it at run time. `PATH` cannot be relied on because it seems to be
impossible to modify Xcode's default, unless you start it from the command line.

Setting any environment variables for Xcode is highly non-obvious
(thanks Apple :confounded:). See [osx-env-sync](https://github.com/ersiner/osx-env-sync)
to see how to set `DOXYGEN_BIN`. (`~/.MaxOSX/environment.plist` does not work
in recent versions of OS X, if it ever worked.).

Windows and Linux users shouldn't have any trouble setting this environment variable.

Building
--------

The KTX source distribution contains project files generated with GYP. At
present only xcode projects for Mac and iOS and MS Visual Studio projects for
Windows are included.

Use `build/xcode/ios/ktx.xcodeproject` to build the library and load tests to
run on iOS using OpenGL ES 3.0.

Use `build/xcode/macgl/ktx.xcodeproject` to build the library and load tests
to run under OpenGL 3.3 on OS X.

Use one of `build/msvs/win/vs20{08,10,10e,13,13e,15}/ktx.sln` to build the
library and load tests for Windows Win32 or x64.

Generating Projects
-------------------

To (re-)generate the projects run the following commands in a shell:

```bash
cd <your KTX clone>
make [xcode,msvs]
```

All important configuration options are gathered together in the file
`gyp_include/config.gypi`. Change these as necessary to suit your local
set up.


{# vim: set ai ts=4 sts=4 sw=2 expandtab textwidth=75:}