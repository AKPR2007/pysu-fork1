Python 3 Android
================

This is an experimental set of build scripts that will cross-compile the latest Python 3 git master for an Android device.

Build status:

| System            | Status        |
| ----------------- |---------------|
| Linux (GitLab)    | [![Build Status](https://gitlab.com/yan12125/python3-android/badges/master/pipeline.svg)](https://gitlab.com/yan12125/python3-android/pipelines) |

Prerequisites
-------------

Building requires:

1. Linux. This project might work on other Unix-like systems but no guarantee.
2. Android NDK r19 beta 1 or newer installed and environment variable ``$ANDROID_NDK`` points to its root directory. NDk r18 or below is not supported.
3. git and python3.9 in $PATH. It's recommended to use the latest git-master to build python3.9. Here are some ways to install the python3.9:
* For Arch Linux users, install [python-git](https://aur.archlinux.org/packages/python-git) package from AUR
* For other users, install 3.9 from [pyenv](https://github.com/yyuu/pyenv)
4. (Optional yet highly recommended) Vinay Sajip's [python-gnupg](https://bitbucket.org/vinay.sajip/python-gnupg) package for verifying PGP signatures of source tarballs and patches. You can install it with the following command:
```
python -m pip install --user python-gnupg
```
If pip is not installed, the ensurepip module is your friend:
```
python -m ensurepip --user
```

Running requires:

1. Android 5.0 (Lollipop, API 21) or above
2. arm, arm64, x86 or x86-64
3. A `busybox` binary at /data/local/tmp/busybox

Build
-----

1. `make clean` for good measure.
2. For every API Level/architecture combination you wish to build for:
   * Edit `pybuild/env.py` to match your (desired) configuration.
   * `make` to build everything!

Build using Docker
------------------

```
docker run --rm -it --user $(id -u):$(id -g) -v $(pwd):/python3-android yan12125/python3-android-base
```

Installation
------------

1. Make sure `adb shell` works fine
2. Copy all files in `build/sysroot` to a folder on the device (e.g., ```/data/local/tmp/python3```). Note that on most devices `/sdcard` is not on a POSIX-compliant filesystem, so the python binary will not run from there.
3. In adb shell:
<pre>
cd /data/local/tmp
. ./python3/tools/env.sh
python3.9
</pre>
   And have fun!

SSL/TLS
-------
SSL certificates have old and new naming schemes. Android uses the old scheme yet the latest OpenSSL uses the new one. If you got ```CERTIFICATE_VERIFY_FAILED``` when using SSL/TLS in Python, you need to generating certificate names of the new scheme:
```
python3.9 ./python3/tools/c_rehash.py
```
Check SSL/TLS functionality with:
```
python3.9 ./python3/tools/ssl_test.py
```


Known Issues
------------

No big issues! yay
