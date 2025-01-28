# nonmake
Building C/C++ projects without a make-system

This nonmake project is meant to show how to build small to medium sized C/C++ projects without a make-system. Currently focused on a Windows environment, but Microsoft is spending a lot of energy to chase away its users, so linux will follow naturally at some time.

With a make-system is meant any combination of low level make or meta make-system, like make, nmake, cmake, ninja, bazel, etc., or better, a bloated or extremely complicated syntax ridden system that chases away many beginners that like to start software development in C or C++. The nonmake project is meant to show beginners (or even advanced developers) how they can start creating their own C-projects without these mostly bloated systems, by using easy commandline commands or a simple batch- or shell-script. Example batch-files are given to explain the methods how to develop in C/C++ without a make-system, also examples are given to build several projects found on github with just a batch-script.

# Make systems

Although there are a lot of use-cases where a kind of complicated make- or build-system is absolutely necessary, nowadays where the hardware and storage devices are many hundreds of times faster then the hard-disks of 30+ years ago, the need for a complicated make-system is more or less totally useless and only adding bloat in a small to medium sized project. Even for very large projects, it is not always needed to make use of a make-system. Large systems can also be set up with libraries and or groups of object-files such that only a part of the project needs to be rebuild for certain purposes.

The famous make was introduced a long time ago mainly for handling dependencies between source-files and was very helpfull in that era, because rebuilding every source-file was very time-consuming and only building the files that needed to be build was helping a lot to keep buildtimes acceptably low. Nowadays where a typical c- or h-file can be read in within a small fraction of a second, the need for a make-system is more or less gone for small to medium sized projects. To keep the makefile for the make system useful, in the past it was also necessary to maintain the dependencies manually, so if a function was moved from one c-module to another and also h-files needed to be updated, one also had to update the dependencies in the makefile, and of course, because not doing this, didn't directly lead to problems, resulted in many hard to diagnose build-problems later on. And the bigger the project became, and the make-system was becoming more and more usefull, maintaining these dependencies manually was almost impossible ...

There are many small C/C++ projects on github with only 10-20 source files, and have more files and/or directories just for supporting multiple build-systems and hard to read text (and mostly incorrect or not-working) documentation to explain how to build the project and most of the time only one environment is explained correctly where the documentation is matching reality.

Cmake for example is an extremely bloated system, that can generate files with thousands of hard to read "cmake code" and that tries to support any variations of installed packages and versions of dependencies and of course there is a percentage that is not supported (yet) and if one encounters that problem, it is up to you to debug and find the problem in the make system.

# Rant about make-systems

A rant about make-systems only has value if the disadvantages can be illustrated by clear examples where a make-system is only hurting a project and not helping in any way. Following is a long rant about problems encountered with make-systems of different github projects.

# LibZip example github project

The following example is meant to illustrate the problems with make-systems and is not meant to criticize the projects itself, in this case libzip and zlib. Thousands of other github projects could have been used to visualize the modern problems with make-systems.

Suppose one is looking for a zip-library that can be used in a project and finds a project that seemingly does exactly what one was looking for, the libzip library:

https://github.com/nih-at/libzip

