# Linux 内核发行版本 2.6.xx

<http://kernel.org/>

本文是 linux 2.6 的版本说明。本文主要介绍什么是 linux，如何安装 kernel 以及如果
出现问题该怎么处理。 

## 什么是 LINUX?

  Linux 是 Unix 操作系统的一个克隆，是由 Linus Torvalds 和一群网络黑客从头开始写
  的一个操作系统。Linux 的目标是更加符合 POSIX 和 Single Unix 规格标准。

  它拥有在现代成熟的 Unix 中所有的特性，包括真正的多任务处理、虚拟内存、共享库、
  加载器、执行程序的共享写时拷贝技术(STL)、优化的内存管理，还有多网络协议如 IPv4
  和 IPv6。

  它在GNU公共许可证的保护下发行。

## 硬件运行环境

  虽然最初的 Linux 运行在 x86 架构的 32 位机上，但是随着 Linux 的发展，目前至少
  可以运行下以下架构的处理器上：Compaq Alpha AXP, Sun SPARC and UltraSPARC,
  Motorola 68000, PowerPC, PowerPC64, ARM, Hitachi SuperH, Cell, IBM S/390,
  MIPS, HP PA-RISC, Intel IA-64, DEC VAX, AMD x86-64, AXIS CRIS, Xtensa,
  AVR32 and Renesas M32R architectures.

  只要含有分页机制的内存管理单元(PMMU) 和 GNU C 编译器接口(The GNU Compiler
  Collection 的组成部分，GCC)，Linux 就能很容易的移植到主流的 32 或 64 位体系结
  构的处理器上。Linux也能在一些没有 PMMU 的架构上进行移植，虽然功能性上面或多或
  少有所限制。

  Linux 还能移植到它自己身上，可以像用户空间的应用程序一样运行内核 -- 这叫 "用户
  模式 Linux" (UserMode Linux，简称 UML)。

## 文档

 - 已经有大量针对 Linux 或关于通用 Unix 问题的文档，或以电子档的形式在网络上、或
   以书本的形式发行。这里建议在任何地方分享的 Linux 的文档都值得好好看看，这份注
   记不是作为系统的文档而提供的，有很多更好的资源适合你去阅读。

 - 在 Documentation/ 的子目录下有许多不同的 README 文件，比如其中会有关于一些驱
   动的内核安装说明。Documentation/00-INDEX 文件中包含关于各文件主要内容介绍的索
   引。也看下 Changes 文件，其中有关于升级内核的一些信息。

 - Documentation/DocBook/ 子目录下包含一些内核开发人员和用户的指南。这些指南提供
   多种格式：PostScript(.ps), PDF, HTML, 和 man-pages 等。内核代码安装好之后，可
   以通过使用 "make psdocs", "make pdfdocs", "make htmldocs" 或 "make mandocs"
   命令来生成相应格式的文档。

