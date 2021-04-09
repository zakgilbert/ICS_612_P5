Porting NetBSD Userland to `MINIX3` 
 ==================
 The following work flow outlines the basic tasks need to successfully [port from netBSD](https://wiki.minix3.org/doku.php?id=developersguide:portingnetbsduserland).
 
1. Clone both the NetBSD and MINIX3 source code to the MINIX system.
2. Choose a NetBSD utility to port.
    - I'm finding This part to be challenging, surely most of the exciting utilities have already been ported. I was looking at porting a `libc` system call. Minix provides a list of [un-ported system calls](https://git.minix3.org/index.cgi?p=minix.git;a=blob;f=minix/lib/libc/sys/MISSING_SYSCALLS;hb=HEAD) and yet the porting guide only specifies how to port NetBSD utilities to replace the executables in `minix/commands`. Since porting a system call suits this class I'm gonna go with `mremap(2)`.
    - System call [`mremap(2)`](https://man.netbsd.org/mremap.2).
        
          #include <sys/mman.h>
          void *mremap(void *oldp, size_t oldsize, void *newp, size_t newsize,int flags);

           The mremap() function resizes the mapped range (see mmap(2)) starting at oldp and having size oldsize to newsize.  The following arguments can be OR'ed together in the flags argument:
3. Copy the files from NetBSD to MINIX3.
   - If the utiliy exists in `netBSD/usr.sbin` then it needs to be moved to `minix/usr.sbin`.
   - In the case of `mremap(2)` there are multiple files in different locations so multiple files need to be moved to their corresponding locations.
4. Add the subdirectory to the `Makefile`. Directory `usr/bin` contains its own makefile which in turn calls the makefiles in the subdirectories of that directory so the filename of the subdirectory needs to be added to the makefile.
5. Add the subtree to `releasetools/nbsd_ports` (this file does not exist in my current Minix version) with the date that you can tell from  the netBSD commit log.
6. Get the code working in MINIX, should be able to compile and run the command from the local directory. This involves editing the source code.
7. If porting an existing command, make sure its makefile doesn't implement any other commands.
8. Remove the old man pages.
9. Make sure there are no old copies of the command in the source code. Do this by executing a `make clean build` as well as a `cd releaseTools && release.sh -c`. Then run the created ISO to see if the build works.
10. Add binaries and man pages to minix and mark the old ones as _obsolete_. 
11. Submit the commit for merging with master (this would look pretty good on an interview.)