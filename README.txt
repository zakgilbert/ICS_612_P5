 ICS 612 Project 5
 ==================
 [Project Guidelines](http://www2.hawaii.edu/~esb/2021spring.ics612/project5.html)

 I have two ideas...

-----------------------------------------
## 1. Porting NetBSD Userland to MINIX 3 
Searching for a project I stumbled upon the `MINIX3` Google summer of code site. It seems that `MINIX` has not participated since 2018, but I found that the documentation for the years past contained many clues as to what projects can be done. [Google Summer of Code 2013](https://wiki.minix3.org/doku.php?id=soc:2013:projects) stuck out the most, particularly the section on porting standard network utilities from `netBSD`. After searching I found a full page on how one might [port from netBSD](https://wiki.minix3.org/doku.php?id=developersguide:portingnetbsduserland).

The documentation has a list of all the programs that have already been ported. I think the hardest part about this task would be finding a worthwhile program that hasn't already been ported. I was able to clone the `netBSD` source code to my `MINIX3` virtual machine. I ran a plethora of shell commands in order to rule out all the program which have already been ported. It seems `libc` has already been completely ported. I looked at `/usr/src/usr.sbin` as well. I hope I did this correctly.

    # Get the two files we want to compare.
        $ ls /usr/src/minix/usr.sbin >> minixls
        $ ls /usr/netbsd/usr.sbin >> netls
    # Concat the two files.
        $ cat minixls netls >> combined
    # Sort and list by uniq...
        $ sort combined | uniq -c | sort -n >> sorted
    # Remove those tabs that uniq produced.
        $ sed -i -e 's/[ \t]*//' sorted
    # Remove all lines with a frequency greater than 1.
        $ grep -v '^[2-9]' sorted >> final
        $ cat final | wc -l
        140
Assuming that my shell script is correct usr.sbin contains a hundred and forty portable programs. I thought [quot(8)](https://man.netbsd.org/NetBSD-5.0.1/quot.8) could be a suitable candidate to port.

--------------------
## 2. Porting a Game
Over the past few years I've written numerous simple C games using the [SDL2](https://www.libsdl.org/) graphics library. `MINIX3` contains a few games including _Tetris_ and my favorite _Fortune_. I've glanced at the source code and it seems to use `term.h` and `termios.h` to render graphics. I understand that this will be a lot harder than using a graphics library but I feel like this is a project I would truly enjoy.





  