## 安装内核源码

 - If you install the full sources, put the kernel tarball in a
   directory where you have permissions (eg. your home directory) and
   unpack it:
   
 - 如果安装所有的源代码，将压缩包放在某个你有相应权限的目录下(在你的 home 目录下
   就可以了)，然后解压：

		gzip -cd linux-2.6.XX.tar.gz | tar xvf -

   或者

		bzip2 -dc linux-2.6.XX.tar.bz2 | tar xvf -

   其中 "XX" 指对应的内核版本号(一般升级时我们都选择最新的内核版本)。

   请不要使用 /usr/src/linux 这个区域。这个区域包含库头文件需要使用的大量(通常不
   完全)的内核头文件。它们必须与库相匹配，而不应该因为内核变更弄的一团糟。

 - 你可以通过打补丁在 2.6.xx 发布版之间升级。补丁以传统的 gzip 格式和较新的
   bzip2格式提供。打补丁时，先下载所有新的补丁文件，进入内核源码的顶层目录，然后
   执行以下命令：

		gzip -cd ../patch-2.6.xx.gz | patch -p1

   或者

		bzip2 -dc ../patch-2.6.xx.bz2 | patch -p1

   (repeat xx for all versions bigger than the version of your current
   source tree, _in_order_) and you should be ok.  You may want to remove
   the backup files (xxx~ or xxx.orig), and make sure that there are no
   failed patches (xxx# or xxx.rej). If there are, either you or me has
   made a mistake.

   Unlike patches for the 2.6.x kernels, patches for the 2.6.x.y kernels
   (also known as the -stable kernels) are not incremental but instead apply
   directly to the base 2.6.x kernel.  Please read
   Documentation/applying-patches.txt for more information.

   Alternatively, the script patch-kernel can be used to automate this
   process.  It determines the current kernel version and applies any
   patches found.

		linux/scripts/patch-kernel linux

   The first argument in the command above is the location of the
   kernel source.  Patches are applied from the current directory, but
   an alternative directory can be specified as the second argument.

 - If you are upgrading between releases using the stable series patches
   (for example, patch-2.6.xx.y), note that these "dot-releases" are
   not incremental and must be applied to the 2.6.xx base tree. For
   example, if your base kernel is 2.6.12 and you want to apply the
   2.6.12.3 patch, you do not and indeed must not first apply the
   2.6.12.1 and 2.6.12.2 patches. Similarly, if you are running kernel
   version 2.6.12.2 and want to jump to 2.6.12.3, you must first
   reverse the 2.6.12.2 patch (that is, patch -R) _before_ applying
   the 2.6.12.3 patch.
   You can read more on this in Documentation/applying-patches.txt

 - Make sure you have no stale .o files and dependencies lying around:

		cd linux
		make mrproper

   You should now have the sources correctly installed.

## SOFTWARE REQUIREMENTS

   Compiling and running the 2.6.xx kernels requires up-to-date
   versions of various software packages.  Consult
   Documentation/Changes for the minimum version numbers required
   and how to get updates for these packages.  Beware that using
   excessively old versions of these packages can cause indirect
   errors that are very difficult to track down, so don't assume that
   you can just update packages when obvious problems arise during
   build or operation.

## BUILD directory for the kernel:

   When compiling the kernel all output files will per default be
   stored together with the kernel source code.
   Using the option "make O=output/dir" allow you to specify an alternate
   place for the output files (including .config).
   Example:
     kernel source code:	/usr/src/linux-2.6.N
     build directory:		/home/name/build/kernel

   To configure and build the kernel use:
   cd /usr/src/linux-2.6.N
   make O=/home/name/build/kernel menuconfig
   make O=/home/name/build/kernel
   sudo make O=/home/name/build/kernel modules_install install

   Please note: If the 'O=output/dir' option is used then it must be
   used for all invocations of make.

## CONFIGURING the kernel:

   Do not skip this step even if you are only upgrading one minor
   version.  New configuration options are added in each release, and
   odd problems will turn up if the configuration files are not set up
   as expected.  If you want to carry your existing configuration to a
   new version with minimal work, use "make oldconfig", which will
   only ask you for the answers to new questions.

 - Alternate configuration commands are:
	"make config"      Plain text interface.
	"make menuconfig"  Text based color menus, radiolists & dialogs.
	"make xconfig"     X windows (Qt) based configuration tool.
	"make gconfig"     X windows (Gtk) based configuration tool.
	"make oldconfig"   Default all questions based on the contents of
			   your existing ./.config file and asking about
			   new config symbols.
	"make silentoldconfig"
			   Like above, but avoids cluttering the screen
			   with questions already answered.
			   Additionally updates the dependencies.
	"make defconfig"   Create a ./.config file by using the default
			   symbol values from either arch/$ARCH/defconfig
			   or arch/$ARCH/configs/${PLATFORM}_defconfig,
			   depending on the architecture.
	"make ${PLATFORM}_defconfig"
			  Create a ./.config file by using the default
			  symbol values from
			  arch/$ARCH/configs/${PLATFORM}_defconfig.
			  Use "make help" to get a list of all available
			  platforms of your architecture.
	"make allyesconfig"
			   Create a ./.config file by setting symbol
			   values to 'y' as much as possible.
	"make allmodconfig"
			   Create a ./.config file by setting symbol
			   values to 'm' as much as possible.
	"make allnoconfig" Create a ./.config file by setting symbol
			   values to 'n' as much as possible.
	"make randconfig"  Create a ./.config file by setting symbol
			   values to random values.

   You can find more information on using the Linux kernel config tools
   in Documentation/kbuild/kconfig.txt.

	NOTES on "make config":
	- having unnecessary drivers will make the kernel bigger, and can
	  under some circumstances lead to problems: probing for a
	  nonexistent controller card may confuse your other controllers
	- compiling the kernel with "Processor type" set higher than 386
	  will result in a kernel that does NOT work on a 386.  The
	  kernel will detect this on bootup, and give up.
	- A kernel with math-emulation compiled in will still use the
	  coprocessor if one is present: the math emulation will just
	  never get used in that case.  The kernel will be slightly larger,
	  but will work on different machines regardless of whether they
	  have a math coprocessor or not. 
	- the "kernel hacking" configuration details usually result in a
	  bigger or slower kernel (or both), and can even make the kernel
	  less stable by configuring some routines to actively try to
	  break bad code to find kernel problems (kmalloc()).  Thus you
	  should probably answer 'n' to the questions for
          "development", "experimental", or "debugging" features.

## COMPILING the kernel:

 - Make sure you have at least gcc 3.2 available.
   For more information, refer to Documentation/Changes.

   Please note that you can still run a.out user programs with this kernel.

 - Do a "make" to create a compressed kernel image. It is also
   possible to do "make install" if you have lilo installed to suit the
   kernel makefiles, but you may want to check your particular lilo setup first.

   To do the actual install you have to be root, but none of the normal
   build should require that. Don't take the name of root in vain.

 - If you configured any of the parts of the kernel as `modules', you
   will also have to do "make modules_install".

 - Verbose kernel compile/build output:

   Normally the kernel build system runs in a fairly quiet mode (but not
   totally silent).  However, sometimes you or other kernel developers need
   to see compile, link, or other commands exactly as they are executed.
   For this, use "verbose" build mode.  This is done by inserting
   "V=1" in the "make" command.  E.g.:

	make V=1 all

   To have the build system also tell the reason for the rebuild of each
   target, use "V=2".  The default is "V=0".

 - Keep a backup kernel handy in case something goes wrong.  This is 
   especially true for the development releases, since each new release
   contains new code which has not been debugged.  Make sure you keep a
   backup of the modules corresponding to that kernel, as well.  If you
   are installing a new kernel with the same version number as your
   working kernel, make a backup of your modules directory before you
   do a "make modules_install".
   Alternatively, before compiling, use the kernel config option
   "LOCALVERSION" to append a unique suffix to the regular kernel version.
   LOCALVERSION can be set in the "General Setup" menu.

 - In order to boot your new kernel, you'll need to copy the kernel
   image (e.g. .../linux/arch/i386/boot/bzImage after compilation)
   to the place where your regular bootable kernel is found. 

 - Booting a kernel directly from a floppy without the assistance of a
   bootloader such as LILO, is no longer supported.

   If you boot Linux from the hard drive, chances are you use LILO which
   uses the kernel image as specified in the file /etc/lilo.conf.  The
   kernel image file is usually /vmlinuz, /boot/vmlinuz, /bzImage or
   /boot/bzImage.  To use the new kernel, save a copy of the old image
   and copy the new image over the old one.  Then, you MUST RERUN LILO
   to update the loading map!! If you don't, you won't be able to boot
   the new kernel image.

   Reinstalling LILO is usually a matter of running /sbin/lilo. 
   You may wish to edit /etc/lilo.conf to specify an entry for your
   old kernel image (say, /vmlinux.old) in case the new one does not
   work.  See the LILO docs for more information. 

   After reinstalling LILO, you should be all set.  Shutdown the system,
   reboot, and enjoy!

   If you ever need to change the default root device, video mode,
   ramdisk size, etc.  in the kernel image, use the 'rdev' program (or
   alternatively the LILO boot options when appropriate).  No need to
   recompile the kernel to change these parameters. 

 - Reboot with the new kernel and enjoy. 

## IF SOMETHING GOES WRONG:

 - If you have problems that seem to be due to kernel bugs, please check
   the file MAINTAINERS to see if there is a particular person associated
   with the part of the kernel that you are having trouble with. If there
   isn't anyone listed there, then the second best thing is to mail
   them to me (torvalds@linux-foundation.org), and possibly to any other
   relevant mailing-list or to the newsgroup.

 - In all bug-reports, *please* tell what kernel you are talking about,
   how to duplicate the problem, and what your setup is (use your common
   sense).  If the problem is new, tell me so, and if the problem is
   old, please try to tell me when you first noticed it.

 - If the bug results in a message like

	unable to handle kernel paging request at address C0000010
	Oops: 0002
	EIP:   0010:XXXXXXXX
	eax: xxxxxxxx   ebx: xxxxxxxx   ecx: xxxxxxxx   edx: xxxxxxxx
	esi: xxxxxxxx   edi: xxxxxxxx   ebp: xxxxxxxx
	ds: xxxx  es: xxxx  fs: xxxx  gs: xxxx
	Pid: xx, process nr: xx
	xx xx xx xx xx xx xx xx xx xx

   or similar kernel debugging information on your screen or in your
   system log, please duplicate it *exactly*.  The dump may look
   incomprehensible to you, but it does contain information that may
   help debugging the problem.  The text above the dump is also
   important: it tells something about why the kernel dumped code (in
   the above example it's due to a bad kernel pointer). More information
   on making sense of the dump is in Documentation/oops-tracing.txt

 - If you compiled the kernel with CONFIG_KALLSYMS you can send the dump
   as is, otherwise you will have to use the "ksymoops" program to make
   sense of the dump (but compiling with CONFIG_KALLSYMS is usually preferred).
   This utility can be downloaded from
   ftp://ftp.<country>.kernel.org/pub/linux/utils/kernel/ksymoops/ .
   Alternately you can do the dump lookup by hand:

 - In debugging dumps like the above, it helps enormously if you can
   look up what the EIP value means.  The hex value as such doesn't help
   me or anybody else very much: it will depend on your particular
   kernel setup.  What you should do is take the hex value from the EIP
   line (ignore the "0010:"), and look it up in the kernel namelist to
   see which kernel function contains the offending address.

   To find out the kernel function name, you'll need to find the system
   binary associated with the kernel that exhibited the symptom.  This is
   the file 'linux/vmlinux'.  To extract the namelist and match it against
   the EIP from the kernel crash, do:

		nm vmlinux | sort | less

   This will give you a list of kernel addresses sorted in ascending
   order, from which it is simple to find the function that contains the
   offending address.  Note that the address given by the kernel
   debugging messages will not necessarily match exactly with the
   function addresses (in fact, that is very unlikely), so you can't
   just 'grep' the list: the list will, however, give you the starting
   point of each kernel function, so by looking for the function that
   has a starting address lower than the one you are searching for but
   is followed by a function with a higher address you will find the one
   you want.  In fact, it may be a good idea to include a bit of
   "context" in your problem report, giving a few lines around the
   interesting one. 

   If you for some reason cannot do the above (you have a pre-compiled
   kernel image or similar), telling me as much about your setup as
   possible will help.  Please read the REPORTING-BUGS document for details.

 - Alternately, you can use gdb on a running kernel. (read-only; i.e. you
   cannot change values or set break points.) To do this, first compile the
   kernel with -g; edit arch/i386/Makefile appropriately, then do a "make
   clean". You'll also need to enable CONFIG_PROC_FS (via "make config").

   After you've rebooted with the new kernel, do "gdb vmlinux /proc/kcore".
   You can now use all the usual gdb commands. The command to look up the
   point where your system crashed is "l *0xXXXXXXXX". (Replace the XXXes
   with the EIP value.)

   gdb'ing a non-running kernel currently fails because gdb (wrongly)
   disregards the starting offset for which the kernel is compiled.

