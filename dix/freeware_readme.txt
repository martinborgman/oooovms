DIX,Utility,A program to read/modify records in any RMS (seq/relative/idx) file

This program lets you view/modify records in any RMS file.
It also has a powerful scripting facility(DCL like) including a debugger.
DIX can also be used to plot T4 files, like TLVIZ, but now on VMS (Decwindows)

Dix can work in five modes

a. Full screen mode. DIX uses SMG to display the data on the screen and 
   allows you to modify it (if you specify /MODIFY (not the default))
   This mode is the default on a terminal.
   You enter this mode by specifying the /SCREEN qualifier.

b. Interactive mode. DIX will prompt you for commands. This mode can also
   be used in batch with command files. If you specified /MODIFY
   the file can also be modified (not the default). 
   You also have poweful scripting possibilities, including a debugger.
   You enter this mode by specifying the /INTER qualifier.
   See the help with DIX/HELP INTER for possible commands.

c. File mode. DIX will display one or more records and returns to DCL. No
   interaction is possible.
   You enter this mode by specifying the /FILE qualifier.

d. Demo mode. In this mode DIX will demonstrate how to use it.
   You enter this mode by specifying the /DEMO qualifier. 

e. Plot mode. DIX can plot variables, T4 files, and general CSV files, 
    using DECWINDOWS.
   
In the first three modes the data can be displayed in two ways.

a. Interpreted. You need a record description to do this.
   The description file syntax looks like fortran record definitions(structures)
   and the descriptions can be in a file or in the DIX_DES.TLB text library.
   The layout of the description files is described in the DIX
   help library under the topic RECORD_FORMAT_FOR_DESCRIPTIONS

   See DIX/HELP RECORD_FORMAT

   DIX delivers (in DIX_DES.TLB) descriptions for the following files
    SYSUAF.DAT
    RIGHTSLIST.DAT
    INDEXF.SYS 
    MONITOR            (filename MONITOR.*)
    BACKUP_SAVE_SETS   (extension .bck or .sav)
    VMS$PASSWORD_HISTORY
    VMSMAIL_PROFILE
    DIRECTORY files   (filename .DIR)
    ACCOUNTNG.DAT     (filename ACCOUNTNG.*)
    Some ALL-IN-1 files.
    AUDIT$JOURNAL files
    QUOTA.SYS 
    TCPIP$HOST
    VMSMAIL_PROFILE

   And you can add any file you like, if you know the record layout.
   For a complete list use DIX>show desciptions/all  

b. Raw dump. The program displays the data like VMS DUMP and
   you can modify any byte/word/longword in hex/decimal/octal/binary.


  See the help via the HELP command.

usage:
Define DIX as a foreign command

DIX:=$'directory'dix_alpha or DIX_VAX or DIX_IA64

DIX[/mode] filename /qualifiers

For the qualifiers see the help via DIX/HELP, but one very 
one important one is the /MODIFY. If you do not specify /MODIFY the
file is opened readonly and cannot be modified.

You can also get some idea about the possibilities of DIX via the
demo mode DIX/DEMO, and select one of the demo's.

Note:
The files DIX.HLB and DIX_DES.TLB must be in the same directory as DIX, 
or you must define the logical DIX_HELP and/or DIX_DES to another file.

Building:
The executables and objects for Vax,Alpha and IA64 are skipped with the kit,
as well as the sources. If you want to rebuild DIX, goto the dix directory and
use the command procedure @MAKE_DIX_'arch' [LINK]. 
This will compile (if p1<>"LINK") and link DIX.
You will need a fortran compiler to do the compilation.

Examples:

$DIX[/SCREEN] SYSUAF/EQ=smith [/MODIFY]
  Will display the SYSUAF record of user "SMITH" using SMG, and lets
  you scroll though this data. If you specified /MODIFY, you can also
  modify entries (When you type any character on that field, you enter 
  modify mode for that field (this is signalled by an underline under
  the text)). You leave modify mode for that field when you type ENTER.
  The (modified) data is not written to the file until you type DO or PF4.
  F10 or ^Z returns you to DCL.

$DIX/INTER SYSUAF/EQ=smith [/MODIFY]
  Will enter interactive mode (non-smg), and allows you to inspect/modify
  fields of the current record. It also contains a scripting
  language. Type help in this mode to see the possible commands.

$DIX/DECW SYSUAF/EQ=smith [/MODIFY]
  Will enter decwindows mode, and allows you to inspect/modify
  fields of the current record.

