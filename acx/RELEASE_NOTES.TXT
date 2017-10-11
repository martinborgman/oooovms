acx,utility,An enhanced accounting utility

Usage:
acx:=$disk:[directory]acx   !define a foreign command

acx [qualifiers] [parameter] [qalifiers]

Parameters
The acounting files to process, default=sys$manager:accountng.dat
Just like accounting, but with a lot of extra selection qualifiers

Qualifiers

Most of the normal accounting qualifiers are supported

See the help for accounting for these. ACX will use wildcard matching
for text fields so /user=*SYS* will match all username *SYS*.
ACX also supports the /user=-system  (all records except user=system)

/ACCOUNT
/ADDRESS
/BEFORE
/ENTRY
/IDENT
/IMAGE
/JOB
/NODE
/OWNER
/PRIORITY
/PROCESS
/QUEUE
/REMOTE_ID
/SINCE
/SORT
/STATUS
   Thas has been enhanced to allow for 
       /status=FATAL,WARNING,ERROR,SUCCESS,INFORMATIONAL
/TERMINAL
/TYPE
/UIC
/USER

/BINARY           Output is in binary format

And a few specific to ACX

/HELP
                  Display the contents of the help file

/OUTPUT=filename  This decides between the filemode and screen mode
                   See below

/FIRST=n          In file mode print only the first "n" records.
                   In Screen mode this qualifier is ignored.
                   See also the /separated qualifier for more info
/LAST=n           In file mode print only the last "n" records.
                   In screen mode, start display at the 'last-n' record
                   See also the /separated qualifier for more info
/BRIEF            Display data in brief mode. In Screen mode you can toggle
                   between full and brief mode via the F12 key
/WIDE             Set the screen to 132 in screen mode, or use more then 80 cols
                   in the output file for brief mode (the max size is 176).
/SEARCH=searchstring
                  In file mode  : print only those records that 
                                  contain "searchstring"
                  In screen mode: goto the first selected record that 
                                  contains "searchstring" 
                     searchstring is NOT case-sensitive
/HEADER[=ALL]     Enable the header in brief mode to file (default).
                   /NOHEADER will not print a header
                   If /HEADER=all is specified, and also /separated, and
                   ACX found more than one list (see the /separated for 
                   details) ACX will print headers between the lists.
                   (in file mode only)

/DAY=(day,day,..)  Select only records for specific days
                   Day can be monday,tuesday,wednesday,thursday,friday,
                   saturday,sunday and weekend,workday. The selection
                   is by default on the termination date.
/TIME=(range,range..)
                   Select only record in specific time intervals
                   range can be hh:mm:ss.tt-hh:mm:ss:tt (so 12:15-12:30)
                   Range can contain wildcards like 
                   *:10:10:*-*:10:20:*, all records for between 10:10 
                   10:20 (mm:ss) after all hours. The time selection is
                   by default on the termination time.

           with the /time and /day you can select records from more 
           then one day in the time interval. If you include /since
           and or /after, you have a powerful time selection.
 
/SELECT=(selectclause[,selectclause...])

   Select record on field contents

   selectclause=keyword=(min=value,max=value)

    Keyword can be 
      cpu           Select on CPU   usage, value is deltatime
      dirio         Select on DIRIO usage, value is integer
      bufio         Select on BUFIO usage, value is integer
      pagefaults             etc...
      pfltread
      peakworking
      peakpagefile
      volmounted
      imagecount
      pagesprinted  For print records, value is integer

  Examples
    /select=cpu=(min=00:00:05)    Only record with >=5 seconds cpu
    /select=dirio=(min=10,max=20) Only record with DIRIO beteen 10 and 20
    /select=imagecount=max=5      Only record with imagecount<=5
    /select=pages=(min=10)        Only print records with >= 10 pages

