analyze_rms,Utility, A decwindows interface for ANALYZE/RMS

This program is a decwindow's version of analyze/rms
 It displays the contents of indexed (and also relative and sequential)files
 using decwindows, and lets you display 
   fileinfo,keyinfo,areainfo,bucketinfo,recordinfo
 It also explains how the various RMS-structures are used, and where the
 definitions can be found.

Usage

$arms:=$'device'['directory']analyze_rms_alpha
$arms:=$'device'['directory']analyze_rms_ia64   
$arms:=$'device'['directory']analyze_rms_vax   

$arms filename

You can left click on each white underlined item. 
A red underlined word can be left clicked to get an explanation about the word
On most windows a right click will popup a window with an explanation

Installation

Unpack the kit

@make_analyze_rms_alpha       will compile(fortran) and link the alpha version
@make_analyze_rms_alpha link  will link the program

@make_analyze_rms_ia64        will compile(fortran) and link the ia64 version
@make_analyze_rms_ia64 link   will link the program

@make_analyze_rms_vax         will compile(fortran) and link the vax version
@make_analyze_rms_vax link    will link the program

The file analyze_rms_tlb.tlb contains the help texts. This file must be
in the same directory as the image

You may add explanations about words (the red underlineds ones).

Suppose you want to add an explantion about the word "indexed"
Create a text file HW_indexed.txt with

1. A header line like "indexed files"
2. any number of lines containing the info you want

then $LIBRARY/INSERT ANALYZE_RMS_TLB.TLB HW_INDEXED.TXT

After that if analyze_rms finds the text indexed (case insensitive) anywhere
in the text, it will underline the word in red, and allow you to click
on it and a window will pop up with your explanaiton


Author
Fekko Stubbe
