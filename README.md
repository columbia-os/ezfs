
EZFS
======

Submission
----------

As with previous assignments, we will be using GitHub to distribute skeleton
code and collect submissions. Please refer to our [Git Workflow][git-workflow]
guide for more details. Note that we will be using multiple tags for this
assignment, for each deliverable part.

For students on arm64 computers (e.g. M1/M2/M3 machines): if you want your
submission to be built/tested for ARM, you must create and submit a file called
`.armpls` in the top-level directory of your repo; feel free to use the
following one-liner:

```sh
cd "$(git rev-parse --show-toplevel)" && touch .armpls && git add -f .armpls && git commit .armpls -m "ARM pls"
```

**You should do this first so that this file is present in all parts.**

[git-workflow]: https://columbia-os.github.io/dev-guides/git-workflow.html

Mandatory Survey
----------------
Since this is the last assignment of the semester, **EACH** group member should
indicate in the README two important pieces of information:
- (1) the number of hours spent on this assignment, and
- (2) a rank ordering of the difficulty of the homework assignments.

Here's a recap of this semester's homework
- HW # 1 Linux-List
- HW # 3 Multi-Server
- HW # 4 Tabletop
- HW # 5 Fridge
- HW # 6 Freezer
- HW # 7 Farfetch
- HW # 8 EZFS

For example,
```
abc123: 15hrs, Linux-List < Multi-Server < EZFS < Fridge < Farfetch < Freezer < Tabletop
```
would indicate that student with UNI abc123 spent 15 hrs on this assignment and
that Linux-List was the easiest and Tabletop was the hardest.
The README should be placed in the top level directory of your team repo.
5 points for this assignment will be allocated to grading your README.

Code Style
----------

There is a script in the skeleton code named `run_checkpatch.sh`. It is a
wrapper over `linux/scripts/checkpatch.pl`, which is a Perl script that
comes with the linux kernel that checks if your code conforms to the
kernel coding style.

Execute `run_checkpatch.sh` to see if your code conforms to the kernel
style – it'll let you know what changes you should make. You must make
these changes before pushing a tag. Passing `run_checkpatch.sh` with no
warnings and no errors **is required** for this assignment.

Overview
--------

In this assignment, you will write your own disk-based file system, EZFS.
You will learn how to use a loop device to turn a regular file into a block
storage device, then format that device into an EZFS file system.
Then you will use EZFS to access the file system.
EZFS will be built as a kernel module that you can load into the stock
**Debian 11.6** kernel in your VM.
You do NOT need to use the 4118 kernel you built for previous homework
assignments and there is no need to build the entire Linux kernel tree
for this assignment.

Part 1: Formatting and mounting disks
-------------------------------------

A loop device is a pseudo-device that makes a file accessible as a block device. Files of this kind are often used for CD ISO images. Mounting a file containing a file system via such a loop mount makes the files within that file system accessible. You will do this with EZFS, but to first gain some experience with a loop device, the following gives you a sample session for creating a loop device and building and mounting an ext2 file system on it. This session starts from the home directory of a user zzj. You should read man pages and search the Internet so you can understand what is going on at each step.

