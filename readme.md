## Table of Contents
1. [Build on Windows](#build-on-windows)
4. [Install DigiByte](#install-digibyte)
9. [Install IPFS](#install-ipfs)
15. [Documentation](#Documentation)
16. [Other Notes](#other-notes)
17. [Special Thanks](#special-thanks)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Build on Windows

This fork builds a Windows version with Visual Studio and MSVC in the main branch, with upstream tracking in the 'upstream-master' branch.

### Prerequisites

This project uses CMake for the build system. Since we're focusing on Visual Studio:

1. Install CMake support through the Visual Studio Installer (select "Desktop development with C++" and ensure "C++ CMake tools for Windows" is checked)
2. Once installed, open a command prompt with "Tools > Command Line > Developer Command Prompt"
3. You can run `cmake` commands from within this Developer Command Prompt

### Clone the Repository

If you just want to build binaries:

```cmd
git clone --depth=1 --recurse-submodules --shallow-submodules https://github.com/chopperbrian/DigiAsset_Core_Windows.git
```

Or if you want to work on the code (see CONTRIBUTING.md for more details):

```cmd
git clone --no-single-branch --recurse-submodules https://github.com/chopperbrian/DigiAsset_Core_Windows.git
```

### Set Up VCPKG

Run the following command in cmd.exe:

```cmd
cd vcpkg
.\bootstrap-vcpkg.bat
```

### Install Dependencies

At the top-level directory, execute:

```cmd
.\install-dependencies.bat
```

### Build JsonCpp Library

Execute:

```cmd
.\config-jsoncpp.bat
```

Now open the jsoncpp.sln file inside the `jsoncpp\build` directory with Visual Studio. Before building, select either "Debug" or "Release" configuration from the dropdown in the toolbar, depending on whether you want a debug or release build. From "Solution Explorer", right click on `ALL_BUILD` and select "Build". Do the same thing for `INSTALL` in order to install the library.

### Build LibJson-RPC Library

Execute:

```cmd
.\config-libjson-rpc.bat
```

Like with JsonCpp, open the libjson-rpc-cpp.sln file inside the `libjson-rpc-cpp\build` directory. Select the same build configuration ("Debug" or "Release") that you chose in the previous step. Build the `ALL_BUILD` and `INSTALL` options in "Solution Explorer".

### Build DigiAsset Core

Execute:

```cmd
.\config.bat
```

Then open the solution file in Visual Studio, select the same build configuration ("Debug" or "Release") as in the previous steps, and run "Build" on `ALL_BUILD`. The `digiasset_core.exe` binary can be found inside the `DigiAsset_Core\build\src\Debug` or `DigiAsset_Core\build\src\Release` directory, depending on your chosen configuration. It should be launched from there since there are some DLL dependencies in that location.


## Install DigiByte

```bash
wget wget https://github.com/DigiByte-Core/digibyte/releases/download/v7.17.3/digibyte-7.17.3-x86_64-linux-gnu.tar.gz
tar -xf digibyte-7.17.3-x86_64-linux-gnu.tar.gz
rm digibyte-7.17.3-x86_64-linux-gnu.tar.gz
mkdir .digibyte
nano .digibyte/digibyte.conf
```

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
addnode=191.81.59.115
addnode=175.45.182.173
addnode=45.76.235.153
addnode=24.74.186.115
addnode=24.101.88.154
addnode=8.214.25.169
addnode=47.75.38.245
```

to get digibyte to run on boot do the following

```bash
sudo nano /etc/systemd/system/digibyted.service
```

```
[Unit]
Description=DigiByte's distributed currency daemon
After=network.target

[Service]
User=<your-username>
Group=<your-username>

Type=forking
PIDFile=/home/<your-username>/.digibyte/digibyted.pid
ExecStart=/home/<your-username>/digibyte-7.17.2/bin/digibyted -daemon -pid=/home/<your-username>/.digibyte/digibyted.pid \
-conf=/home/<your-username>/.digibyte/digibyte.conf -datadir=/home/<your-username>/.digibyte

Restart=always
PrivateTmp=true
TimeoutStopSec=60s
TimeoutStartSec=2s
StartLimitInterval=120s
StartLimitBurst=5

[Install]
WantedBy=multi-user.target
```

replace <your-username>

Enable the service on boot

```bash
sudo systemctl enable digibyted.service
```

Start the service

```bash
sudo systemctl start digibyted.service
```

## Install Dependencies

```bash
sudo apt update
sudo apt upgrade
sudo apt-get install cmake libcurl4-openssl-dev libjsoncpp-dev golang-go libjsonrpccpp-dev libjsonrpccpp-tools libsqlite3-dev build-essential pkg-config zip unzip libssl-dev
sudo apt install libboost-all-dev
```

## Install VCPKG

```bash
wget -qO vcpkg.tar.gz https://github.com/microsoft/vcpkg/archive/master.tar.gz
sudo mkdir /opt/vcpkg
sudo tar xf vcpkg.tar.gz --strip-components=1 -C /opt/vcpkg
rm vcpkg.tar.gz
sudo /opt/vcpkg/bootstrap-vcpkg.sh
sudo ln -s /opt/vcpkg/vcpkg /usr/local/bin/vcpkg
```

## Install Standard C++ Dependencies

Warning: The following steps build a lot of code and can take a long time to complete

```bash
sudo vcpkg install sqlite3
```

## Update CMAKE

```bash
wget https://github.com/Kitware/CMake/releases/download/v3.27.7/cmake-3.27.7-linux-x86_64.sh
chmod +x cmake-3.27.7-linux-x86_64.sh
sudo ./cmake-3.27.7-linux-x86_64.sh --prefix=/usr/local
export PATH=/usr/local/cmake-3.27.7-linux-x86_64/bin:$PATH
nano ~/.bashrc
```

at the end of the file add

```
export PATH=/usr/local/cmake-3.27.7-linux-x86_64/bin:$PATH
```

## Install IPFS

```bash
wget https://dist.ipfs.tech/kubo/v0.22.0/kubo_v0.22.0_linux-amd64.tar.gz
tar -xvzf kubo_v0.22.0_linux-amd64.tar.gz
cd kubo
sudo bash install.sh
ipfs init
ipfs daemon

```

this step will list out a lot of data of importance is the line that says "RPC API server listening on" it is usually
port 5001 note it down if it is not. You can now see IPFS usage at localhost:5001/webui in your web browser(if not
headless).
Press Ctrl+C to stop the daemon

## Set IPFS to run on boot

```bash
cd ~
sudo nano /etc/systemd/system/ipfs.service
```

edit the file to look like this

```
[Unit]
Description=IPFS Daemon
After=network.target

[Service]
ExecStart=/usr/local/bin/ipfs daemon
User=<your-username>
Restart=always

[Install]
WantedBy=multi-user.target
```

replace <your-username>

```bash
sudo systemctl daemon-reload
sudo systemctl enable ipfs.service
sudo systemctl start ipfs.service
```

## Build DigiAsset Core

```bash
git clone -b master --recursive https://github.com/DigiAsset-Core/DigiAsset_Core.git
cd DigiAsset_Core
git submodule update --init --recursive
mkdir build
cd build
cmake -B . -S .. -DCMAKE_TOOLCHAIN_FILE=/opt/vcpkg/scripts/buildsystems/vcpkg.cmake
cmake --build .
mv src/digiasset_core ../bin
mv cli/digiasset_core-cli ../bin
mv web/digiasset_core-web ../bin
cd ../bin
```

* if you wish to build the test scripts add to first cmake -DBUILD_TEST=ON

## Configure DigiAsset Core

The first time you run DigiAsset Core it will ask you several questions to set up your config file.  Run DigiAsset Core using

```bash
./digiasset_core
```

This will create bin/config.cfg the wizard creates only the basic config for a full list of config options see example.cfg

Make sure DigiAsset Core is running correctly and then press ctrl+c to stop it and continue with instructions.

---

## Set DigiAsset Core to run at boot

```bash
sudo nano /etc/systemd/system/digiasset_core.service
```

```
[Unit]
Description=DigiAsset Core
After=network.target digibyted.service

[Service]
User=<your-username>
Group=<your-username>

Type=simple
ExecStart=/home/<your-username>/DigiAsset_Core/bin/digiasset_core
WorkingDirectory=/home/<your-username>/DigiAsset_Core/bin

Restart=always
PrivateTmp=true
TimeoutStopSec=60s
TimeoutStartSec=2s
StartLimitInterval=120s
StartLimitBurst=5

[Install]
WantedBy=multi-user.target
```

replace <your-username>

Enable the service on boot

```bash
sudo systemctl enable digiasset_core.service
```

Start the service

```bash
sudo systemctl start digiasset_core.service
```

## Upgrading DigiAsset Core

When a new version is available you can upgrade by running the following commands
```bash
cd DigiAsset_Core/bin
./digiasset_core-cli shutdown
sudo systemctl stop digiasset_core.service
cd ..
git pull
git submodule update --init --recursive
cd build
cmake -B . -S .. -DCMAKE_TOOLCHAIN_FILE=/opt/vcpkg/scripts/buildsystems/vcpkg.cmake
cmake --build .
mv src/digiasset_core ../bin
mv cli/digiasset_core-cli ../bin
mv web/digiasset_core-web ../bin
cd ../bin
sudo systemctl start digiasset_core.service
```

---

## Documentation

To access documentation run the digiasset_core-web application then go to http://127.0.0.1:8090/

## Other Notes

- If submitting pull requests please utilize the .clang-format file to keep things standardized.


---

# Special Thanks

### Major Financial Support:

RevGenetics [Longevity Supplements](https://www.RevGenetics.com)
