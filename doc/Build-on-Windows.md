# Build on Windows

I'm using [Visual Studio 2019 Community](https://visualstudio.microsoft.com/vs/community/), but I think 2017 should be OK, too.

Webcc depends on `std::filesystem` which is a C++17 feature. There's a branch which is still using `boost::filesystem` so it could be built with VS2013. Check out it if you have only VS2013.

## Install OpenSSL

Download from [here](http://slproweb.com/products/Win32OpenSSL.html).

The following installers (the "L" might change) are recommended for development:

- Win64 OpenSSL v1.1.0L
- Win32 OpenSSL v1.1.0L

During the installation, you will be asked to copy OpenSSL DLLs (`libcrypto-1_1-x64.dll` and `libssl-1_1-x64.dll`) to "The Windows system directory" or "The OpenSSL libraries (/bin) directory". If you choose the later, remember to add the path (e.g., `C:\OpenSSL-Win64\bin`) to the `PATH` environment variable.

![OpenSSL Installation](screenshots/win_openssl_install.png)

OpenSSL can also be statically linked (see `C:\OpenSSL-Win64\lib\VC\static`), but it's not recommended. Because the static libraries might not match the version of your VS.

The only drawback of dynamic link is that you must distribute the OpenSSL DLLs together with your program.

## Install Zlib

Zlib has been included in `third_party\src`. Maybe it's not a good idea.

In order to integrate `webcc` into your project, you have to integrate this zlib, too. This makes it complicated. I will come back to this later.

## Install Googletest

Download the latest release of [Googletest](https://github.com/google/googletest/releases).

Use CMake to generate VS solution:

![Googletest Installation](screenshots/win_cmake_config_gtest.png)

Please note the highlighted configurations.

The `CMAKE_INSTALL_PREFIX` has been changed to `D:/lib/cmake_install_2019_64` (NOTE: use "/" instead of "\\" as path seperators!). This path should be added to an environment variable named `CMAKE_PREFIX_PATH`. Then, CMake can find this installed Googletest during the configuration of Webcc.

![CMAKE_PREFIX_PATH](screenshots/win_cmake_prefix_path.png)

After build Googletest in VS, install it by building `INSTALL` project from the whole solution.

## Build Webcc

Open CMake, set **Where is the source code** to Webcc root directory (e.g., `D:/github/webcc`), set **Where to build the binaries** to any directory (e.g., `D:/github/webcc/build_2019_64`).

Check _**Grouped**_ and _**Advanced**_ two check boxes.

Click _**Configure**_ button, select the generator and platform (win32 or x64) from the popup dialog.

![CMake generator](screenshots/win_cmake_generator.png)

In the center of CMake, you can see a lot of configure options which are grouped. Change them according to your need. E.g., set `WEBCC_ENABLE_SSL` to `1` to enable OpenSSL.

![CMake config](screenshots/win_cmake_config.png)

Click _**Configure**_ button again. OpenSSL should be found.

![CMake config OpenSSL](screenshots/win_cmake_config_openssl.png)

Click _**Configure**_ button again. If everything is OK, click _**Generate**_ button to generate the VS solution.

Click _**Open Project**_ button to open VS.
