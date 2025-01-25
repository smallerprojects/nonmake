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



