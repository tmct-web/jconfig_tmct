mt_jconfig: jrunner.o jb_io.o jb_jtag.o
	gcc -Wall -O2 -o mt_jconfig jrunner.o jb_io.o jb_jtag.o

jrunner.o:
	gcc -c jrunner.c
jb_io.o:
	gcc -c jb_io.c
jb_jtag.o:
	gcc -c jb_jtag.c
clean:
	rm -f mt_jconfig jrunner.o jb_io.o jb_jtag.o
