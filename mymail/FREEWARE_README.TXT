MYMAIL 1.0
FREEWARE_README 
MYMAIL,Utility, A program to use vms mail syntax in a communigate-pro environment

This utility can send mail (files) from batch and dcl procedures using
a vmsmail syntax via the communigate pro mail system. This makes it 
easy to replace VMS-mail by communicate pro without changing all 
existing DCL procedures that use VMS mail as mail product. 

The following syntax is supported:

MAIL file to[,to[,to...] [/qualifiers]

 p1 : The file to send (default = .TXT)
 p2 : The user(s) to send to 
      Distribution lists are supported via the "@file" (default .DIS)
        upto a depth of 10 levels.
        in .dis files the  !-sign is taken as comment delimiter
        and empty lines are ignored.

 Qualifiers
 /signature_file          Get the signature file from VMSMAIL_PROFILE
 /signature_file=filename Use this file as a signature file 
                             (default extension .TXT)
 /subject="Subject text") Subject information
 /self                    Include the (vms-)username in the TO list
 /personal_name           Get the personal_name from VMSMAIL_PROFILE
 /personal_name="Name"    Use this as personal name

 Extra qualifiers for mymail (debugging)

 /log                      Return the resulting mailfile
 /verbose                  Tell a lot of info about the phases

 e.g.
 mail x john,@list/subject="test"/self/signature
  sends x.txt to john,'vmsuser' and everyone in list.dis
  the subject is "test"
  and append the signature file from VMSMAIL_PROFILE to the
  text in x.txt


Usage:

Define a logical (mymail_location) pointing to the target directory
for mymail. You can define it to sys$scratch for testing
Normally it will contain something like 
 SYS$SPECIFIC:[COMMUNIGATE.SUBMITTED]

$define mymail_location sys$specific:[communigate.submitted][/system]

Define a foreign command (can be done in SYS$SYLOGIN)

$mai*l:=$device:[directory]mymail

From now on the MAIL command will be processed by the mymail image
This image needs BYPASS or READALL to open VMSMAIL_PROFILE, so it
should be installed with /PRIV=BYPASS or /PRIV=READALL.

Building
The Alpha executable is in the kit, but if you want to rebuild 
MYMAIL, go to the MYMAIL directory and use @MAKE

Contents:

Ths package contains the following files
In the home directory

 MYMAIL.EXE             The executable
 MAKE.COM	        The command file to compile/link
 FREEWARE_README.TXT    This file
 RELEASE_NOTES.TXT      The release notes

 MYMAIL.FOR		The Fortran source
 MYMAIL.INC		The include file
 MAYMAIL_CLD.CLD        The CLD file

 MYMAIL.OBJ		The (Alpha) object of the fortran source
 MYMAIL_CLD.OBJ         The (Alpha) object of the .cld file

There is no need for a VAX equivalent, since Communigate Pro only 
runs on Alpha and IA64.

Instructions:

Unpack the save set.

If you want to rebuild:
 If you have a fortran compiler
  @MAKE
 Else link only
  @MAKE LINK

On oooovms.dyndns.org is a link to the most recent version.

Author : Fekko Stubbe
Email  : mymaildev@oooovms.dyndns.org
