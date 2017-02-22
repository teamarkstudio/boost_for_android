This is a simple tutorial for building boost on android platform. if you don`t like pre-build binaries, do following steps:

1、prepare environment, make sure cygwin is installed if you work on windows.
2、execute ./bootstrap.sh to generate compile tools.
3、edit project-config.jam or user-config.jam, add following lines, you can change building libs or compiler flags as your wish,

===> copy start

# define platform name of ndk
import os ;
if [ os.name ] = CYGWIN || [ os.name ] = NT 
{  
    androidPlatform = windows-x86_64 ;  
}  
else if [ os.name ] = LINUX 
{  
    androidPlatform = linux-x86_64 ;  
}  
else if [ os.name ] = MACOSX 
{  
    androidPlatform = darwin-x86 ;  
}

# replace with your own path
ANDROID_NDK = "D:/NVPACK/android-ndk-r12b" ;

# compile with gcc, you can change compiler to clang or others
using gcc : android4.9 : $(ANDROID_NDK)/toolchains/arm-linux-androideabi-4.9/prebuilt/$(androidPlatform)/bin/arm-linux-androideabi-g++ :  
<archiver>$(ANDROID_NDK)/toolchains/arm-linux-androideabi-4.9/prebuilt/$(androidPlatform)/bin/arm-linux-androideabi-ar  
<ranlib>$(ANDROID_NDK)/toolchains/arm-linux-androideabi-4.9/prebuilt/$(androidPlatform)/bin/arm-linux-androideabi-ranlib  
<compileflags>-I$(ANDROID_NDK)/platforms/android-19/arch-arm/usr/include 
<compileflags>-I$(ANDROID_NDK)/sources/cxx-stl/gnu-libstdc++/4.9/include
<compileflags>-I$(ANDROID_NDK)/sources/cxx-stl/gnu-libstdc++/4.9/include/backward
<compileflags>-I$(ANDROID_NDK)/sources/cxx-stl/gnu-libstdc++/4.9/libs/armeabi-v7a/include  
<compileflags>-fexceptions
<compileflags>-frtti
<compileflags>-fpic
<compileflags>-ffunction-sections
<compileflags>-funwind-tables
<compileflags>-D__ARM_ARCH_5__
<compileflags>-D__ARM_ARCH_5T__
<compileflags>-D__ARM_ARCH_5E__
<compileflags>-D__ARM_ARCH_5TE__
<compileflags>-D__ARM_ARCH_7__
<compileflags>-D__ARM_ARCH_7A__
<compileflags>-Wno-psabi
<compileflags>-march=armv7-a
<compileflags>-mtune=xscale
<compileflags>-mfloat-abi=softfp
<compileflags>-marm
<compileflags>-mthumb
<compileflags>-Os
<compileflags>-std=gnu++11
<compileflags>-fomit-frame-pointer
<compileflags>-fno-strict-aliasing
<compileflags>-finline-limit=64
<compileflags>-Wa,--noexecstack
<compileflags>-DANDROID
<compileflags>-D__ANDROID__
<compileflags>-D__ARM_EABI__
<compileflags>-DNDEBUG
<compileflags>-O2
<compileflags>-g
;  

# project default compiler
project : default-build <toolset>gcc-android4.9 ;

# replace with libraries you wanna to build
libraries = --with-container --with-coroutine --with-coroutine2 --with-fiber --with-graph --with-graph_parallel --with-log --with-metaparse --with-python --with-test --with-type_erasure --with-atomic --with-date_time --with-program_options --with-chrono --with-context --with-iostreams --with-locale --with-mpi --with-serialization --with-signals --with-timer --with-wave --with-math --with-random --with-exception --with-filesystem --with-thread --with-system --with-regex --with-program_options ;

===> copy end

4、execute ./b2 toolset=gcc-android4.9 link=static threading=multi target-os=linux --stagedir=android, or replace args for your own.