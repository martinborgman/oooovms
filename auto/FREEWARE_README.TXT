AUTO,tool, A program to AUTOmatically expand DCL commands

AUTO is a tool to let you use command completion
Each token of the command line (VERB, QUALIFIER, PARAMETER
or QUALIFIER-Value) can be expanded to completion
or sequentially searched for the next or previous valid match.
This tool will display the buildup command after the ENTER
key, but if you specify the /EXECUTE qualifier,
the buildup command will be executed via LIB$DO_COMMAND, and
control will return to AUTO. (see examples in the user guide)

AUTO makes use of the DCL tables in P1-space (so the link with /SYSEXE or SYS.STB). 
Symbols are also recognized, see the following table

1. a symbol that executes a dcl_command (@)     : allow the /output and upto 8 parameters
2. a symbol that executes a foreign command ($) : Look in the image for a command-table structure
                                                  and use that for definitions. If none is found
                                                  a $REST_OF_LINE parameter is assumed.
3. a symbol that executes MC(R)                 : this will revert to case 2
4. a symbol that translates to a normal verb    : Lookup the verb in the normal DCL tables


Warning: this programs uses the CLD tables in P1 space or in an image , so if these tables 
are not correct or incomplete, the command may not be executed correctly.

Building info
Objects and image are in the kit. If you want to build the program
Since the program has been linked with /SYSEXE, and was build under 
VMS 8.2, you may have to relink the program if you are on a different
version of VMS.

 1 : Unpack the kit         !$backup AUTO.bck/sav [...]
 2 : @MAKE_AUTO_'ARCH'      !if you have a recent fortran compiler
                            !arch = VAX,ALPHA,IA64
 3 : @MAKE_AUTO_'ARCH' LINK !otherwise


See the User_GUIDE.TXT for further information.
The most recent version can be downloaded from oooovms.dyndns.org.
If you have suggestions or questions, please mail to the address below

Author : Fekko Stubbe
E-mail : autodev (at) oooovms.dyndns.org
