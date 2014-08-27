==================================================
Android
==================================================

rootfs
u-boot
vold / udev
dalvik(zygote)

- 加载动态库使用 /system/bin/linker 而不是 /lib/ld.so 。

- prelink 工具使用 apriori 而不是 prelink 。

- strip 工具使用 soslim 而不是 strip 。


--------------------------------------------------
Layer
--------------------------------------------------
Kernel

Runtime
Application framework

--------------------------------------------------
C Library
--------------------------------------------------
Android 使用 Google 自己开发的 Bionic Libc 而非 glibc （GNU C Library） 或者 uClibc （C Library for Embedded Linux）

- 采用 BSD 协议，非 glibc 和 uClibc 采用的 LGPL 协议。

- 大小更小，运行更快。

- 实现了一个更小，更快的 pthread 。

- 提供了一些 Android 所需的重要函数，如“getprop”，“LOGI”等。

- 不完全支持 POSIX 标准，比如 C++ exceptions ， wide chars 等。

- 不提供 libthread_db 和 libm 的实现。


--------------------------------------------------
Linaro 与 Android
--------------------------------------------------
Linaro


--------------------------------------------------
Repo
--------------------------------------------------

Repo is a tool that makes it easier to work with Git in the context of Android.

To install Repo:

Make sure you have a bin/ directory in your home directory and that it is included in your path:

.. code:: shell

  $ mkdir ~/bin
  $ PATH=~/bin:$PATH

Download the Repo tool and ensure that it is executable:

.. code:: shell

  $ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
  $ chmod a+x ~/bin/repo


--------------------------------------------------
Toolchain
--------------------------------------------------
Android 世界的四种 toolchain ：
It is possible to build toolchain with sysroot, but that is a special-purpose option. Here are short description of 4 types of toolchains available in Android world:

- arm-linux-androideabi toolchain.
  This is the main toolchain used for current builds. It differs from the bare-metal toolchain (see below) in that the target platform is arm-linux-androideabi, not arm-eabi.

- Bare-metal toolchain.
  In Android 2.x releases, this was used to build entire (monolithic) platform. This toolchain is built as decsribed above, without --with-sysroot switch. This is all that was needed to build platform, because libc, libm, etc. are built as part of the platform, and build takes care to pass explicit references to them to build other packages (it uses -nostdlib -libc, etc.). In newer releases, the arm-linux-androideabi toolchain is used instead. The bare-metal toolchain is still useful to build bits and pieces that are loaded before the OS is up, such as u-boot.

- Toolchain built using these instructions and --with-sysroot .
  Such toolchain is not needed during normal Android development process, which consists of building monolithic platform once, and then build (Java/Dalvik-based) applications for it using SDK/NDK. However, sometimes you need to build additional software which runs on system, platform level. One example is building busybox outside of normal platform build. Another is building official Android toolchain benchmark suite. For such cases, after you build monolothic platform, you can build a sysrooted toolchain for it. Note that you don't need to rebuild platform with this sysrooted toolchain - as described above, just bare-metal compiler needed to build platform, toolchain's libc is explicitly ignored during platform build using -nostdlib switch.

- NDK toolchain.
  This is used for Android application development. This is completely different layer from system, platform layer - all Android application are Dalvik managed once, any native-CPU code goes in form of JNI shared libraries. NDK toolchain is intended to build just such JNI shared libs. NDK toolchain is build separately from the platform toolchain, with other set of scripts (See "make ndk" in platform tree).


.. code:: shell

  $ repo init -u https://android.googlesource.com/toolchain/manifest
  $ repo sync


--------------------------------------------------
Compile Android system
--------------------------------------------------

Android.mk

config.mk
  build/core/config.mk
envsetup.mk
  build/core/envsetup.mk

BoardConfig.mk
  比如：
  build/target/board/generic/BoardConfig.mk
  device/generic/armv7-a-neon/BoardConfig.mk
  device/samsung/manta/BoardConfig.mk

linaro-build.sh

build-sysroot.sh


envsetup.sh
  build/envsetup.sh

vendorsetup.sh
  [device|vendor]/<PRODUCT_MANUFACTURER>/<PRODUCT_MODEL>/vendorsetup.sh
    device/generic/armv7-a-neon/vendorsetup.sh
    device/samsung/manta/vendorsetup.sh


Notes
--------------------------------------------------

Using Bash shell please.


Initialize
--------------------------------------------------

初始化编译使用的 shell 函数及自动完成设置。

.. code:: shell

  $ . build/envsetup.sh

or

.. code:: shell

  $ source build/envsetup.sh


Choose a Target
--------------------------------------------------

选择编译目标平台、设备，并设置相关的环境变量。

.. code:: shell

  $ lunch aosp_arm-eng

All build targets take the form BUILD-BUILDTYPE, where the BUILD is a codename referring to the particular feature combination.
Here's a partial list:

+-------------+--------------+-----------------------------------------------------------------+
| Build name  | Device       | Note                                                            |
+=============+==============+=================================================================+
| aosp_arm    | ARM emulator | AOSP, fully configured with all languages, apps, input methods  |
+-------------+--------------+-----------------------------------------------------------------+
| aosp_maguro | maguro       | AOSP, running on Galaxy Nexus GSM/HSPA+ ("maguro")              |
+-------------+--------------+-----------------------------------------------------------------+
| aosp_panda  | panda        | AOSP, running on PandaBoard ("panda")                           |
+-------------+--------------+-----------------------------------------------------------------+

and the BUILDTYPE is one of the following:

+------------+-----------------------------------------------------------------------------+
| Buildtype  | Use                                                                         |
+============+=============================================================================+
| user       | limited access; suited for production                                       |
+------------+-----------------------------------------------------------------------------+
| userdebug  | like "user" but with root access and debuggability; preferred for debugging |
+------------+-----------------------------------------------------------------------------+
| eng        | development configuration with additional debugging tools                   |
+------------+-----------------------------------------------------------------------------+


Build the Code
--------------------------------------------------

Assume you have 4 CPUs

.. code:: shell

  $ make -j4

Makefile -> build/core/main.mk -> Android.mk


OpenMAX
--------------------------------------------------

OpenMAX 是一个多媒体应用程序的标准。由 NVIDIA 公司和 Khrons 在 2006 年推出。

OpenMAX 是 Khrons 制定的 API ，这个 Khrons 也是 OpenGL 的制定者，同时在着手制定的标准规范还有 OpenGL es ， OpenVG ， OpenEL 等等，其中大多数都是针对手持设备和嵌入式设备的。

OpenMAX 自上而下分为三个层次： OpenMAX AL ， OpenMAX IL 和 OpenMAX DL 。

- OpenMAX AL
  Application Layer

- OpenMAX IL
  Integration Layer

- OpenMAX DL
  Development Layer


Android 源代码
--------------------------------------------------

${AOSP}/bionic                          Bionic Libc 库
${AOSP}/system                          嵌入式 linux 系统核心
${AOSP}/system/core                     核心底层
${AOSP}/system/core/libutils
${AOSP}/system/core/libcutils           系统级支持，线程、进程、 socket 等。
${AOSP}/system/core/init                init 启动程序

sp 定义
${AOSP}/system/core/include/utils/StrongPointer.h

wp 定义
${AOSP}/system/core/include/utils/RefBase.h

Binder 机制的 native 支持 （ libbinder ）
${AOSP}/frameworks/native/libs/binder

DECLARE_META_INTERFACEDECLARE_META_INTERFACE 和 IMPLEMENT_META_INTERFACE
${AOSP}/frameworks/native/include/binder/IInterface.h
