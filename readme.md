## Table of Contents
1. [Build on Windows](#build-on-windows)
2. [Install DigiByte](#install-digibyte)
3. [Documentation](#Documentation)
4. [Other Notes](#other-notes)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Build on Windows

This fork builds a Windows version with Visual Studio and MSVC in the main branch, with upstream tracking in the 'upstream-master' branch.

Note: If you want to skip the build you can download the DigiAsset for Windows self extracting exe, and extract to c:\digiasset_core_windows or whatever path you chose.
You will still need to install the IPFS Desktop and DigiByte Core wallet with the changes from below. You can run DigiAsset_Core.exe from a cmd prompt, as well as DigiAsset_core-web.exe from another cmd prompt.

### Prerequisites

This project uses CMake for the build system. Since we're focusing on Visual Studio:

1. Install CMake support through the Visual Studio Installer (select "Desktop development with C++" and ensure "C++ CMake tools for Windows" is checked)
2. Once installed, open a command prompt with "Tools > Command Line > Developer Command Prompt"
3. You can run `cmake` commands from within this Developer Command Prompt

### Clone the Repository

If you just want to build binaries, use the Clone a repository option and use the following path.

```cmd
https://github.com/chopperbrian/DigiAsset_Core_Windows.git
```

### Set Up VCPKG

Run the following command in cmd.exe:

```cmd
cd vcpkg
.\bootstrap-vcpkg.bat
```

### Choose Build Configuration

Before proceeding with the library builds, you must decide whether you want to create a "Debug" or a "Release" build.

*   **Debug:** Includes debugging information, useful for development and troubleshooting. Binaries are typically larger and may run slower.
*   **Release:** Optimized for performance and size, suitable for deployment.

**Crucially, you must select *one* configuration (either "Debug" or "Release") in Visual Studio for the first library (JsonCpp) and use that *exact same* configuration for all subsequent library builds (LibJson-RPC and DigiAsset Core). Mixing configurations will lead to build failures.**

### Install Dependencies

At the top-level directory, execute. Note: This will take some time. Depending on the hardware this could take over 2 hours to finish.

```cmd
.\install-dependencies.bat
```

### Build The Binaries 

#### Build JsonCpp Library

Execute:

```cmd
.\config-jsoncpp.bat
```

Now open the jsoncpp.sln file inside the `jsoncpp\build` directory with Visual Studio. Select your chosen build configuration ("Debug" or "Release") from the dropdown in the toolbar. From "Solution Explorer", right click on `ALL_BUILD` and select "Build". Do the same thing for `INSTALL` in order to install the library.

#### Build LibJson-RPC Library

Execute:

```cmd
.\config-libjson-rpc.bat
```

Like with JsonCpp, open the libjson-rpc-cpp.sln file inside the `libjson-rpc-cpp\build` directory. Ensure the **same** build configuration ("Debug" or "Release") you chose previously is selected. Build the `ALL_BUILD` and `INSTALL` options in "Solution Explorer".

#### Build DigiAsset Core

Execute:

```cmd
.\config.bat
```

Then open the solution file in Visual Studio, select the **same** build configuration ("Debug" or "Release") as in the previous steps, and run "Build" on `ALL_BUILD`. The `digiasset_core.exe` binary can be found inside the `DigiAsset_Core\build\src\Debug` or `DigiAsset_Core\build\src\Release` directory, depending on your chosen configuration. It should be launched from there since there are some DLL dependencies in that location.


## Install DigiByte

Download and install the latest verison of the DigiByte Core Wallet. https://github.com/DigiByte-Core/digibyte/releases/download/v8.22.2/digibyte-8.22.2-win64-setup.exe
Install to the default locations, unless you need to change the location on your hard drive. Then add the following lines to the digibyte.conf file.

```
rpcuser=user
rpcpassword=pass11
rpcbind=127.0.0.1
rpcport=14022
whitelist=127.0.0.1
rpcallowip=127.0.0.1
listen=1
server=1
txindex=1
deprecatedrpc=addresses
addnode=191.81.59.115
addnode=175.45.182.173
addnode=45.76.235.153
addnode=24.74.186.115
addnode=24.101.88.154
addnode=8.214.25.169
addnode=47.75.38.245
```


## Install IPFS

Download and isntall the curretn IPFS from https://github.com/ipfs/ipfs-desktop/releases

this step will list out a lot of data of importance is the line that says "RPC API server listening on" it is usually
port 5001 note it down if it is not. You can now see IPFS usage at localhost:5001/webui in your web browser(if not
headless).

## Configure DigiAsset Core

The first time you run DigiAsset Core it will ask you several questions to set up your config file.  Run DigiAsset_Core.exe from a cmd prompt start in the c:\digiasset_core_windows folder. You will also need to open another cmd prompt and run digiasset_core-web.exe.

```bash
digiasset_core.exe
digiasset_core-web.exe

```

This will create config.cfg the wizard creates only the basic config for a full list of config options see example.cfg

Make sure DigiAsset Core is running correctly and then press ctrl+c to stop it and continue with instructions.

NOTE: You will also need to open up two firewall ports:

```bash
Inbound TCP:5001
Inbound TCP:12024
```

## Documentation

To access documentation run the digiasset_core-web application then go to http://127.0.0.1:8090/

## Other Notes

- If submitting pull requests please utilize the .clang-format file to keep things standardized.


---

