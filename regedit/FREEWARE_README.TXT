REGEDIT,Utility, A program to look at or modify the registry database

Regedit is a program to look at or modify the registryserver database.
It is a full screen (SMG) program and looks like the (MS-)Windows regedit.
If you use the DecWindows environment, you can use mouse-clicks, 
otherwise you must use keyboard-keys.
Because the registry-server exists only on alpha and IA64, 
there is no VAX version.

Usage:

Run regedit

Once in the program you can use PF2/HELP for help info
F10 or ^z will return you to DCL. If you have a Decwindows/VXT display, you
can use the mouse, otherwise you must use the keyboard.

Building
The Alpha and IA64 executables are in the kit, but if you want to rebuild 
REGEDIT, go to the regedit directory and use @create_regedit


Contents:

Ths package contains the following files
In the home directory

 REGEDIT_ALPHA.EXE	The executable
 REGEDIT_IA64.EXE	The executable
 CREATE_REGEDIT.COM	The command file to compile/link
 FREEWARE_README.TXT    This file

In the [.SRC] directory
 REGEDIT.FOR		The Fortran source
 REGEDIT.INC		The include file

In the [.ALPHA] directory
 REGEDIT.OBJ		The Alpha object

In the [.IA64] directory
 REGEDIT.OBJ		The IA64 object

There is no VAX equivalent, since the registry server only 
runs on Alpha and IA64.

Instructions:

Unpack the save set.
If you want to rebuild:
 If you have a fortran compiler
  @CREATE_REGEDIT
 Else to link only
  @CREATE_REGEDIT LINK

On oooovms.dyndns.org is a link to the most recent version.

Author : Fekko Stubbe
Email  : regeditdev@oooovms.dyndns.org
