# nonmake
Building C/C++ projects without a make-system

This nonmake project is meant to show how to build small to medium sized C/C++ projects without a make-system. Currently focused on a Windows environment, but Microsoft is spending a lot of energy to chase away its users, so linux will follow naturally at some time.

With a make-system is meant any combination of low level make or meta make-system, like make, nmake, cmake, ninja, bazel, etc., or better, a bloated or extremely complicated syntax ridden system that chases away many beginners that like to start software development in C or C++. The nonmake project is meant to show beginners (or even advanced developers) how they can start creating their own C-projects without these mostly bloated systems, by using easy commandline commands or a simple batch- or shell-script. Example batch-files are given to explain the methods how to develop in C/C++ without a make-system, also examples are given to build several projects found on github with just a batch-script.

# Make systems

Although there are a lot of use-cases where a kind of complicated make- or build-system is absolutely necessary, nowadays where the hardware and storage devices are many hundreds of times faster then the hard-disks of 30+ years ago, the need for a complicated make-system is more or less totally useless and only adding bloat in a small to medium sized project. Even for very large projects, it is not always needed to make use of a make-system. Large systems can also be set up with libraries and or groups of object-files such that only a part of the project needs to be rebuild for certain purposes.

The famous make was introduced a long time ago mainly for handling dependencies between source-files and was very helpfull in that era, because rebuilding every source-file was very time-consuming and only building the files that needed to be build  

There are many small C/C++ projects on github with only 10-20 source files, and have more files and/or directories just for supporting multiple build-systems and hard to read text (and mostly incorrect or not-working) documentation to explain how to build the project and most of the time only one environment is explained correctly and where the documentation is matching reality.

Cmake for example is an extremely bloated system, that can generate files with thousands of hard to read "cmake code" and that tries to support any variations of installed packages and versions of dependencies and of course there is a percentage that is not supported (yet) and if one encounters that problem, it is up to you to debug and find the problem in the make system.

# Rant about make-systems

