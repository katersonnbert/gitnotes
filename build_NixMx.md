How to build NIX-MX on Windows
------------------------------

Follow these steps to set up the development environment for the NIX Matlab bindings under Windows32/64 for Matlab 32bit

**Dependencies**
- download and install Visual Studio 12
- make sure you have a fork of [NIX](https://github.com/G-Node/nix) and [NIX-MX](https://github.com/G-Node/nix-mx)

	NOTE: if you are using the virtual box avoid setting the working directory on any network drives, the cmd shell cannot easily handle network drives in a virtual box from a linux
	machine, while powershell can handle network drives but at the moment cannot handle the batch file used to build nix and nix-mx.
- get the latest [NIX Windows dependencies](https://projects.g-node.org/nix/), extract to `[your path]/nix-dep/`

- IMPORTANT: if you need the DEBUG instead of the RELEASE build, make sure you use the corresponding debug folders instead of the release folders and settings in all subsequent paths and scripts of these set up notes:

**Build process for RELEASE build**
- start virtual box
- MatLab 32/64bit, extract/install to `[directory of choice]`
	/* G-Node SPECIFIC!
		use WinSCP, get MATLAB_R2011a_Win32.zip (~676mb) for 32bit Matlab
					get MATLAB_R2011a_Win62.zip (~745mb) for 64bit Matlab
		from gate.g-node.org, `/groups/g-node-code/share/MATLAB/`
	*/
- check MatLab registry keys, if they are not there set up:
	add keys to `HKEY_LOCAL_MACHINE/Software` to create the following structure:
		`HKEY_LOCAL_MACHINE/Software/Mathworks/MATLAB/7.12`
	add string `MATLABROOT = [path to main MatLab directory]`		e.g. `c:/work/MATLAB/R2011a`
- DL latest [nix windows dependencies](https://projects.g-node.org/nix/), extract to `c:/[directory of choice]/nix-dep/`

- start cmd (use `nixenv.bat`) or powershell (use `nixenv.ps1`), move to `[your path]/nix`; create `build` directory, move inside
- if you need any other build than DEBUG (in our case RELEASE) on a 32bit windows, edit `[your path]/nix-dep/nixenv.bat` or `.ps1`:
	edit PLATFORM from x86 to x64 if required
	edit CONFIGURATION from Debug to Release if required
- to set up dependencies path variables for NIX cmake, run:
	`[your path]/nix-dep/nixenv.bat` or `.ps1`
- if working on a 32bit Windows, run:
	`cmake .. -G "Visual Studio 12"`
- if working on a 64bit Windows, run:
	`cmake .. -G "Visual Studio 12 Win64"`
- with Visual Studio open `[your path]/nix/build/nix.sln`

IMPORTANT NOTE! Visual Studio builds by default with configuration "Debug" and "32bit"!
- If some other build is required (in our case we need at least configuration "Release"), set BUILD->ConfigurationManager->Active solution configuration!
- Build "ALL_BUILD"
- within the cmd shell move to `[your path]/nix/build/Release`
- run `[your path]/nix/build/Release/TestRunner.exe`
- within the cmd shell move to `[your path]/nix-mx`, create and move into `build` folder
- set the NIX root path;
	`set NIX_ROOT=[your path]/nix`

	IMPORTANT:
	- do not use quotes in cmd!
	- do not use backslashes, only slashes! e.g. `c:/work/nix`; `c:\work\nix` will not work!

- check, if the `nix.dll` library has been created in `[your path]/nix/build/Release`
- if yes, open `[your path]/nix-mx/cmake/FindNIX.cmake`
- add `HINTS $ENV{NIX_ROOT}/build/Release` to the `find_library(NIX_LIBRARY NAMES` statement
- if working on Windows 32bit, run
	`cmake .. -G "Visual Studio 12"`
- if working on Windows 64bit, run
	`cmake .. -G "Visual Studio 12 Win64"`
- with Visual Studio open `[your path]/nix-mx/build/nix-mx.sln`
- Set BUILD->ConfigurationManager->Active solution configuration to "Release" and the correct bit version!
- Build "ALL_BUILD"
- copy all of the following files into the `[your path]/nix-mx/build` folder; simply providing the directories to MatLab using `addpath` does not work

	from `[your path]/nix-dep/[x86/x64]/hdf5-1.8.14/bin` 				(path dependent on the Windows 32/64 bit version)
		`hdf5.dll, msvcp120.dll, msvcr120.dll, szip.dll, zlib.dll`
	from `[your path]/nix/build/Release`
		`nix.dll`
	from `[your path]/nix-mx/build/Release`
		`nix_mx.mexw32` or `nix_mx.mexw64`
- get some NIX files from the `[your path]/nix/build/` test folder and NIX away!


**Alternate build process for DEBUG build**

The build described above is for RELEASE which requires the usage of msvcp120d.dll and msvcr120d.dll and prevents the usage of msvcp120.dll and msvcr120.dll

To use the hdf5 debug dlls, nix and nix-mx have to be built in DEBUG mode! For this:
- replace "Release" with "Debug" in the `nixenv.bat` or `.ps1`, use this to setup environmental variables
- run `cmake` as described above
- open sln, in Visual Studio set BUILD->ConfigurationManager->Active solution configuration to "Debug" instead of "Release"
- re-run TestRunner.exe in folder "Debug" instead of "Release" (Duh!)
- move to the `[your path]/nix-mx/build` folder
- re-run the modified `nixenv.bat` or `.ps1`
- run
	`$env:NIX_ROOT="c:/work/nix"`
- open `[your path]/nix-mx/cmake/FindNIX.cmake` and add
	`HINTS $ENV{NIX_ROOT}/build/Debug` to the `find_library(NIX_LIBRARY NAMES` statement
- run `cmake` as described above
- open sln, in Visual Studio set BUILD->ConfigurationManager->Active solution configuration to "Debug" instead of "Release"
- setup the rest like in debug mode


**Additional notes**
- If you receive obscure LNK errors (externals cannot be resolved) at any point, re-run NIX-MX cmake.



How to build nix-mx under Linux:
================================

- fetch and build nix:
- cmake ..
- make all
- ctest
- sudo make install

- Move to nix-mx folder
- build folder
- cmake ..
			use "cmake -DDEBUG_GLUE=0 .." to include debug messages in Matlab 
- make

if required, change the following links:
- in /opt/matlab/bin/glnxa64:

	sudo mv libhdf5.so.6 /tmp
	sudo mv libhdf5_hl.so.6 /tmp
	sudo mv libboost_regex.so.1.49.0 /tmp
	sudo ln -s /usr/lib/x86_64-linux-gnu/libhdf5.so libhdf5.so.6
	sudo ln -s /usr/lib/x86_64-linux-gnu/libhdf5_hl.so libhdf5_hl.so.6
	sudo ln -s /usr/lib/x86_64-linux-gnu/libboost_regex.so libboost_regex.so.1.49.0

- in /opt/matlab/sys/os/glnxa64:
	sudo mv libstdc++.so.6* /tmp/
	sudo ln -s /usr/lib/x86_64-linux-gnu/libstdc++.so.6
	sudo ln -s /usr/lib/x86_64-linux-gnu/libstdc++.so.6.0.19