If one downloads the libzip-main.zip file (The whole essence of this nonmake project is to show how to build a c-project without all kind of dependencies and installed packages, so this also explains the steps without even having git installed) and extract the files in a directory. The INSTALL.md mentions that the project needs to be build using cmake, it also mentions 10(!) other mandatory and optional packages (11 with cmake included) to be installed in order to build or use the library. So if one tries to do that on a Windows 10 system with Visual Studio and cmake installed, your environment and results may vary, it stops (after more than 3 minutes(!) of strange c-function checking, with the following error-message:

Could NOT find ZLIB (missing: ZLIB_LIBRARY ZLIB_INCLUDE_DIR)  (Required is at least version "1.1.2")

This already raises an important question; Why does a zip-library need another zip-library? Which library does what? I can imagine that one library does the actual zipping part and the other the file meta data part, but why is that not described anywhere, why do I have to make that up myself and still not knowing if my assumptions are correct? Also the INSTALL.md mentions that zlib comes with most operating systems. Apparently Windows 10 does not fall into that "most" category? And this is the first and only mandatory package/library that is needed for libzip. That does not give much confidence for the 9 other optional packages. So we have to install zlib first, but from the error-message and the use of cmake that hides all the intelligence, it is absolutely not clear **how** to install zlib, such that cmake finds it correctly. The error-message for example seems to point to the environment variables ZLIB_LIBRARY and ZLIB_INCLUDE_DIR that need to be set correctly. The link in the INSTALL.md file points to the following site:

https://www.zlib.net

However, that site does not show any installer to install, only 3 compressed files with source-code and a cryptictally described zlib.tar.gz file, named "Permalink for the most recent release:", so installing yet another application (for example 7zip) is mandatory, just to check what is contained in that file ... and appears to be an Install-on-Demand archive ... after installing that package, of course did lead to the same error for the missing ZLIB library. So, ..., where to go from this?; try to set the environment variables, try to read and understand the 500 lines of the CMakeLists.txt file, or try to find the problem in the 43 files and 14 directories that cmake generated, or try to find the problem in the CMakeCache.txt file with 750 unreadable lines of cmake code? Or ... try to take the easy route and try to build the zlib library from source? The latter it is ...

After downloading the zlib source code I discovered that my ExtractAll option in Windows to extract a zip-file was replaced by Express Zip from NCH?!@#@? And trying to unzip the zlib131.zip took ages (several minutes). Considering the fact that the Express Zip replaces a well working unzip option in Windows with an unworkable slow application can be considered malware. So how can this rabbit hole just trying to build the libzip library get worse?

The zlib project contains 253 files and 36 folders! and only 15(!) c-files to build the library!! So many more make-system related files than actual source-files! Running cmake in a build folder results in 53 extra files in 15 directories and not even building the library! But because it creates a zconf.h file needed for the c-files to be build (and is not available earlier), it creates the zconf.h file such that all c- and h-files are present to be able to build the library with just the following commands:

cl -c *.c\
lib *.obj /OUT:zlib.lib

That is it, these two commands replaces **ALL** the cmake bloat, all the MB's of make-system bloat and ALL the unnecessary directories and other make-files that only add confusion and absolutely add **nothing** usefull, at least if one only wants to build the library in a single environment! But for linux this would also be a few simple commands. So in essence the zlib project can be condensed from a 4.36 MB 255 files in 36 directories project (4.85 MB, 307 files and 52 directories after cmake run), into a 2.31 MB, 52 files and 3 folder project and a 2-line batch-script to build the library without make-system bloat within a second. It takes much more time to remove all the make-system bloat than just building the library with these 2 simple commands! I do not think the zlib project can be blamed for this, because it probably originated from an era where makefiles were very important, but why not add this easy script-solution that can have anyone building the library within a minute after downloading the project, without even having to install cmake?

But, the quest continues, because we still do not know how to communicate to cmake within the libzip project where the zlib library is. At no point in the documentation it is described how to incorporate the zlib library in the libzip project. So one has to guess or dive into the cmake bloat files. To save time, the first option is tried; copy the zlib.lib and zlib.h in the libzip folder and set the ZLIB_LIBRARY and ZLIB_INCLUDE_LIB environment variables ... It still gives the ZLIB errors, but now also fails on the other "optional" packages.

Could NOT find PkgConfig (missing: PKG_CONFIG_EXECUTABLE)\
Could NOT find Nettle (missing: Nettle_LIBRARY Nettle_INCLUDE_DIR) (Required is at least version "3.0")\
Could NOT find GnuTLS (miising: GNUTLS_LIBRARY GNUTLS_INCLUDE_DIR)\
Could NOT find MbedTLS (missing: MbedTLS_LIBRARY MbedTLS_INCLUDE_DIR) (Required is at least version "1.0")\
Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the system variable OPENSSL_ROOT_DIR (missing: OPENSSL_CRYPTO_LIBRARY OPENSSL_INCLUDE_DIR)\
Could NOT find ZLIB (missing: ZLIB_LIBRARY ZLIB_INCLUDE_DIR)  (Required is at least version "1.1.2")\

SO in trying to solve the ZLIB issue, it creates 5 other issues, for packages probably not even needed (but that is not clear from the documentation)! One could say that these kind of issues are not make-system related issues, but they are, because cmake is the failing system in this and not working correctly, or at least not giving any indication how to solve the problem. And not forgetting that these are just two very simple libraries! Without cmake, no more than 10 lines of "build-code" should have been present to investigate, but in this case one has to dive into cmake files and investigate at least 1250 lines of unreadable cmake-code in order to solve this "simple" problem ... Remember the goal of any cmake system is to help and not to make something very simple extremely complex.

Another "solution" (=workaround) is just to locate all the relvant c- and h-files and just build those and ignore all other make-system files (a method that works for many small projects!). But for a library this method is more difficult than for an executable, because even if one is able to create a library from all obj-files, it does not mean all functionality is present. The other problem is to locate the correct c- and h-files, if cmake worked, we did not have to be bothered by this, but because cmake fails, we have to be bothered ... and are hampered by the fact that the libzip project is assumed to be working with cmake and no real logic or design seems to be present to make clear what the essential c- and h-files are. In order to find the correct c- and h-files we see in line 487 the inclusion of the following cmake include file:

export(TARGETS zip\
  FILE "${PROJECT_BINARY_DIR}/${PROJECT_NAME}-targets.cmake)

The PROJECT_NAME is not set anywhere in the CMakeLists.txt file, so it is not clear what value it has and not to mention PROJECT_BINARY_DIR that is also not set in the cmake file ... sigh and double sigh ... and no *.cmake files are found in the libzip directory ... Normally I would have stopped when reading about the 10-11 optional packages the library depends on, but in this case I want to illustrate the actual problems one encounters, just trying to use a "simple" library. There are probably an endless number of cmake-aware people who know a good solution for every problem that I encountered, but that is the issue, why do I need these cmake-experts to just build a simple library? If I would know the essential c- and h-fiies to build the library, I would be able to build it within a minute after downloading the project, it is the cmake environment that obscures this and makes it unnecessarily complex.

We could assume that the src directory contains all the critcal c- and h-files, but after looking in the lib-directory it seems that that directory contains all essential c- and h-files. The README.md file only adds to the confusion, because it mentions that the src and examples directories contain the examples, but no mention about the lib-directory. We have to assume that all lib c- and h-files are in the lib directory and try to build them to a library with the following commands:

cl -c lib\\*.c\
lib *.obj /OUT:libzip.lib

But of course that also fails, in this case because the cmake command was not completed and the config.h file was not created correctly (also a task that cmake does). The configure nonsense is a linux feature, and even a much much more complex system then any make-system combined. It was the main reason for me not to switch to linux 20+ years ago and it is extremely irritating to be bothered by it 20 years later on a windows system. Luckily for a windows system it is relatively easy and just renaming the config.h.in to config.h is in most cases sufficient, but still a nasty burden, because it is not always that easy. And of course libzip is one of those cases and the h-files are littered with cmake nonsense. So now we have to debug/fix the config.h and zipconf.h files.

After fixing the config.h and zipconf.h files such that they compile, it now fails on the compat.h file:

compat.h(147): warning C4067: unexpected tokens following preprocessor directive - expected a newline

At this point a normal person would reject the project altogether and start doing more usefull things in their life. As already stated this is not a criticism of the libzip project itself, the problems can be a result of many things, for example Windows not supporting ZLIB, or the installation of zlib library having changed, or due to cl or cmake changes, etc.. but this is a problem with every dependency and a normal developer should stop after seeing more than 2 dependencies in a project, especially if no potential problems with the dependencies are explained. But in this case we still continue ...

The compat.h fails on the line with the contents "#if SIZEOF_OFF_T == 8". And it is absolutely not clear where it actually fails on. Trying different std:c++ versions did not result in any solution. In the end the #ifdef switch was commented out to let it compile further. But too many compilation errors occurred with the other modules and many of the failures seems to be related to the fact that some c-files are dependent on the optional packages. This switching was normally done by the cmake-system, so I had to go back to get cmake working, such that it would be clear whcich c-files are not dependent on the optional packages, such that only the ZLIB problem remained.

However, during this attempt to get all the c-files building, I discovered that the zlib.h (matching the zlib.lib library) needs to include the zconf.h file, but that file is nowhere to be found in the zlib project?!? Where is the compiler able to find that file even if we use the cmake system? Trying to disable the inclusion of the zconf.h file, also led to problems, first of all because it defines the ZEXTERN macro, but that clearly would be extern of empty, so that was easy to fix, but of course not the other many build errors ...

Time to give up ... Although there is another last possibility, but very manually intensive and that is trying to get it building under linux, because I suspect that both libraries are mainly geared for that platform, and after that, remove all the cmake and other nonsense and try to get the same projects building under Windows ... It are just c-files, so how hard could that be ...?

It seems that the linux version worked better, but after calling cmake it created the Makefile correctly, but calling make resulted in a critical error at the 46% level of the build.

CMake Error: cmake_symlink_library: System Error: Operation not permitted\
CMake Error: cmake_symlink_library: System Error: Operation not permitted\
lib/CMakeFiles/zip.dir/build.make:3013: recipe for target 'lib/libzip.so.5.5' failed

Yes, cmake generates many make-files and some having literally thousands lines of code, and in this case the build.make file fails on line 3013 on a the following make command:

@$(CMAKE_COMMAND) -E cmake_echo_color --switch=$(COLOR) --green --bold --progress-dir="/media/user/FAT32 DATA/linux/libzip/libzip-1.11.3/build/CMakeFiles" --progress-num=$(CMAKE_PROGRESS_115) "Linking C shared library libzip.so"

How to handle this error? Why do I see color settings in a cmake make-file for a zip-library? And what is it actually failing on? I do not know any cmake syntax, so how can we get any further from this cryptic error message? I have no clue where to look for a solution for this problem and forced to give up ... I expected to use this libzip project to point to the complexity of the cmake system and how it could have been done much easier with a simple batch-script, but did not expect these many problems, and not even being able to build anything in a Windows and even linux environment! So unfortunately we have to use a different project to illustrate the unnecessary complexity of modern build- or make-systems. I deliberately did not want to use a known project that I already converted to use a batch-script and worked, but an unknown project and describe the method and to remove the make-system from it ...

A diff-tool is an essential tool for every software developer, so that will be our next project to illustrate the make-system related problems ...

# Winmerge example github project

The following github project is downloaded:

https://github.com/WinMerge/winmerge

And that is a much larger project than the libzip project, it contains 6274(!) files in 558(!) directories and looks to be based on vc-projects and of course sln-files for every version of Visual Studio (that is another problem in itself). Having experience with github projects that only have vc-project files, almost never work and also for this project this was the case. The first build failed on the missing afxres.h and afxwin.h files. And also with the message that the MFC libraries needed to be installed, ... so that was a no-go area and not even worth pursuing ... not even as an example for the make-system problems ... So another github project must be considered, what about a screen-capture tool?

# Ksnip example github project

This is a much smaller project but still has 639 files in 106 directories and is based on cmake, so another good example for the problems with cmake.

https://github.com/ksnip/ksnip

Running cmake already points out a serious problem with using cmake and maintaining long term projects, the following warning was shown:

Compatibility with CMake < 3.10 will be removed from a future version of CMake.

So, ... that means if one has a project and want to maintain it for a longer period of time, one has to keep different versions of CMake working in your development environment. I did not even know about this backwards incompatibility and is even more of a reason never to build projects based on cmake.

But of course, the version warning was not the only problem, cmake failed on the fact that Qt was not installed. Reading the README.md did mention that ksnip is based on Qt, but no instructions how to install it, but in this case the cmake output showed a lot of information about linking the Qt package (but do not know yet wether it is usefull). It also mentions that the packages kImageAnnotator & kColorPicker need to be installed. First Qt must be installed (the writer of the README.md probably expects that everyone else already has this installed?). This also illustrates all the problems with building all kind of dependencies in a github project. Just trying out 3 projects, needed me to install; cmake, MFC libraries, zlib, Qt, etc..

I had to assume that the Qt SDK needed to be installed, but trying to do that showed that one has to download a free trial version, that will only be working for 10 days! That is fair for a company to do, but is weird if an open source project is based on commercial software that needs to be bought and no mention of that in the ksnip README.md file ... So this project must be abandoned also ... The ksnip project also mentioned that is uses tesseract as an OCR system, that is a good next example to try ...

# Tesseract example github project

This project is also based on cmake and "only" consists of 726 files in 47 directories.

https://github.com/tesseract-ocr/tesseract

The first cmake run of course failed, because it depends on a non-installed package. In general this should not be the biggest problem, but in this case it failed on a SWConfig.cmake error and it is not clear what this SW-package is. The README.md also mentions dependencies on Leptonica that in its turn depends on zlib (!), png and tiff packages. The leptonica project only seems to be available as source-code, so this is yet another project that needs to be build first.

The leptonica project seems to be a bigger project than the tesseract project itself and is also based on cmake, but there should be a project on github that builds out of the box, so fingers crossed. As expected, the cmake run failed on a dependency and again failed on this mysterious SW-package. Looking into the README.md file gives more information, the SW client software needs to be downloaded from a site that lists an unusual large number of packages and executables to download, chose one zip-file that looked appropriate, but that only contained one large sw.exe executable of 63 MB! Even if I am working on a virtual machine, running a vague executable without any description of what it is or does, is a definite no-go. There was a sw.pdf file in the repository, but the first sentence contains the words "incomplet and incorrekt". Also the introduction was very unclear of what it actually was, it is a kind of software management system, dedicated to better software management ...?!? So distributing a single executable and on the side with incomplet and incorrekt documentation is a way for better software management?? Further reading doesn't make it much clearer and saw that it is a kind of package management system or something like that and also related to cmake config issues. No thanks ... On to the next project .. let's try another project, a plot tool.

# Matplot++ example github project

This project is a visualisation library for making graphs of most diverse data. It is a relatively large project with 1911(!) files in 267 directories. How much plotting can you do with 1900+ files?

https://github.com/alandefreitas/matplotplusplus

The cmake run worked first time! And although the build took 30 minutes, the build was also successfull! We have a winner, however, the size of the project is probably not a good example, because for this project it pobably is usefull to use a make-system (in this case via Visual Studio). Also after the build, the project directory suddenly contains 12997 files in 2412 directories! But 10661 files are part of the examples directory, so should not important for building the library itself. However, it was too good to be true, after removing most of the examples from the project a rebuild only took 5 min, but trying to run an example, it became clear that gnuplot was needed to run the example .. Scanning the extremely large README.md file, it mentioned somewhere the text:

Install Gnuplot 5.2.6+ (Required at runtime)

The gnuplot is probably much larger than matplot++, so this quest also ends here ... Not mentioning this runtime dependency somewhere in the README.md in a more accessible location, raises the question what else is needed to get it running.

The make-system rant journey will now only focus on very small projects ... that have no dependencies at all ... One or two dependencies is acceptable if one really wants the project and use it in some form, but for illustrating the problems with make-systems and how it can be done without a make-system, any dependency is killing. Lets try another compression library.

# Lzham codec example github project

This compression project can be considered a small project with just 89 files in 10 directories and based on cmake.

https://github.com/richgel999/lzham_codec

Apart from the cmake version warnings and another projectname warning, cmake runs OK when first running it. Cmake created 99 extra files in 31 directories for just this small project, but is has no dependencies, so seems to be a perfect example. Apart from some build-warnings the project was build with Visual Studio in ca. 31 sec (including examples). The resulting directory exploded to a whopping 146 MB(!) 362 files in 77 directories, but this due to the fact that default the debug option was set and the resulting pdb and ilk files are normally very large. Nowadys 146 MB is not that large, but cosidering one can have a lot of small C/C++ projects in its development storage, backing up these bloated directories can be a heavy burden. Especially for developers who do not use debuggers, the debug build is an unecessary burden also. Changing the project to only perform a release build also does not remove the debug output files and only adds more files and directories. So now the directory has a size of 228(!) MB and 527 files in 95 directories. Note, we are still talking about a small project, so this is ridiculous!

Now for the fun part, removing all the make-system related bloat and illustrate how this project can be converted to a true small make-system free project. From the start a release rebuild takes around 1 min (on a slow virtual machine). Building the release build without any changes takes around 25 sec to 60 sec. I have no clue why this is rebuilding source-files again when there are no changes, but I also do not care and do not want to investigate that problem. The total project at this point contains the following files/directories:

bin                            - Contains the executables that were built\
build                          - Directory made for cmake to put all cmake generates files in\
example1                       - Example 1 source code\
example2                       - Example 2 source code\
example3                       - Example 3 source code\
example4                       - Example 4 source code\
include                        - Include directory with the h-files for the library\
lib                            - Generated library and dll files\
lzhamcomp                      - ???\
lzhamdecomp                    - ???\
lzhamdll                       - Source code for generating dll file(s)\
lzhamlib                       - Source code for lzham library\
lzhamtest                      - Command line test program source code\
CMakeLists.txt                 - Main cmake file\
LICENSE                        - License file\
lzham.sln                      - Visual studio solution file\
README.md                      - Readme file\

Just to test, the example1 executable can ge gemerated with the following command in the example1 directory:

cl example1.c /MD -I..\include ..\lib\x64\lzham_64.lib

And generates example1.exe with a size of 13 kB i.s.o. the size of 286 kB for the release build and 1.6 MB(!) for the debug build! Building the test program in lzhamtest however failed on a missing header file. But after some investigation it was clear the following command is needed:

cl lzhamtest.cpp timer.cpp /MD /EHsc -DWIN32 -I..\include ..\lib\x64\lzham_x64.lib

So building the executables is a breeze without any make-system whatsoever! The library is a bit harder, because the project supports a static library, a dynamic dll library and a static library around the dll and this is not clearly documented how this is all connected together. This information must be extracted from the CMakeLists.txt files ... or just by trial and error. Preferably we want to build it all in one executable without using a static or dynamic library, but then one must determine what the source-files are to enable this. The lzhamtest program is a commandline test program that enables compressing and decompressing files and can be used as the start for any project that uses the lzham compression "library". This process to determine what are the essential source-files for the static lib and dynamic dll libraries was not necessary if it was explained clearly or if cmake did not obscure all this info in the cmake-files.

After some trial and error and investigating the source-code, it was possible to create the lzhamtest.exe executable with the following command (without creating or using a static or dynamic library):

cl lzhamtest.cpp timer.cpp ..\src\*.cpp /MD /EHsc -DWIN32 -I..\include

But the executable was increased to 320 kB, but this also includes the code for the lib or dll, so no seperate *.lib or *.dll needed. At this point we can clean up all the bloated make-system files (we already copied the cpp-source files in a src directory). The examples also do not seem to be adding much and two of them are more autotesters than examples, so we remove them also. After the first cleanup we have the following (further cleanups can even make it more compact and smaller):

include                        - Include directory with the h-files for the lzham functionality\
src                            - LZHAM Source files\
LICENSE                        - License file\
README.md                      - Readme file\
build.bat                      - Windows build batch script\
lzham.cpp                      - Original lzhamtest.cpp source file\

That is it! The whole repository is now < 1 MB(!) with 55 files in 2 directories. No cmake bloat, no VC/VS bloat, just a simple script containing the following lines of code:

@ECHO OFF\
cl lzham.cpp src\*.cpp /MD /EHsc -DWIN32 -Iinclude\
del *.obj\

3 lines replacing hundreds of mega-bytes and uncountable lines of a cmake code and with a build-time of around 10 seconds (building every source-code is 2x faster than an empty build with Visual Studio)!! For linux another small sh-shell file can be created. The question then arises, why use any bloated build-system for such a small project at all? The only precondition is having the Microsoft CL compiler installed and that can be done by installing the full Visual Studio with C/C++ packages, but if only the commandline versions are needed one can download and install the Visual Studio Buildtools. Microsoft is well known for changing URLs and names at will at random times, so giving links here is useless ...
