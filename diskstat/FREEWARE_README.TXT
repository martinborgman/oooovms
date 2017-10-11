Diskstat,utility, A program to display consequences of changing clustersize

DISKSTAT can do three things
1. Report of the consequences of a change to clustersize
2. Report on filesiz distribution
3. Report on specific file type

Author: Fekko Stubbe
Please mail problems/wishes to : dixdev (at) oooovms.dyndns.org

Ad 1.

This utility displays the consequences of changing the clustersize of a 
disk. It will not change the clustersize, but only report.
If you enlarge the clustersizem, you will more blocks allocated, but unused
the gap between the EOF block and the Allocated size will increase. 
IF you have a lot of small files, increasing the clustersize will have a 
bigger impact than when you have only one very big file.

Diskstat needs read access to the indexf.sys on the disk.

DISKSTAT can also report on size distributions

Usage
Define a foreign command
$diskstat:=$device:[directory]diskstat

To report on clustersizes you can specify
$diskstat device /cluster[=(size1,size2,size3...)][/output=filename]

   Default list is 1,2,3,4,8,16,18,32,35,64,70,128
   Default output is sys$output

Example:

$diskstat/clus dsa0:

Cluster size      3 Total blocks    9086133 waste =   0.40% Current
Cluster size      1 Total blocks    9049753 waste =   0.00%
Cluster size      2 Total blocks    9075892 waste =   0.29%
Cluster size      3 Total blocks    9086133 waste =   0.40%
Cluster size      4 Total blocks    9126944 waste =   0.85%
Cluster size      8 Total blocks    9245400 waste =   2.16%
Cluster size     16 Total blocks    9495856 waste =   4.93%
Cluster size     18 Total blocks    9556866 waste =   5.60%
Cluster size     32 Total blocks   10062368 waste =  11.19%
Cluster size     35 Total blocks   10161095 waste =  12.28%
Cluster size     64 Total blocks   11290240 waste =  24.76%
Cluster size     70 Total blocks   11490710 waste =  26.97%
Cluster size    128 Total blocks   13848832 waste =  53.03%

The waste is calculated from a clusterize of 1 (hence the 0.0% on clustersize 1)
The first line is the current clustersize.
So if you would increase the clustersize for this disk to 16, you would
waste 4.93%(counting from clustersize 1) or 4.53%(from the currens clusterize)


ad 2.
Diskstat can also report on the filesize distribution

$diskstat device /size=(size_opt,size_opt...)[/output=file] [/allocated]

 Diskstat will count filesizes in a table. Each table element has a 
 maxvalue. All files upto this maxvalue(and larger than the previous 
 tableelement) will be counted in this table element.
 Normally DISKSTAT will use the EOF size (except for indexed files),
 If you specify /ALLOCATED, DISKSTAT will use the allocated size.

Size_opt can be
  tablesize=n             The table has n elements, default is 8192
  increment=n             Each element is n blocks
  factor=n                Each element is n times as large as its predecessor
  values=(v1,v2,v3)       We begin with a list of v1,v2,v3 and then multiply
                          these values by the factor value.
  All                     Report empty elements too

 
Examples:
  increment=3                 : Elements 3,6,9,12,15,....
  Factor=2                    : Elements 2,4,8,16,32,...
  Values=(1,3,5,8),factor=10  : Elements 1,3,5,8,10,30,50,80,100,....

Default is increment=1

 The output can be formatted in three ways
  1. The default is lowval-highval:count (if increment=1 val:count)
     and as as much as will fit on a line.
  2. One table elements on one line (size_opt=oneline)
  3. A csv type report, this might be used for Excell.
      size_opt=csv[=separator]. Separator is 
         space comma tab colon semicolon bar slash or character=nn

Examples
$diskstat dsa0:/size : Will report all sizes per block

   0: 471    1:6510    2:2559    3:5790    4:1564    5:1412    6:3051    7: 746
   8: 641    9:1817   10: 512   11: 455   12:1078   13: 449   14: 374   15: 616
 ......
7957:   1 7958:   1 8039:   1 8157:   1 >8192:92(5293727 blocks,max=2073708)
Found 45385 files, total nblock 9049755, average size 199