/SEPARATED

  If the user specifies one of the selection criteria  ACX will maintain 
  a separate list of records that satisfy the(se) selection-criteria.

   f.e /user=(system,john)/que=(printer1,printer2)
   There will be 4 lists (system on printer1, system on printer2,
                         john on printer1 and john on printer2) 
    (Since the fields may contain wildcards, the number of lists may be high)  

  If you want to select on fields, but you do not want a separate list
   preceed (one of the) items with an #-sign.

   So    /user=(system,john)/que=(#printer1,printer2)
     or  /user=(system,john)/que=(printer1,#printer2)
    Will select only the records with queuename printer1 or printer2
    but create only 2 lists (one for user system and one for user john)

  The /last and /first work per list, so if you specify /last=1
  you will see the last record of each list.

  If you specify /SEPARATED, ACX will print (or display) the records
  sorted by list, and within the list, sorted by time.
  If you do not specify /SEPARATED, ACX will display the records sorted
  by time only.

  In screen mode, you can toggle between the separated mode (a lot
  of lists) and single list (all records together). You can also
  select on the separated list via the F9(default key mapping) key.
 
ACX will use the accounting qualifiers and parameters (the filenames)
to make a list of records that match the criteria, and then display
those records. Since ACX uses Virtual memory to save the record, 
you must specify a reasonable of selection criteria, or the program
will skip the records after depleting the pagefilequota

ACX works in two modes

1. File mode. If you specify /output, ACX will print all selected
   records to the specified file, and no interaction is possible
   If you specify /last=n , only the last N will be printed
   If you specify /first=n, only the first N will be printed
   The /separated qualifiers decides if there is one long list
   of records, or a list of small lists. 

2. Screen mode. If you do not specify the /output, ACX will enter
   smg mode. If you have specified the /last=n, of /first=n  
   ACX will display only the last or first n record (per list)

   You can use the some keys to select another record.
   Supported keys are below

   ACX allows you to redefine keys. See the remarks at the bottom
   The next lines assume the default setting

   Left/right arrow: Mover horizontally
   Down arrow     : GOTO the next record
   Up arrow       : GOTO the previous record
   PF1-Down arrow : go forward 1% of the records
   PF1-Nextscreen : Go backward 1% of the records
   Nextscreen     : Go forward 10% of the records
   Prevscreen     : Go backward 10% of the records
   PF1-Prevscreen : GOTO the first record
   PF1-Nextscreen : GOTO the last record
   PF3            : Find next/previous record that contains searchstring
                    If no search string has already been specified you must enter one
   PF1-PF3        : Modify find string first and then search
   Find           : Find next/previous record that contains searchstring in
                    a specific field with specific search criteria
                    If no search string has already been specified you must enter one
   PF1-Find       : Modify find string/settings first and then search
   Keypad 4       : Set search direction to forward
   Keypad 5       : Set search direction to Backward
   PF2 or HELP    : Display help
   PF4            : Toggle screen width 80<>132
   F12            : Toggle between brief and full mode
   F10 or ^Z      : Exit the program
   ^W or ^R       : Repaint the screen
   0..9           : Display a window to enter a record number
                    If the number is >0, goto that record
   D,H,M,S        : goto the record with a Day,Hour,Minute,Second different
                    from the current one in the search direction

   P,I,Y,L,R,U,F  : Goto a record of type
                     Process,Image,sYsinit,Logfail,pRint,User,File
                     in the search direction
 PF1-P,PF1-I,PF1-Y,
 PF1-L,PF1-R,PF1-U,
 PF1-F            : Goto a record NOT of type
                     Process,Image,sYsinit,Logfail,pRint,User,File
                     in the search direction
   TAB            : Goto a record with a different search pattern
                     in the search direction
   F11            : Toggle the separated mode
   F9             : Display a list of lists, and let the user select one.


User defined keys

With acx/key_definitions[=filename] you can override the default acx key
definitions. The filename has as default value acx_key_defs, and the
default filename programdiskdir:.keydef.

In this file are instructions for modifications, and the names of the keys
and their functions

Building:

If you have the fortran compiler
@make_acx_'arch'         !compile and link

You can always do the following

@make_acx_'arch' link    !only link

Author
Fekko Stubbe