$DIX/FILE SYSUAF/EQ=smith
  Will display the data on the terminal, but will not allow you to modify
  the record. If a description can be found, all fields are displayed,
  if not the output will be like the vms DUMP command

$DIX/FILE datafile/REC=10
  Will display the data on the terminal, but will not allow you to modify
  the record (DUMP command). Only record 10 will be displayed

$DIX/FILE datafile/RECORD=10/COUNT=5
  Will display the data on the terminal, but will not allow you to modify
  the record (DUMP command). The records 10 upto 14 (5) wil be displayed.

$DIX/DEMO
  Will start a demo session. You will be given a choise of various
  demo modes. They look like the real thing, but do NOT open/change 
  any file.

$DIX/PLOT T4_CSV040_26FEB2011_0001_2359_COMP.CSV;1
  Will start plot mode for a t4 file.

Example of a (complicated) description record of the INDEXF.SYS file
   the (n) are explained below

	ubyte		id_offset	! Offset to Ident area
	ubyte		map_offset	! Offset mapping area
	ubyte		acl_offset	! Offset to ACL area
	ubyte		res_offset	! Offset to reserved area
	integer*2	seg_num		! Extension seqment number
	byte struct_lev_min		! On disk structure level
	byte struct_lev_maj		! On disk structure level
	fileid          file_id		! File id
	fileid          ext_fid		! File id extension header
        structure       rec_attr	! RMS record attributes
         byte rectyp
         byte recattr
         integer*2 recsiz
         rinteger*4 hblk       !highest block with high/low word reversed
         rinteger*4 eofblk     !eof block also reversed, a heritance of the PDP11 
         integer*2 eofbyte
         byte bucketsize
         byte vfcsize
         integer*2 maxrec
         integer*2 defext
         integer*2 globbuf
         integer*2 %fil3(4)
         integer*2 verslim
	end structure
	bits*4		file_char -		(1)
		[Wascontig,Nobackup,Writeback,Readcheck,Writecheck,-
                 Contigb,Locked,Contig,,,,Badacl,-
		 Spool,Directory,Badblock,Markdel,Nocharge,Erase,alm_aip,-
                 shelved,scratch,nomove,noshelvable,shelv_res]
	character*2	%res_1		! reserved 1
	ubyte		map_in_use	! # map words in use
	byte		acc_mode	! File accessor priv mode needed
	uic		owner		! Owning UIC
	protection	protection	! File protection
	fileid		backl_fid	! Backlink file id
	bits*2		journal		! Journalling flags
	integer*2	ru_active	! Recover facility unit number
	intege*4	highwater	! Highest blocknr written + 1
	union
	 map struct_lev_maj=5                     (2)
	  byte FI5$B_CONTROL    [0=ODS-2,1=ODS-5] (3)
	  byte FI5$B_NAMELEN   
	  integer*2 FI5$W_REVISION   
	  date FI5$Q_CREDATE
	  date FI5$Q_REVDATE 
	  date FI5$Q_EXPDATE   
	  date FI5$Q_BAKDATE    
	  date FI5$Q_ACCDATE    
	  date FI5$Q_ATTDATE  
	  integer*8 FI5$Q_EX_RECATTR
	  integer*8 FI5$Q_LENGTH_HINT_LOW
	  integer*8 FI5$Q_LENGTH_HINT_HIGH
	  character*(fi5$b_namelen) FI5$S_FILENAME  (7)
	 end map
	 map struct_lev_maj=2
	  character*20	fnam		! Variable mapped entries
	  integer*2         revnr
	  date*8            cdat
	  date*8            rdat
	  date*8            edat
	  date*8            bdat
	  character*66	rest_fnam
	 end map
	 map *			(4)
	  integer*4 data(20)
	 end map
	end union
	range (map_offset*2:map_offset*2+map_in_use-1) (5)
           diskmap maps(256)	 
	end range
	range (acl_offset*2:res_offset*2-1)
	   acl acls(50)	
	end range
	position (min(510,max(0,recordsize)))    (6)
	integer*2/hex checksum

(1) is an example for a bits type. The part between the []
    gives an more friendly view of the bits.
    For example bit3 will be displayed as "readonly"
    Any bit not described will be displayed as BITnn (for example bit8)
(2) is an example of a union/map structure. A union contains one
    or more maps. This part is selected if the field STRUCT_LEV_MAJ 
    contains 5. See also (4)
(3) This is an example for an integer (byte). The part between
    the [] gives a list of possible values of the integer.
    For example : if the value of FI5$B_CONTROL is 1, the
    text displayed is ODS-5. If is is 0 the text displayed will
    be ODS-2. Anly other value than 0 or 1 will be displayed as the 
    normal numeric value.
