---
title: Get process info from proc
authors: Shawn Hoffman
editors: 
summary: "How to get information about processes from /proc"
tags: [Linux Administration, Linux Processes]
keywords: Linux Administration, Linux Processes
references: https://man7.org/linux/man-pages/man5/proc.5.html, https://en.wikipedia.org/wiki/Procfs, 
permalink: /linux-admin-get-process-info-from-proc

folder: docker
sidebar: docker_sidebar
---

The **proc file system (procfs)** is a virtual file system created dynamically when the system boots, and is dissolved at system shut down.

`/proc` contains useful information about the processes that are currently running, it is regarded as the information and control center for the Linux kernel.

The proc file system also provides a communication medium between the kernel space and the user space. You can issue `cat /proc/{file}` on the following files to output information.

<br>

| Directory | Description |
|-----------|-------------|
| `/proc/cpuinfo` | Information about the processor, such as its type, make, model, and performance. |
| `/proc/crypto` | List available cryptographic modules on the host. |
| `/proc/devices` | List of device drivers configured into the currently running kernel. |
| `/proc/diskstats` | Information about each of the logical disk devices, including device numbers. |
| `/proc/dma` | Shows which DMA channels are being used at the moment. |
| `/proc/filesystems` | List of the file systems supported by the kernel at the time of listing. |
| `/proc/kmsg` | Holds messages output by the kernel. |
| `/proc/meminfo` | Summary of how the kernel is managing its memory. |
| `/proc/tty` | Information about the current terminals. |
| `/proc/version` | Contains the Linux kernel version, distribution number, gcc version number (this is used to build the kernel) and any other pertinent information relating to the version of the kernel currently running. |
| `/proc/[pid]/cmdline` | Holds the complete entrypoint command and arguments for the process, unless the whole process has been swapped out, or unless the process is a zombie. In either of these later cases, there is nothing in this file and a read on this file will return 0 characters. The command line arguments appear in this file as a set of null-separated strings, with a further null byte after the last string. |
| `/proc/[pid]/cwd` | This is a link to the current working directory of the process. |
| `/proc/[pid]/environ` | This file contains the environment for the process. The entries are separated by null characters, and there may be a null character at the end. |
| `/proc/[pid]/exe` | The exe file is a symbolic link containing the actual path name of the executed command. The exe symbolic link can be dereferenced normally – attempting to open exe will open the executable. You can even type `/proc/[pid]/exe` to run another copy of the same process as `[pid]`. |
| `/proc/[pid]/fd` | This is a subdirectory containing one entry for each file which the process has opened, named by its file descriptor, and which is a symbolic link to the actual file (as the exe entry does). Thus, 0 is standard input, 1 standard output, 2 standard error, etc. |
| `/proc/[pid]/maps` | A file containing the currently mapped memory regions and their access permissions. |
| `/proc/[pid]/mem` | The mem file provides a means to access the process memory pages, using `open`, `fseek` and `read` commands. |
| `/proc/[pid]/root` | This is a link to the root directory which is seen by the process. This root directory is usually “/”, but it can be changed by the chroot command. |
| `/proc/[pid]/stat` | This file provides status information about the process. This is used by the Process Show utility. It is defined in fs/proc/array.c source file and may differ from one distribution to another. |
