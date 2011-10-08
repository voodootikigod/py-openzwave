py-openzwave
============

What is py-openzwave?
---------------------
py-openzwave is a python wrapper around the open-zwave c++ project.
This allows you to interact with z-wave networks from withing Python.

1. [OpenZWave][ozw] talks to ZWave devices in your house via ZWave PC controller
2. py-openzwave provides python bindings for [OpenZWave][ozw]
3. program your house with Python!

  [ozw]: http://code.google.com/p/open-zwave/

Build instructions for Windows
------------------------------
In order to build py-openzwave on Windows you need MinGW, MinGW is a minimalist GNU compiler for Windows.
You can download MinGW here: http://www.mingw.org/
Other requirements are:
- Cython 14.1 or higher, available from: http://www.cython.org
- Open-zwave revision r321 or higher (r321 contains patches for MinGW)

We will assume the following build structure (different structure requires changes in py-openzwave's setup.py!):

\ Root folder
-\ py-openzwave source folder
-\ open-zwave source folder

1) From the root folder, type "cd open-zwave\cpp\build\windows\mingw32"
2) Enter "make", this will build open-zwave. If all went will you should have an "openzwave.a" file in the "open-zwave\cpp\lib\windows-mingw32" directory.
3) From the root folder, type "cd py-openzwave"
4) Type "python setup.py build --compiler=mingw32", this should build the py-openzwave module. If you are satisfied with the build, you can install it by running "python setup.py install"

Building on Ubuntu
------------------

Download and Build
==================

(starting inside the py-openzwave directory)

    $ cd ..
    $ svn checkout http://open-zwave.googlecode.com/svn/trunk/ open-zwave-read-only
    $ cd py-openzwave
    $ ln -s ../open-zwave-read-only openzwave
    $ patch -d openzwave -p0 < fix_openzwave.patch
    $ (cd openzwave/cpp/build/linux; make)

Then the python library:

    $ python setup.py build

Now run the test program (change `ZWAVE_SERIAL` if necessary):

    $ python -i test.py

-------------------------------------------------------------------------------
Hints
=====

Building on Ubuntu 10.10 - (drewp@bigasterisk.com)
--------------------------------------------------

The 'cython' version for Ubuntu 10.10 is 0.12.1, which is too
old. You'll get an error at `cdef extern from # "<string>"`. Removing
that cython and running 'easy_install cython' will get you a version
at least as new as 0.14.1, which will work.

For the tricklestar USB device, a more robust device name to use is 
`/dev/serial/by-id/usb-Prolific_Technology_Inc._USB-Serial_Controller_D-if00-port0`
(as opposed to `ttyUSB0`, `ttyUSB1`, etc).

I (drewp) still don't know the workflow for adding new devices. For
that I used https://code.google.com/p/openzwave-control-panel/ which
does have an 'add device' operation.

-------------------------------------------------------------------------------
Adding devices
--------------

from the ozcp code, 

          setAdminFunction("Add Device");
          setAdminState(
                   Manager::Get()->BeginControllerCommand(homeId,
                                   Driver::ControllerCommand_AddDevice,
                                   web_controller_update, this, true));
then it waits for
  `case Driver::ControllerState_Completed:`