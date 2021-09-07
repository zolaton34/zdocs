# [Introduction to Linux](https://learning.edx.org/course/course-v1:LinuxFoundationX+LFS101x+2T2021/home)
* [Distribution List](https://lwn.net/Distributions)
## Linux Terminology
Before you begin using Linux, you need to be aware of some basic terms such as kernel, distribution, boot loader, service, filesystem, X Window system, desktop environment, and command line. These are very commonly used by the Linux community.
* kernel
  * brain of Linux OS
  * controls hardware
  * makes hardware interact with the applications
* distribution
  * collection of programs + Kernel = Linux based OS
* boot loader
  * program to boot the OS
  * examples: GRUB and ISOLINUX
* service
  * program running as a background process
  * examples: httpd, nfsd, ntpd, ftpd and named
* filesystem
  * method for storing and organizing files
  * examples: ext3, ext4, FAT, XFS and Btrfs
* X Window System
  * standard toolkit and protocol to build graphical user interfaces
* desktop environment is a graphical user interface on top of the operating system
  * examples: GNOME, KDE, Xfce and Fluxbox
* command line
  * interface for typing commands on top of the operating system
  * shell
    * command line interpreter
    * interprets the command line input
    * instructs the operating system to perform any necessary tasks and commands
    * examples: bash, tcsh and zsh.

## The Boot Process
1. Power on
2. BIOS.
3. Master Boot Record (MBR) = First sector of the hard disk
4. Boot Loader
5. Kernel
6. Initial RAM disk - initramfs image
7. `/sbin/init` (parent process)
8. Command shell using getty
9. X Window System (GUI)

### BIOS
* initializes the hardware
  * including screen and keyboard
  * tests the main memory
* called POST (Power On Self Test)
* BIOS software is stored on a ROM chip on the motherboard
* remainder of the boot process controlled by OS

### Master Boot Record (MBR) and Boot Loader
* after POST completed system control passes from the BIOS to the boot loader
* boot loader stored on hard disk
  * on boot sector, or
  * on EFI partition (Extensible Firmware Interface)
* next
  * information on date, time, and the most important peripherals are loaded from the CMOS values

#### Boot Loader in Action
The boot loader has two stages.
* first stage
  * With BIOS/MBR method
    * boot loader on first sector of the hard disk (MBR)
    * MBR size = 512 bytes. 
    * the boot loader 
      * examines the partition table
      * finds a bootable partition
    * searches for the second stage boot loader, for example GRUB
    * loads it into RAM
  * With EFI/UEFI
    * UEFI firmware reads its Boot Manager data to determine which UEFI application is to be launched and from where (i.e. from which disk and partition the EFI partition can be found)
    * UEFI application (ie. GRUB) is launched
* second stage
  * under `/boot`
  * slash screen is displayed to choose OS to launch
  * kernel of chosen OS is loaded to RAM
  * control passes to the kernel
    * kernel uncompress itself (if compressed)
    * initializes and configures computer’s memory
    * check and analyze hardware
    * configures the hardware including
      * processors
      * I/O subsystems
      * storage devices, etc. 
    * loads some necessary user space applications

### Initial RAM Disk
* __initramfs__ filesystem image contains programs and binary needed to
  * perform all actions needed to mount the root filesystem, like 
    * providing kernel functionality for
      * the needed filesystem
      * device drivers for mass storage controllers with __udev__
        * __udev__ 
          * find present devices
          * locate and load the device drivers they need
    * find root filesystem 
    * check root filesystem 
    * mount root filesystem
    * if success
      * __initramfs__ cleared from RAM
      * `/sbin/init` is executed
          * mounts real final root system
          * pivots to it
          * login

### `/sbin/init` and Services
* __init__
  * initial process
  * start other processes
  * most other processes trace to __init__, except kernel processes
  * keeps sytem running
  * responsible for clean shutdown
  * manager for all non-kernel processes
    * cleans up after their completion
    * restarts user login services, and other services as needed when users log in and out
    * __systemd__: method to manage services

### systemd Features
* faster than previous methods
* use configuration instead of scripts
  * defines
    * what has to be done before a service is started
    * how to execute service startup
    * what conditions indicate service startup is finished
Note: `/sbin/init` links to `/lib/systemd/systemd`; i.e. __systemd__ takes over the init process.
* `systemctl`: __systemd__ used for most basic tasks.
```bash
# systemctl examples
$ sudo systemctl start|stop|restart httpd
$ sudo systemctl enable|disable httpd
```

### Filesystems & Partition
* __partition:__ physically (apparently) contiguous section of a disk
* __filesystem:__ method of storing/finding files on a hard disk (usually in a partition)
  * Linux filesystems
    * Conventional disk filesystems: ext3, ext4, XFS, Btrfs, JFS, NTFS, vfat, exfat, etc.
    * Flash storage filesystems: ubifs, jffs2, yaffs, etc.
    * Database filesystems
    * Special purpose filesystems: procfs, sysfs, tmpfs, squashfs, debugfs, fuse, etc.

#### The Filesystem Hierarchy Standard
* `/bin/` essential user command binaries
* `/boot/` static files of the bootloader
* `/dev/` device files
* `/etc/` host-specific system configution
* `/home/` user home directories
* `/lib/` essential shared libraries and kernel modules
* `/media/` mount point for removable media (on recent system: `/run/media/yourusername/disklabel`)
* `/mnt/` mount point for a temporary filesystem
* `/opt/` add-on application software packages
* `/sbin/` system binaries
* `/srv/` data for services provided by this system
* `/tmp/` temporary files
* `/usr/` multi-user utilities and applications
  * `/usr/bin/`
  * `/usr/include/`
  * `/usr/lib/`
  * `/usr/local/`
  * `/usr/sbin/`
  * `/usr/share/`
* `/var/` variable files
* `/root/` home directory of the root user
* `/proc/` virtual file system documenting kernel and process status as text files

## Graphical User Interface
* Desktop environment (display manager)
  * session manager: starts and maintains the components of the graphical session
  * window manager: controls the placement and movement of windows, window title-bars, and controls
* Examples of desktop environment
  * __gdm__ GNOME
  * __kdm__ KDE
  * etc.
### Gnome
* __gnome-tweaks__ more setting options
  * changing the theme
  * font size
  * etc.

### System Configuration from the Graphical Interface
#### Installing and Updating Software (Debian)
* __dpkg__ underlying package manager
  * Advanced Package Tool (APT) = higher-level package management system
    * Interfaces to APT. Examples
      * apt
      * apt-get
      * synaptic
      * gnome-software
      * Ubuntu Software Center
* Most repositories target a particular distribution
* Often software distributors ship with multiple repositories to support multiple distributions

#### Installing and Updating Software (SUSE/openSUSE, Mageia, CentOS, Oracle Linux)
* __rpm__ underlying package manager
  * higher-level package management system
    * yum
    * dfn
    * zypper (SUSE)

## Processes
### Listing Processes: ps and top
#### Interactive Keys with top
* __t__  Display or hide summary information (rows 2 and 3)
* __m__  Display or hide memory information (rows 4 and 5)
* __A__  Sort the process list by top resource consumers
* __r__  Renice (change the priority of) a specific processes
* __k__  Kill a specific process
* __f__  Enter the top configuration screen
* __o__  Interactively select a new sort order in the process list
* __h__  Help on interactive keys

## User Environment
### Order of the Startup Files
 `/etc/profile` is executed first then,
1. `~/.bash_profile`
2. `~/.bash_login`
3. `~/.profile`

* Executes on login
* Only the first script in the numbered list that is found is run
* Then for all every new shell or terminal without login, `.bashrc` is run

Usually user customize `.bashrc` since it is always run