(4) The rest of the union/map. If none of the map statements has
    matched, this one will. If you do not specify a map with an *,
    the first map will be taken (in this case the map struct_lev_maj=5 
(5) This is an example for the RANGE statement. A RANGE defines a
    part of the record. 
    In this case the field diskmap map(256) in contained in a part
    of the record between bytes map_offset*2:map_offset*2 and map_in_use-1
    The length 256 is choosen to be big enough.
(6) A example of a Position statement. This set the offset for the 
    next field to an absolute value (in this case 510).
    The next field (checksum) will be at offset 510.
(7) The length of the character string depends on a previous field
    (fi5$b_namelen). 

All fieldsizes are in bytes, except within a bitfield/endbitfield range.
In this bitfield_mode only (u)integer, (r)bits and logical fields are allowed.


Another example of files that are linked 

  The example is about 3 RMS indexed files that form a simple sourcemodule
  cross_reference system

  The first file (CROSS_REF.CRF_CROSS) has the following description
  (.CRF_CROSS in the system or user textlibrary)
          integer*2    caller_nr /file=.crf_mod_names   !link to modulename
          integer*2    called_nr /file=.crf_mod_names   !link to modulename

  The second file (CROSS_REF.CRF_MOD_NAMES) has the following description
  (.CRF_MOD_NAMES in the system or user textlibrary)
          integer*2     mod_nr                           !primary key
	  integer*2     nk_mod_name
          character*40  mod_name
          integer*2     file_nr/file=.crf_file_names     !link to the filename

  The third file (CROSS_REF.CRF_FILE_NAMES) has the following description
  (.CRF_FILE_NAMES in the system or user textlibrary)
          integer*2     file_nr                          !primary key
          integer*2     nk_file_name
          character*256 file_name

  When you now (dix-)edit the CROSS_REF.CRF_CROSS file
    $DIX/INTER CROSS_REF.CRF_CROSS
    %DIX-I-USINGFIL, Using file DSA40:[DIR]CROSS_REC.CRF_CROSS
    %DIX-I-USINGDES, Using description SYSLIB(.CRF_CROSS)
    DIX> EXA *
     0|CALLER_NR>|738             !the > tells us there is a link present
     2|CALLED_NR>|-262

 You can follow the link to the next file

    DIX> Follow CALLER_NR         !try to follow this link
    File .CRF_MOD_NAMES not (yes) opened, open it (y/[n]):Y     !do you want to open the file?
    %DIX-I-USINGFIL, Using file DSA40:[DIR]CROSS_REF.CRF_MOD_NAMES
    %DIX-I-USINGDES, Using description SYSLIB(.CRF_MOD_NAMES)
    DIX> Exa *
    0|MOD_NR  |738
    2|MOD_NAME|CHECK_ALLOWED_USER
   34|FILE_NR>|66                 !and this field also has a link defined

 Now follow the link to the third file (open automatic)

    DIX> Follow/automatic file_nr
    %DIX-I-USINGFIL, Using file DSA40:[DIR]CROSS_REF.CRF_FILE_NAMES
    %DIX-I-USINGDES, Using description SYSLIB(.CRF_FILE_NAMES)
    DIX> Exa *
    0|FILE_NR  |66
    2|FILE_NAME|REM_SERVER_CHECK_ACCESS

 Now backtrace to the CROSS_REF.CRF_MOD_NAMES file

    DIX> BACK
    %DIX-I-USINGFIL, Using file DSA40:[DIR]CROSS_REF.CRF_MOD_NAMES
    %DIX-I-USINGDES, Using description SYSLIB(.CRF_MOD_NAMES)
    DIX> Exa *
    0|MOD_NR  |738
    2|MOD_NAME|CHECK_ALLOWED_USER
   34|FILE_NR>|66
  
Now DIX also knows views. These are special descriptions that allow you to
combine fields from various files (like database views). These views make
use of the description files for the files.
For the above example you can also have used the following view (in dix_des.tlb)

  field caller_nr       !include the field caller_nr
  follow caller_nr      !follow the link on caller_nr to the .cref_mod_names
  gosub display_it      !go to a general routine to select the fields
  field/readonly called_nr  !include field called_nr (but reaonly)
  follow/read called_nr     !follow the link to the .cref_mod_names
  gosub display_it	!and again the general routine
  exit			!end of this view

  display_it:           ! general routine to display info of calle*_nr
    field mod_name      !select mod_name
    follow/read file_nr !follow the link to the .cref_file_names
    field file_name     !and display the filename
    back                !back to the .crf_mod_names
    back                !back to the .crf_cross
  return		!done with the routine

    $DIX/INTER CROSS_REF.CRF_CROSS/VIEW
    %DIX-I-USINGFIL, Using file USER50:[PROGRAMS.DIX.REGRES]DIX.CRF_CROSS;23/NOMOD
    %DIX-I-USINGVIEW, Using view DSA50:[PROGRAMS.DIX]DIX_DES.TLB;1(VIEW#.CRF_CROSS)
    DIX>exa
    Data from View
      0|CALLER_NR  |1085              !show caller nr
      2|MOD_NAME   |DIX_CLD           !show module name for called
     42|FILE_NAME  |DIX_CLD.CLD       !show filename
                   |                  !256 bytes
                   |                  !so multiple line
                   |
    298|CALLED_NR  |34                !show called nr
    300|MOD_NAME_1 |DIX_MODE_SCREEN   !show module name for called routine
    340|FILE_NAME_1|DIX_MAIN.FOR      !and its filename
                   |                  !and again multiple lines
                   |
                   |


For a complete list of directives see the help in the DIX_HELP.HLB under the
topic "RECORD_FORMAT". Use DIX/HELP to display this help.

Note:

Be careful when modifying datafiles. DIX is very powerful and has no UNDO
function after you update the record. If you modify the record, there is 
no way back except the backup (if you have one). /BLOCK mode is even more
powerful (and also dangerous), since it bypasses RMS. 

This package contains the following files

In the main directory

 DIX_VAX.EXE			The Vax executable
 DIX_ALPHA.EXE			The Alpha Executable 
 DIX_IA64.EXE			The IA64 Executable 
 DIX_HELP.HLB			The help library
 DIX_DES.TLB			The file containing the description records
 FREEWARE_README.FIRST		This file
 RELEASE_NOTES.TXT   		The release notes
 DIX_RGB.DAT                    The colour table. This file is needed if you
                                 want to plot to a .ps file and no decwindows 
                                 is installed

In the [.SOURCE] directory

 The FORTRAN sources

  DIX*.FOR			A lot of source files
  DIX*.INC                      The include files (definitions)
  DIX.VERSION			The version file
  DIX.OPT                       The options file
  DIX_HELP.HLP                  The Help file
  DIX_CLD.CLD			The command line definitions
  DIX_MESSAGE.MSG               The Message file
  MAKE_DIX_VAX.COM		The command procedure to compile/link VAX
  MAKE_DIX_ALPHA.COM		The command procedure to compile/link Alpha
  MAKE_DIX_IA64.COM		The command procedure to compile/link IA64


In the [.VAX]   directory the VAX   objects
In the [.ALPHA] directory The Alpha objects
In the [.IA64]  directory The IA64  objects

Instructions:

 Unpack the save set
  $BACKUP DIX_vers[_a].BCK [...]/log
    vers is the dix version f.e. 0830
    a    is the optional architecture (V(ax), A(lpha) or I(A64)
   This will create a [.DIX...] directory tree

 If you want to rebuild the program, goto the [.source] directory

    $set default device:[directory.dix.source]
   If you have a fortran compiler 
     $@make_dix_'architecture'       (vax/alpha/ia64)
   otherwise, just link
     $@make_dix_'architecture' link  (vax/alpha/ia64)

 DIX was linked with VMS-VAX 7.3, VMS-Alpha 8.4, VMS-IA64 8.3-1.
 If you have older versions (specifically VMS-Alpha below 7.3) you
 may have to relink. I included all objects so you can relink without
 compiling.

 If you do not have DECWINDOWS installed, DIX will not start because
  SYS$SHARE:DECW$XLIBSHR cannot be found. In this case you have to relink DIX.
  Before you link DIX you must remove the line SYS$SHARE:DECW$XLIBSHR/SHARE in 
   the make_dix_'architecture'.com file, 
   and then use @make_dix_'architecture' LINK
  This will generate some messages from the linker about missing X$* functions,
  but DIX will link correctly, and of course you cannot use the DIX/DECW mode.
 DIX now distributes the DIX_RGB.DAT file to allow plotting to a postscript file
  and no DECWINDOWS installed (it is a copy of the sys$manager:decw$rgb.dat)
  DIX/PLOT file /post=x.ps 

The most recent version can be downloaded from oooovms.dyndns.org.

Author: Fekko Stubbe

If you have questions or suggestions, please mail to the mail address below:
Email : dixdev (at) oooovms.dyndns.org 
