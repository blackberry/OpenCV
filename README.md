# Blackberry Port of OpenCV 2.4.1

Officially sanctioned port of OpenCV to the Blackberry/QNX platform. To avoid fragmentation, please do not use any other version of OpenCV.

OpenCV has a CMake-based build system. This port was based on the sources for the Unix variant downloaded from http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.1/.

### What's working, what's not?

This is an untested compilation of the OpenCV libraries for PlayBook.  Areas that are highly dependent on the support of the operating system, such as the window system and (video) camera input facilities, have not been modified and will not work as-is.
Basic data types (points, matrices, rectangles, etc..), colour space transformations and the like appear to work, as does the 
source code that provides mean shift based tracking (meanshift and CAMShift), optical flow (good features to track, 
LK pyramids), histograms and back projection.  The machine learning components needed for ADABoost and Haar Training also seem to work, as do
Kalman filters.  Other parts of OpenCV have not been looked at in any way.

### Prerequisites

- Blackberry Native SDK (NDK) for Tablet OS

### Instructions for cross-compiling on Linux

1.  If not already installed, obtain a native development kit for the BlackBerry PlayBook, and install it.
    We'll use NDK as the name of the directory where the native development kit is installed.
2.  Open a new command line shell, and invoke the script that initializes critical environment variables.  In a BASH shell
    you would type:
        > source $NDK/bbndk-env.sh
3.  Create a sub-directory called "build".
4.  Change to the "build" directory, then invoke the configuration script to run CMake for the ARM architecture:
        > ../configure-qsk arm a9

    The output will report that, for example certain GUI features have been enabled.  Other 3rd party libraries are 
    already disabled by default.  Also, this will use the GNU STL implementation rather than the default "Dinkum"
    implementation.
5.  Modify the build options: run the CMake configuration GUI to modify any other compile options that should be 
    disabled at the moment:
        > ccmake .

    A "curses"-based GUI will appear, and you can ensure that many options have been turned off. Make the following changes: 
    - Disable BUILD_DOCS, BUILD_NEW_PYTHON_SUPPORT, BUILD_PACKAGE, BUILD_TESTS 
    - Disable all WITH_something options (like WITH_1394, WITH_CUDA, WITH_EIGEN, WITH_FFMPEG, ... etc..)
   The arrow keys take you between options.  The "enter" key toggles between "ON" and "OFF"

   After all the changes have been made, press "c" to modify the configuration.  One error message may appear with respect
   to "sphinx".  It is safe to ignore it. Press "g" to generate and exit.

6.  Compile the libraries using "make".  If it makes sense, use "make -j" to accelerate the compile.
        > make -j4
    Both static and dynamic libraries will be built.  They can be found in build/lib.