```console
$ sudo su
# dd if=/dev/zero of=./ext2.img bs=1024 count=100
100+0 records in
100+0 records out
102400 bytes (102 kB, 100 KiB) copied, 0.000600037 s, 171 MB/s
# modprobe loop
# losetup --find --show ext2.img
/dev/loop0
# mkfs -t ext2 /dev/loop0
mke2fs 1.44.5 (15-Dec-2018)
Creating filesystem with 100 1k blocks and 16 inodes

Allocating group tables: done
Writing inode tables: done
Writing superblocks and filesystem accounting information: done

# mkdir mnt
# mount /dev/loop0 ./mnt
# df -hT
Filesystem     Type      Size  Used Avail Use% Mounted on
...
/dev/loop0     ext2       93K   14K   74K  16% /root/mnt
# cd mnt
# ls -al
total 17
drwxr-xr-x  3 root root  1024 Apr 21 02:22 .
drwxr-xr-x 37 zzj zzj  4096 Apr 21 02:22 ..
drwx------  2 root root 12288 Apr 21 02:22 lost+found
# mkdir sub2
# ls -al
total 18
drwxr-xr-x  4 root root  1024 Apr 21 02:23 .
drwxr-xr-x 37 zzj zzj  4096 Apr 21 02:22 ..
drwx------  2 root root 12288 Apr 21 02:22 lost+found
drwxr-xr-x  2 root root  1024 Apr 21 02:23 sub2
# cd sub2
# ls -al
total 2
drwxr-xr-x 2 root root 1024 Apr 21 02:23 .
drwxr-xr-x 4 root root 1024 Apr 21 02:23 ..
# mkdir sub2.1
# ls -al
total 3
drwxr-xr-x 3 root root 1024 Apr 21 02:24 .
drwxr-xr-x 4 root root 1024 Apr 21 02:23 ..
drwxr-xr-x 2 root root 1024 Apr 21 02:24 sub2.1
# touch file2.1
# ls -al
total 3
drwxr-xr-x 3 root root 1024 Apr 21 02:24 .
drwxr-xr-x 4 root root 1024 Apr 21 02:23 ..
-rw-r--r-- 1 root root    0 Apr 21 02:24 file2.1
drwxr-xr-x 2 root root 1024 Apr 21 02:24 sub2.1
# cd ../../
# umount mnt/
# losetup --find
/dev/loop1
# losetup --detach /dev/loop0
# losetup --find
/dev/loop0
# ls -al mnt/
total 8
drwxr-xr-x  2 root root 4096 Apr 21 02:22 .
drwxr-xr-x 37 zzj zzj 4096 Apr 21 02:22 ..
```

In the sample session shown above, files and directories are created. Make sure you see the number of links each file or directory has, and make sure you understand why.

Also try creating some hard links and symlinks.  Make sure you understand how they affect the link counts.

Part 2: Exploring EZFS
-------------------------------------
TODO:

Part 3: Changing the formatting program
-------------------------------------
TODO:

Part 4: Initializing and mounting the file system
-------------------------------------
TODO:

Part 5: Listing the contents of the root directory
-------------------------------------
TODO:

Part 6: Accessing subdirectories
-------------------------------------
TODO:

Part 7: Reading the contents of regular files
-------------------------------------
TODO:

Part 8: Writing to existing files
-------------------------------------
TODO:

Part 9: Creating new files
-------------------------------------
TODO:

Part 10: Deleting files
-------------------------------------
TODO:

Part 11: Making and removing directories
-------------------------------------
TODO:

Part 12: Compile and run executable files
-------------------------------------
TODO:

Part 13: Move files
-------------------------------------
You might notice that mv works when moving files from another file system to yours, but mv does not work to move files within your file system. Fix this by adding support for `renameat` and `renameat2`, specifically the `RENAME_NOREPLACE` flag.
See `man rename` for details.

Submission
-------------------------------------
At this point, you should make sure that whatever robustness tests you did earlier continue to pass with your completed file system, and your tests should include having multiple processes or threads perform various file system operations concurrently. In addition, you should try running various programs manipulating the files in your file system.  

In your README, note which applications you have used, which ones worked, and which ones do not. What are some file operations supported on your default Linux file system that are not supported by EZFS? Which of these affect the functionality of the programs you ran?

To submit this part, push the hw8handin tag with the following:
```console
$ git tag -a -m "Completed hw8." hw8handin
$ git push origin main
$ git push origin hw8handin
```

--------
### *Acknowledgment*

The EZFS assignment was previously used by Prof. Jason Nieh in his COMS W4118 Operating Systems I instruction at Columbia University.
It was rewritten by the following TAs in Spring 2024 to be incorporated into the teaching of Prof. Kostis Kaffes:

-   Abhinav Gupta
-   Alex Jiakai Xu

--------
*Last updated: 2024-04-16*
