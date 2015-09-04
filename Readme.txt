README:
New armv6 hard float cross compiler tool chain for raspberry pi 1 running on x86 windows (gcc tool chain revision 5.2 from July 2015)



Hello,

I successfully built a cross compiler tool chain using the last ct-ng from the git repo.
As i did not found any cross compiler for windows with a gcc revision > 4.7 (2012) for armv6 (raspberry pi 1), i prefered to build my own tool chain.
It was not easier as the host is cygwin and the environment offered is not compatible with ct-ng.
So need to update the binutils for host script and the glibc source taken, to build without errors.

The details:
host: x86 cygwin
target: armv6-rpi-linux-gnueabi with hardfloat
gcc tool chain revision 5.2 (July 2015) for last standards (c++11 and 14) including c/c++/gfortran compilers
gdb/gdbserver revision 7.9.1 for debugging
glibc revision: 2.21
sysroot build for headers and libs.
dependencies: cygwin1.dll. You need to install cygwin and add this dll path to $PATH.

Attached an example picture with codeblocks running on Windows 7 using a remote debugging of the linpack benchmark code build with the crosscompiler.
Of course, you can use the IDE you want.
I added this example to show users that it is possible as it was my first need.

Comparison of the linpack code build using a native compiler from raspberry pi (gcc 4.6) and cross compiler (gcc 5.2 using hard float from the raspberry pi cpu):

native:  file linpack
linpack: ELF 32-bit LSB executable, ARM, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.26, BuildID[sha1]=0xab70ae18ea0d66d5ab5784bba66ec462d75c8be3, not stripped

cross-compiler: file Linpack-dynamic-release
Linpack-dynamic-release: ELF 32-bit LSB executable, ARM, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 3.2.27, BuildID[sha1]=0x4d92b05e1326911042eb7d30deb0a2bdc6cadacd, not stripped

native build execution: ./linpack
Enter array size (q to quit) [200]: 
Memory required:  315K.


LINPACK benchmark, Double precision.
Machine precision:  15 digits.
Array size 200 X 200.
Average rolled and unrolled performance:

    Reps Time(s) DGEFA   DGESL  OVERHEAD    KFLOPS
----------------------------------------------------
      16   0.50  92.00%   4.00%   4.00%  45777.778
      32   1.01  90.10%   1.98%   7.92%  47254.480
      64   2.03  90.64%   1.48%   7.88%  47001.783
     128   4.04  91.34%   4.21%   4.46%  45540.587
     256   8.07  86.99%   3.84%   9.17%  47963.620
     512  16.14  85.56%   4.89%   9.54%  48160.731

cross-compiler build execution: ./Linpack-dynamic-release
Enter array size (q to quit) [200]: 
Memory required:  315K.


LINPACK benchmark, Double precision.
Machine precision:  15 digits.
Array size 200 X 200.
Average rolled and unrolled performance:

    Reps Time(s) DGEFA   DGESL  OVERHEAD    KFLOPS
----------------------------------------------------
      16   0.50  84.00%   8.00%   8.00%  47768.116
      32   0.99  88.89%   1.01%  10.10%  49378.277
      64   1.98  87.88%   4.04%   8.08%  48293.040
     128   3.95  87.59%   3.54%   8.86%  48829.630
     256   7.88  88.20%   2.66%   9.14%  49102.421
     512  15.92  88.00%   4.08%   7.91%  47963.620

cross-compiler build check: readelf -A Linpack-dynamic-release
Section d'Attribut: aeabi
Attributs du fichier
  Tag_CPU_name: "ARM1176JZF-S"
  Tag_CPU_arch: v6KZ
  Tag_ARM_ISA_use: Yes
  Tag_THUMB_ISA_use: Thumb-1
  Tag_FP_arch: VFPv2
  Tag_ABI_PCS_wchar_t: 4
  Tag_ABI_FP_rounding: Needed
  Tag_ABI_FP_denormal: Needed
  Tag_ABI_FP_exceptions: Needed
  Tag_ABI_FP_number_model: IEEE 754
  Tag_ABI_align_needed: 8-octet
  Tag_ABI_align_preserved: 8-octet, sauf pour feuille SP
  Tag_ABI_enum_size: int
  Tag_ABI_VFP_args: VFP registers
  Tag_CPU_unaligned_access: v6
  Tag_Virtualization_use: TrustZone

Get the cross compiler tool chain for windows on github:
git clone https://github.com/Cheb57/armv6-rpi-linux-gnueabi.git

Have a nice build and enjoy your coding on pi !

Regards,
Rabia Chebah.
