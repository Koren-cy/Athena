## keyframe_001_50.7s (50.7s)

## 2.3 代码编译器gcc
## 2.3.1 gcc简介
gcc是GNU Compiler Collection的简称，是GNU(GNU's Not Unix)开源项目的编译器套件。gcc的初衷是为GNU操作系统专门编写的一款编译器，用于编译C代码。现如今已扩展为可以编译C++、Java、Objective-C等多种编程语言的集合。gcc本身也遵循GPL发行许可证，大名鼎鼎的linux就是基于gcc搭建的编译系统。gcc 官方网站(https://gcc.gnu.org/)

---

## keyframe_002_90.4s (90.4s)

murphy@ubuntu:/home/workspace/lesson\$ sudo apt-get install build-essential
Reading package lists... Done
Building dependency tree
Reading state information... Done
build-essential is already the newest version (12.8ubuntul.1).
0 upgraded, 0 newly installed, 0 to remove and 99 not upgraded.
murphy@ubuntu:/home/workspace/lesson\$ g

---

## keyframe_003_99.3s (99.3s)

Reading package lists... Done
Building dependency tree
Reading state information... Done
build-essential is already the newest version (12.8ubuntul.1). 0 upgraded, 0 newly installed, 0 to remove and 99 not upgraded. murphy@ubuntu:/home/workspace/lesson\$ gcc -v
COLLECT\_LTO\_WRAPPER=/usr/lib/gcc/x86\_64-linux-gnu/9/lto-wrapper
Configured with: ../src/configure -v --with-pkgversion='Ubuntu 9.4.0-1ubuntul\~20.04.2' --with-bugurl=file:///usr/share/ doc/gcc-9/README.Bugs --enable-languages=c,ada,c++,go,brig,d,fortran,objc,obj-c++,gm2 --prefix=/usr --with-gcc-major-ve rsion-only --program-suffix=-9 --program-prefix=x86 64-linux-gnu- --enable-shared --enable-linker-build-id --libexecdir =/usr/lib --without-included-gettext --enable-threads=posix --libdir=/usr/lib --enable-nls --enable-clocale=gnu --enabl e-libstdcxx-debug --enable-libstdcxx-time=yes --with-default-libstdcxx-abi=new --enable-gnu-unique-object --disable-vta ble-verify --enable-plugin --enable-default-pie --with-system-zlib --with-target-system-zlib=auto --enable-objc-gc=auto --enable-multiarch --disable-werror --with-arch-32=i686 --with-abi=m64 --with-multilib-list=m32,m64,mx32 --enable-mult ilib --with-tune=generic --enable-offload-targets=nvptx-none=/build/gcc-9-9QD0t0/gcc-9-9.4.0/debian/tmp-nvptx/usr,hsa - -without-cuda-driver --enable-checking=release --build=x86 64-linux-gnu --host=x86 64-linux-gnu --target=x86 64-linux-g nu
Thread model: posix gcc version 9π4.0 (Ubuntu 9.4.0-1ubuntu1\~20.04.2)
murphy@ubuntu:/home/workspace/lesson\$

---
