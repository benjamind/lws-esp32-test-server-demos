lws-esp32-test-server-demos
===========================

## For use with lws-esp32-factory

This relies on setup capability provided by

https://github.com/warmcat/lws-esp32-factory

which runs from the "factory" partition on ESP32.  This app is
designed to run from the 2.9MB OTA partition.

## New!

This includes the latest lws HTTP/2 support now, improved
memory management for headers, and mbedTLS wrapper fixes to
improve speed when multiple SSL connections are coming.

## About this demo

This demo is the standard lws test server using the standard lws test
protocol plugins.

When you open the page the html / png assets are served over http/2
or http/1 depending on how you connected.  Then the browser connects
back over http/1 and upgrades to ws.

## Build

1) This was built and tested against esp-idf at 0c50b65a34cd6b3954f7435193411a88adb49cb0,
from 2017-10-13.  You can force esp-idf to that commit by cloning / pulling / fetching
the latest esp-idf and then doing `git reset --hard 0c50b65a34cd6b3954f7435193411a88adb49cb0`
in the esp-idf directory.

Esp-idf is in constant flux you may be able to use the latest without problems but if not,
revert it to the above commit that has been tested before complaining.

2) Esp-idf also has dependencies on toolchain, at the time of writing it recommends this toolchain version (for 64-bit linux)

[1.22.0-73-ge28a011](https://dl.espressif.com/dl/xtensa-esp32-elf-linux64-1.22.0-73-ge28a011-5.2.0.tar.gz)

3) Don't forget to do `git submodule init ; git submodule update --recursive` after fetching projects like esp-idf with submodules.

4) After updating esp-idf, or this project or components, remove your old build dir with `rm -rf build` before rebuilding.

### Step 0: Install prerequisites

### 0.1: genromfs

For Ubuntu / Debian and Fedora at least, the distro package is called "genromfs"

Under Windows on MSYS2 environment you will need to separately build `genromfs` and add it to the path:

```
git clone https://github.com/chexum/genromfs.git
make
cp genromfs /mingw32/bin/
```

### 0.2: recent CMake

CMake v2.8 is too old... v3.7+ are known to work OK and probably other intermediate versions are OK.

Under Windows on MSYS2 environment you will need to install cmake: `pacman -S mingw-w64-i686-cmake`

### 0.3: OSX users: GNU stat

```
     $ brew install coreutils
```

### Step 1: Clone and get lws submodule

```
     $ git clone git@github.com:warmcat/lws-esp32-factory.git
     $ git submodule update --init --recursive
```

### Step 2: Build and Flash OTA Partition

```
 $ make flash_ota ; make monitor
```

## Using the lws test apps

See what IP your ESP32 got from your AP, the visit it in your browser
using, eg https://192.168.2.249

If your dhcp server provides your dns, you can also reach the device
using lws-serial, eg, https://lws-1234 or https://lws-1234.local

 - dumb increment should be updating at ~20Hz

 - mirror should let you draw in the canvas... open a second browser
   instance and they should be able to see each other's drawings

 - close testing should work

 - server info should reflect browsers open on the site dynamically

 - POST tests should pass the string and upload the file if one given

 - The button at the bottom should reset you into the setup / factory app

 - The "War and Peace" demo should load the 4MB text from the 1.2MB gzipped zip
   file directly, using gzipped data to the browser.
