# librealsense

A cross-platform library for capturing data with the RealSense F200 (IVCAM 1.0, 1.5) and RealSense R200 (DS4). This effort is aimed at supporting prototyping efforts on new platforms and form-factors (robots, drones, VR, etc). This repository hosts both the source-code and binary releases.

Dependency management for GLFW3 and libusb-1.0 is done manually at the moment (see the corresponding sections below), pending the creation of installer scripts to automate the process. 

## Supported Devices

1. RealSense R200 (DS4)
2. RealSense F200 (IVCAM 1.0)
	
DS5 and IVCAM 1.5 support will be provided in the near future. librealsense should in principal be able to support any 3D camera which exposes a UVC interface and which chiefly relies on UVC XU commands and USB sidechannel traffic for controls and calibration information.

## Supported Platforms

librealsense is written in standards-conforming C++11 and relies only on the C89 ABI for its public interface. It was developed and tested on the following platforms:

1. Windows 8 (Visual C++ 2013)
2. Ubuntu 14.04 LTS (gcc toolchain)
3. Mac OS X 10.7+ (clang toolchain)

Android support will be provided in the near future. librealsense is likely to compile and run on other platforms as well, and we appreciate any assistance in testing and debugging support for other platforms and operating system versions.

## Supported Languages and Frameworks

1. C - Core library API exposed via the C89 ABI
2. C++ - Single header file (rs.hpp) wrapper around C API, providing classes and exceptions
3. C# - Single source file (rs.cs) wrapper for P/Invoke to C API, providing classes and exceptions

Our intent is to provide bindings and wrappers for as many languages and frameworks as possible. Our core library exposes its functionality via C, and we intend for our various bindings and wrappers to look and feel "native" to their respective languages, for instance, using classes to represent objects, throwing exceptions to report errors, and following established naming conventions and casing styles of the language or framework in question.

## Library Dependencies

* Windows:
  * Windows SDK (WinUSB + Media Foundation)
* Linux, OS X, Android:
  * libusb-1.0
* Example Programs:
  * OpenGL 1.1+
  * GLFW 3.0+

## Functionality

The goal of librealsense is to provide a reasonable hardware abstraction with minimal dependencies. It is not a computer vision SDK.

1.	Streams: depth, color, infrared
  *	Of the different streaming modes + fps options available, only fairly common ones have been tested. 
2.	Intrinsic/extrinsic calibration information (inc. uv map, etc)
3.	Majority of XU-exposed functionality for each camera

## Apple OSX Installation

1. Install XCode 6.0+ via the AppStore
2. Install the Homebrew package manager via terminal - [link](http://brew.sh/)
3. Install libusb via brew:
  * `brew install libusb`
4. Install glfw3 via brew:
  * `brew tap homebrew/versions`
  * `brew install versions/glfw3`

## Ubuntu 14.04+ Installation

1. Grant appropriate permissions to detach the kernel UVC driver when a device is plugged in:
  * `sudo cp config/99-uvc.rules /etc/udev/rules.d/`
  * `sudo cp config/uvc.conf /etc/modprobe.d/uvc.conf/`
  * Either reboot or run `udevadm control --reload-rules && udevadm trigger` to enforce the new udev rules
2. Ensure apt-get is up to date
  * `sudo apt-get update && apt-get upgrade`
3. Install libusb-1.0 via apt-get
  * `sudo apt-get install libusb-1.0-0-dev`
4. glfw3 is not available in apt-get on Ubuntu 14.04. Use included installer script:
  * `scripts/install_glfw3.sh`
5. We use QtCreator as an IDE for Linux development on Ubuntu: 
  * `sudo apt-get install qtcreator`
  * `sudo scripts/install_qt.sh` (we also need qmake from the full qt5 distribution)
  * `all.pro` contains librealsense and all example applications
  * Clean => Run Qmake => Build
6. Don't want to use QtCreator? We have a makefile!
  * `make && sudo make install`
  * The example executables will build into `./bin`

## FAQ

*Q:* How is this implemented?

*A:* The library communicates with RealSense devices via the UVC and USB protocols, using Media Foundation / WinUSB on Windows and libuvc / libusb on all other platforms. It does not link against DSAPI or IVCAM-DLL. 

*Q:* Is it maintained or supported?

*A:* It is supported in the sense that bugs will be fixed if they are found and new features will be periodically added. It is not intended to replace functionality developed by other teams to support HVM (firmware updates, etc), nor materially impact SSG’s roadmap for the SDK/DCM – it is independent and outside of these major efforts. 

## License

For internal distribution only. 

("Material") are owned by Intel Corporation or its suppliers or licensors. Title to the
Material remains with Intel Corporation or its suppliers and licensors. The Material may
contain trade secrets and proprietary and confidential information of Intel Corporation
and its suppliers and licensors, and is protected by worldwide copyright and trade secret
laws and treaty provisions. No part of the Material may be used, copied, reproduced,
modified, published, uploaded, posted, transmitted, distributed, or disclosed in any way
without Intel’s prior express written permission.

No license under any patent, copyright, trade secret or other intellectual property right
is granted to or conferred upon you by disclosure or delivery of the Materials, either
expressly, by implication, inducement, estoppel or otherwise. Any license under such
intellectual property rights must be express and approved by Intel in writing.

Copyright © 2015 Intel Corporation. All rights reserved.
