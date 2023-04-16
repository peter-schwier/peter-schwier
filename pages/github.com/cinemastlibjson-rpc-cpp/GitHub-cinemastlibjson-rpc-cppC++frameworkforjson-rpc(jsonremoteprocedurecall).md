---
title: GitHub - cinemast/libjson-rpc-cpp: C++ framework for json-rpc (json remote procedure call)
pageTitle: GitHub - cinemast/libjson-rpc-cpp: C++ framework for json-rpc (json remote procedure call)
length: 5695
author: cinemast
timestamp: 2023-04-16T15:03:00-05:00
markdownload-tags: []
markdownload-source: https://github.com/cinemast/libjson-rpc-cpp
markdownload-hostname: github.com
---

# GitHub - cinemast/libjson-rpc-cpp: C++ framework for json-rpc (json remote procedure call)

## Excerpt
> C++ framework for json-rpc (json remote procedure call) - GitHub - cinemast/libjson-rpc-cpp: C++ framework for json-rpc (json remote procedure call)

---
**I created a new C++17 header only implementation -> [json-rpc-cxx](https://github.com/jsonrpcx/json-rpc-cxx).**

Master [![CircleCI][fig1]](https://circleci.com/gh/cinemast/libjson-rpc-cpp/tree/master) [![codecov][fig2]](https://codecov.io/gh/cinemast/libjson-rpc-cpp)

## libjson-rpc-cpp

This framework provides cross platform JSON-RPC (remote procedure call) support for C++. It is fully JSON-RPC [2.0 & 1.0 compatible](http://www.jsonrpc.org/specification).

[![libjson-rpc-cpp logo][fig3]](https://github.com/cinemast/libjson-rpc-cpp/blob/master/dev/artwork/logo.png?raw=true)

**5 good reasons for using libjson-rpc-cpp in your next RPC project**

-   Full JSON-RPC 2.0 & partial JSON-RPC 1.0 client and server Support.
-   jsonrpcstub - a tool that generates stub-classes for your JSON-RPC client and server applications.
-   Ready to use HTTP + TCP server and client to provide simple interfaces for your JSON-RPC application.
-   Cross platform build support for Linux and OS X.
-   Super liberal [MIT-License](http://en.wikipedia.org/wiki/MIT_License).

**Other good reasons to use libjson-rpc-cpp**

-   Easy to use [cmake](http://www.cmake.org/) cross platform build system.
-   Clean and simple architecture, which makes it easy to extend.
-   Continuously tested under MacOS, and [various linux distributions](https://travis-ci.org/cinemast/libjson-rpc-cpp).
-   Automated testing using `make test`.
-   Useful Examples provided. e.g. XBMC Remote using json-rpc client part and stub generator.
-   The stubgenerator currently supports C++, JavaScript, and Python.

## Overview

[![libjson-rpc-cpp logo][fig4]](https://github.com/cinemast/libjson-rpc-cpp/blob/master/dev/artwork/overview.png?raw=true)

## Install the framework

**Debian (stretch) and Ubuntu (15.10 or later)**

```shell
sudo apt-get install libjsonrpccpp-dev libjsonrpccpp-tools
```

**Fedora**

```shell
sudo dnf install libjson-rpc-cpp-devel libjson-rpc-cpp-tools
```

**Arch Linux**

For Arch Linux there is a [PKGBUILD provided in the AUR](https://aur.archlinux.org/packages/libjson-rpc-cpp/).

```shell
sudo aura -A libjson-rpc-cpp
```

**Gentoo Linux**

```shell
sudo emerge dev-cpp/libjson-rpc-cpp
```

**Mac OS X**

For OS X a [Brew](http://brew.sh/) package is available:

```shell
brew install libjson-rpc-cpp
```

**Windows**

There is a ready to use compiled package [here](http://spiessknafl.at/libjson-rpc-cpp). Just download execute the installer EXE.

## Build from source

## Install the dependencies

-   [libcurl](http://curl.haxx.se/)
-   [libmicrohttpd](http://www.gnu.org/software/libmicrohttpd/)
-   [libjsoncpp](https://github.com/open-source-parsers/jsoncpp)
-   [libargtable](http://argtable.sourceforge.net/)
-   [cmake](http://www.cmake.org/)
-   [libhiredis](https://redislabs.com/lp/hiredis/)
-   [catch](https://github.com/catchorg/Catch2)

**UNIX**

For Debian and Arch GNU/Linux based systems, all dependencies are available via the package manager. For OS X all dependencies are available in [Brew](http://brew.sh/)

## Build

```shell
git clone https://github.com/cinemast/libjson-rpc-cpp.git
mkdir -p libjson-rpc-cpp/build
cd libjson-rpc-cpp/build
cmake .. && make
sudo make install
sudo ldconfig          #only required for linux
```

That's it!

If you are not happy with it, simply uninstall it from your system using (inside the build the directory):

**Build options:**

Default configuration should be fine for most systems, but here are available compilation flags:

-   `-DCOMPILE_TESTS=NO` disables unit test suite.
-   `-DCOMPILE_STUBGEN=NO` disables building the stubgenerator.
-   `-DCOMPILE_EXAMPLES=NO` disables examples.
-   `-DHTTP_SERVER=NO` disable the libmicrohttpd webserver.
-   `-DHTTP_CLIENT=NO` disable the curl client.
-   `-DREDIS_SERVER=NO` disable the redis server connector.
-   `-DREDIS_CLIENT=NO` disable the redis client connector.
-   `-DUNIX_DOMAIN_SOCKET_SERVER=YES` enable the unix domain socket server connector.
-   `-DUNIX_DOMAIN_SOCKET_CLIENT=YES` enable the unix domain socket client connector.
-   `-DFILE_DESCRIPTOR_SERVER=NO` disable the file descriptor server connector.
-   `-DFILE_DESCRIPTOR_CLIENT=NO` disable the file descriptor client connector.
-   `-DTCP_SOCKET_SERVER=NO` disable the tcp socket server connector.
-   `-DTCP_SOCKET_CLIENT=NO` disable the tcp socket client connector.

## Using the framework

This example will show the most simple way to create a rpc server and client. If you only need the server, ignore step 4. If you only need the client, ignore step 3. You can find all resources of this sample in the `src/examples` directory of this repository.

### Step 1: Writing the specification file

```json
[
{
"name": "sayHello",
"params": {
"name": "Peter"
},
"returns" : "Hello Peter"
},
{
"name" : "notifyServer"
}
]
```

The type of a return value or parameter is defined by the literal assigned to it. The generated stubs will will use the "returns" type to validate the response. In this example you can see how to specify methods and notifications.

### Step 2: Generate the stubs for client and server

Call jsonrpcstub:

```shell
jsonrpcstub spec.json --cpp-server=AbstractStubServer --cpp-client=StubClient
mkdir -p gen
mv abstractstubserver.h gen
mv stubclient.h gen
```

This generates an `AbstractStubServer` and a `StubClient` class and moves them to the `gen` folder.

### Step 3: implement the abstract server stub

Extend the abstract server stub and implement all pure virtual (abstract) methods defined in `spec.json`.

See [src/examples/stubserver.cpp](https://github.com/cinemast/libjson-rpc-cpp/blob/master/src/examples/stubserver.cpp)

In the main function the concrete server is instantiated and started. That is all for the server. Any JSON-RPC 2.0 compliant client can now connect to your server.

Compile the server with:

```shell
g++ stubserver.cpp -ljsoncpp -lmicrohttpd -ljsonrpccpp-common -ljsonrpccpp-server -o sampleserver
```

### Step 4: Create the client application

See [src/examples/stubclient.cpp](https://github.com/cinemast/libjson-rpc-cpp/blob/master/src/examples/stubclient.cpp)

Compile the client with:

```shell
g++ stubclient.cpp -ljsoncpp -lcurl -ljsonrpccpp-common -ljsonrpccpp-client -o sampleclient
```

## Contributions

Please take a look at [CONTRIBUTING.md](https://github.com/cinemast/libjson-rpc-cpp/blob/master/CONTRIBUTING.md)

You can also donate via [![Donate][fig5]](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=CJV4LY486L838&source=url)

## Changelogs

Changelogs can be found [here](https://github.com/cinemast/libjson-rpc-cpp/blob/master/CHANGELOG.md).

## API compatibility

We do our best to keep the API/ABI stable, to prevent problems when updating this framework. A compatiblity report can be found [here](http://upstream.rosalinux.ru/versions/libjson-rpc-cpp.html).

## License

This framework is licensed under [MIT](http://en.wikipedia.org/wiki/MIT_License). All of this libraries dependencies are licensed under MIT compatible licenses.

## References

-   [NASA Ames Research Center](http://www.nasa.gov/centers/ames/home/): use it to obtain aircraft state information from an aircraft simulator.
-   [LaseShark 3D Printer](https://github.com/macpod/lasershark_3dp): used to control the firmware of the 3D printer.
-   [cpp-ethereum](https://github.com/ethereum/cpp-ethereum): C++ implementations for the ethereum crypto currency.
-   [mage-sdk-cpp](https://github.com/mage/mage-sdk-cpp): a game engine.
-   [bitcodin](http://www.bitmovin.net/): a scalable cloud based video transcoding platform.
-   [wgslib](http://wgslib.com/): a web geostatistics library.
-   [bitcoin-api-cpp](https://github.com/minium/bitcoin-api-cpp): a C++ interface to bitcoin.
-   [NIT DASH Content Server](http://www.nit.eu/offer/research-projects-products/334-http2dash): Dynamic Adaptive Streaming over HTTP server.

If you use this library and find it useful, I would be very pleased if you let me know about it.

[fig1]: https://camo.githubusercontent.com/995be0ad25ce4065f178364944738620b93c1ae9581bfa4df0a22bd02d7df07d/68747470733a2f2f636972636c6563692e636f6d2f67682f63696e656d6173742f6c69626a736f6e2d7270632d6370702f747265652f6d61737465722e7376673f7374796c653d737667
[fig2]: https://camo.githubusercontent.com/690a4ae6a7f1073a511532313be19c121b51f58f2e458d49cee2337e100a97a2/68747470733a2f2f636f6465636f762e696f2f67682f63696e656d6173742f6c69626a736f6e2d7270632d6370702f6272616e63682f6d61737465722f67726170682f62616467652e7376673f746f6b656e3d514e6f58736149327461
[fig3]: https://github.com/cinemast/libjson-rpc-cpp/raw/master/dev/artwork/logo.png?raw=true
[fig4]: https://github.com/cinemast/libjson-rpc-cpp/raw/master/dev/artwork/overview.png?raw=true
[fig5]: https://camo.githubusercontent.com/2bfa6102e99ff9a137185897b0a566aa0977a4790348c462e6951829e787af8f/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d50617950616c2d677265656e2e737667

> Saved from https://github.com/cinemast/libjson-rpc-cpp on 2023-04-16T15:03:00-05:00