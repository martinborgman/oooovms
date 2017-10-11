
This document describes system requirements and installation instructions for
OpenOffice.org 1.0 under Tru64 UNIX.  For information about building 
OpenOffice.org under Tru64 UNIX, see BUILD INSTRUCTION.  For information
regarding known issues in OpenOffice.org 1.0 under Tru64 UNIX, see RELEASE
NOTES.  For information about OpenOffice.org, visit www.openoffice.org.


System Requirements
===================

Before you start the installation, you must ensure that your system satisfies
the recommended software and hardware requirements for the type of system you
are working on.  For Tru64 UNIX, these are as follows:

Software Requirements:

	*  Tru64 UNIX V5.1A  (with all available patches)
	*  Compaq C++ V6.3-13.  To get the C++ kit, visit
 		www.tru64unix.compaq.com/cplus
	
Hardware Requirements:

	*  128 MB RAM
	*  3 GB free disk space


Installation
============

Get the file called install.tar.gz from the Linux and Tru64 UNIX Portability
CD.  Then do the following steps:

> gunzip install.tar.gz
> tar xvf install.tar

The installation file will be uncompressed into a directory called install.
After the file is uncompressed, go into the directory and run 'setup'.

There are two installation models: Single user installation and Multi user
installation.

1. Single user installation

In a single user installation, the product will be installed in one single
step on the computer of the user.  All necessary system configurations such as
writing data into the registry, installing fonts, creating program groups, 
etc. will be done for this single user.  After the installation, the product 
is ready for use.

        % ./setup

2. Multi user installation

The system administrator installs the product into a common location on the 
computer which can be accessed by every user.  During this installation, most
of the system configuration such as font installation, file types registration,
creation of program groups, program files linking, etc. will be done for all
users.

Before a user can use the product for the first time, he has to run the user
installation to copy all the user specific data into a user specific folder.
When he tries to start the product without running the user installation first,
the user installation will be started by the product automatically.  When the
user installation has finished, the product will be started again.

The difference to the single user installation is that different users of the
same computer share the same product installation but use their own user data.

1) As root, run setup with the -net option.
        # ./setup -net
2) As user, run again without the -net option.
        % ./setup

For details on setting up network installation, visit
http://installation.openoffice.org/proposals/netinstall.html

The user installation in step 2) copies some local files and uses approximately
2.7 meg of disk space.

After setup, goto the directory you chose to install OpenOffice.org (default is
OpenOffice.org1.0) and run any of the following applications from the "program"
directory:
soffice  smaster  swriter  simpress  scalc  sdraw  sweb  sfax
sagenda  sletter  svcard   spadmin   slabel smath  smemo

