# nonmake
Building C/C++ projects without a make-system

This nonmake project is meant to show how to build small to medium sized C/C++ projects without a make-system. Currently focused on a Windows environment, but Microsoft is spending a lot of energy to chase away its users, so linux will follow naturally at some time.

With a make-system is meant any combination of low level make or meta make-system, like make, nmake, cmake, ninja, bazel, etc., or better, a bloated or extremely complicated syntax ridden system that chases away many beginners that like to start software development in C or C++. The nonmake project is meant to show beginners (or even advanced developers) how they can start creating their own C-projects without these mostly bloated systems, by using easy commandline commands or a simple batch- or shell-script. Example batch-files are given to explain the methods how to develop in C/C++ without a make-system, also examples are given to build several projects found on github with just a batch-script.

# Make systems

Although there are a lot of use-cases where a kind of complicated make- or build-system is absolutely necessary, nowadays where the hardware and storage devices are many hundreds of times faster then the hard-disks of 30+ years ago, the need for a complicated make-system is more or less totally useless and only adding bloat in a small to medium sized project. Even for very large projects, it is not always needed to make use of a make-system. Large systems can also be set up with libraries and or groups of object-files such that only a part of the project needs to be rebuild for certain purposes.

The famous make was introduced a long time ago mainly for handling dependencies between source-files and was very helpfull in that era, because rebuilding every source-file was very time-consuming and only building the files that needed to be build was helping a lot to keep buildtimes acceptable low. Nowadays where a typical c-file can be read in within a fraction of a second, the need for a make-system is more or less gone for small to medium sized projects. To keep the makefile for the make system useful, in the past it was also necessary to maintain the dependencies manually, so if a function was moved from one c-module to another and also h-files needed to be updated, one also had to update the dependencies in the makefile, and of course, because not doing this, didn't directly lead to problems, and resulted in many hard to diagnose build-problems later on. And the bigger the project became, and the make-system was becoming more and more usefull, maintaining these dependencies manually was almost impossible ...

There are many small C/C++ projects on github with only 10-20 source files, and have more files and/or directories just for supporting multiple build-systems and hard to read text (and mostly incorrect or not-working) documentation to explain how to build the project and most of the time only one environment is explained correctly and where the documentation is matching reality.

Cmake for example is an extremely bloated system, that can generate files with thousands of hard to read "cmake code" and that tries to support any variations of installed packages and versions of dependencies and of course there is a percentage that is not supported (yet) and if one encounters that problem, it is up to you to debug and find the problem in the make system.

# Rant about make-systems

A rant about make-systems only has value if the disadvantages can be illustrated by clear examples where a make-system is only hurting a project and not helping in any way.

# LibZip example github project

Following example is meant to illustrate the problems with make-systems and is not meant to criticize the projects itself, in this case libzip and zlib. Thousands of other github projects could have been used to visualize the modern problems with make-systems.

Suppose one is looking for a zip-library that can be used in a project and finds a project that seemingly does exactly what one was looking for, the libzip library:

https://github.com/nih-at/libzip

If one downloads the libzip-main.zip file (The whole essence of this nonmake project is to show how to build a c-project without all kind of dependencies and installed packages, so this also explains the steps without even having git installed) and extract the files in a directory. The INSTALL.md mentions that the project needs to be build using cmake, it also mentions 10! other mandatory and optional packages (11 with cmake included) to be installed in order to build or use the library. So if one tries to do that on a Windows 10 system with Visual Studio and cmake installed, your environment and results may vary, it stops (after more than 3 minutes(!) of strange c-function checking, with the following error-message:

COULD NOT FIND ZLIB (missing: ZLIB_LIBRARY ZLIB_INCLUDE_DIR)  (Required is at least version "1.1.2")

But the INSTALL.md mentions that zlib comes with most operating systems. Apparently Windows 10 does not fall into that category? And this is the first and only mandatory package/library that is needed for libzip. That does not give much confidence for the 9 other optional packages. So we have to install zlib first, but from the error-message and the use of cmake that hides all the intelligence, it is absolutely not clear how to install zlib, such that cmake finds it correctly. The error-message for example seems to point to the environment variables ZLIB_LIBRARY and ZLIB_INCLUDE_DIR that need to be set correctly. The link in the INSTALL.md file points to the following site:

https://www.zlib.net

However, that site does not show any installer to install, only 3 compressed files with source-code and a cryptictally described zlib.tar.gz file, named "Permalink for the most recent release:", so installing yet another application (for example 7zip) is mandatory, just to check what is contained in that file ... and appears to be an Install-on-Demand archive ... after installing that package, of course did lead to the same error for the missing ZLIB library. So, ..., where to go from this?; try to set the environment variables, try to read and understand the 500 lines of the CMakeLists.txt file, or try to find the problem in the 43 files and 14 directories that cmake generated, or try to find the problem in the CMakeCache.txt file with 750 unreadable lines of cmake code? Or ... try to take the easy route and try to build the zlib library from source? The latter it is ...

After downloading the zlib source code I discovered that my ExtractAll option in Windows to extract a zip-file was replaced by Express Zip from NCH?!@#@? And trying to unzip the zlib131.zip took ages (several minutes). Considering the fact that the Express Zip replaces a well working unzip option in Windows with an unworkable slow application can be considered malware. So how can this rabbit hole just trying to build the libzip library get worse?

The zlib project contains 253 files and 36 folders! and only 15(!) c-files to build the library!! So many more make-system related files than actual source-files! Running cmake in a build folder results in 53 extra files in 15 directories and not even building the library! But because it creates a zconf.h file needed for the c-files to be build, it creates the zconf.h file needed for building the c-files with the following commands:

...
cl -c *.c
lib *.obj /OUT:zlib.lib
...

That is it, these two commands replaces ALL the cmake bloat, all the MB's of make-system bloat and ALL the unnecessary directories and other make-files that only add confusion and absolutely no benefit. If one only wants to build the library in a single environment! But for linux this would also be a few simple commands. So in essence the zlib project can be condensed from a 4.36 MB 255 files in 36 directories project (4.85 MB, 307 files and 52 directories after cmake run), into a 2.31 MB, 52 files and 3 folder project and a 2-line batch-script to build the library without make-system bloat within a second. It takes much more time to remove all the make-system bloat than just building the library with the 2 simple commands!