$diskstat dsa0:/size=incre=10
    0-    9:24561    10-   19: 5431    20-   29: 3036    30-   39: 2318
   40-   49: 1274    50-   59:  885    60-   69: 1077    70-   79: 1376
......
52600-52609:    1 65530-65539:    1 77550-77559:    1
>81920:8(3677696 blocks,max=2073708)

$diskstat das0:/size=factor=3
       0-       2: 9540        3-       8:13204        9-      26: 9455
      27-      80: 7791       81-     242: 2887      243-     728: 1390
     729-    2186:  702     2187-    6560:  303     6561-   19682:   77
   19683-   59048:   26    59049-  177146:    7   177147-  531440:    1
  531441- 1594322:    1  1594323- 4782968:    1
Found 45385 files, total nblock 9049757, average size 199

$diskstat dsa0:/size=(value=(1,3,5,8),factor=10)
        0-        0: 471         1-        2:9069         3-        4:7354
        5-        7:5209         8-        9:2458        10-       29:8467
       30-       49:3592        50-       79:3338        80-       99: 621
      100-      299:2636       300-      499: 681       500-      799: 440
.....
  1000000-  2999999:   1
Found 45385 files, total nblock 9049757, average size 199

$diskstat das0:/size=(factor=3,csv) 
0,2,9540                            lowval,highval,count
3,8,13204                           
....
531441,1594322,1
1594323,4782968,1

$diskstat das0:/size=(factor=3,csv=char=64) 	!use the @ as separator
0@2@9540
3@8@13204
....
531441@1594322@1
1594323@4782968@1

Ad 3.
DISKSTAT can report on specific files

$DISKSTAT device/report=reporttype[/statistics]
  /statistics will print counts of the report types

reporttype can be
overallocated       File with eof&lt;allocated-clustersize
organization=org    Files with specific organization, 
                     org can be: indexed,seqential,relative,other
format=fmt          Files with specific record format
                     fmt can be: udf,fix,var,vfc,stream,stmlf,stmcr,other
mask=mask           Report only files (wildcard-)matching "mask"
directories         Report directories
notcontig           Report non-contiguous files
empty               Report empty files
mark_deleted        Report file marked for delete
multiheader         Report multi header files
full_name           Report full filename instead of the name in the fileheader
                     This will result in more resource usage

Examples

$diskstat dsa0:/report=mark		!report mark deleted files
Markdeleted DCLTABLES.EXE;830 Size 1436
Markdeleted DCLTABLES.EXE;765 Size 1437

$diskstat dsa0:/report=(mark,full)      !the same but now with full filename
Markdeleted DISK$SYS_ITV002:[VMS$COMMON.SYSLIB]DCLTABLES.EXE;830 Size 1436
Markdeleted DISK$SYS_ITV002:[VMS$COMMON.SYSLIB]DCLTABLES.EXE;765 Size 1437


$diskstat dsa0:/report=mark/statis	!add some statistics
Disk _DSA0: Volume SYS_ITV002   MAX_FILES  =  1047510
 Fileheaders (max_files  ) total  1047510 used    45719 free  1001791(  4.36%)
Current size indexf.sys =   157410 blocks, clustersize     3
 Fileheaders (Indexf size) total   157141 used    45719 free   111422( 29.09%)
Extentarea for indexf.sys, used  13 avail 155(  8.39%)
#blocks used    8843288 Blocks allocated    9164349 Waste =       3.63%
  #Overallocated files       4541, Total size         78213
  #Directories   files       2787, Total size          2787
  #Notcontig     files       3110, Total size       2802165
  #Markdeleted   files          2, Total size          2873
  #Multiheader   files         26, Total size       2809259
  #Empty         files        471, Total size             0
Counts for file organizations
  #Sequential    files      45218, Total size       8984014
  #Relative      files          2, Total size            18
  #Indexed       files        160, Total size         65718
  #Other         files          5, Total size             7
Counts for record types
  #Undefined     files         39, Total size       1349249
  #Fixed         files       4597, Total size       5485927
  #Variable      files      10989, Total size        602300
  #VFC           files       1088, Total size         38590
  #STream        files       2867, Total size        110960
  #StreamLF      files      25803, Total size       1462720
  #StreamCR      files          2, Total size            11
  #Other         files          0, Total size             0

