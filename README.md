Darknet on OpenCL
==========

## Darknet
Darknet is an open source neural network framework written in C and CUDA.<br> 
It is fast, easy to install, and supports CPU and GPU computation.<br>
You can find the origin project [here](https://github.com/pjreddie/darknet).<br>
## Darknet on OpenCL
As the origin darknet project is written in CUDA, this project is to port<br>
the darknet to OpenCL. Also, darknet is assumed to run on Linux and used <br>
some POSIX C APIs, This project rewrite this with standard C++ libraries.<br>
So that it could also run on Windows.<br>
## Dependency
`OpenCL`<br>
Make sure you have OpenCL installed, and set environment variables OPENCL_SDK <br>
point to your OpenCL installed path.<br>
`clBLAS`<br>
clBLAS is equivalent to cuBLAS, you can find the source code [here](https://github.com/clMathLibraries/clBLAS)<br>
and compile it yourself.<br>
or you can use binary library for Windows/Ubuntu x64 platform I have already provided<br>
You can find clBLAS.lib/clBLAS.dll for Windows and libclBLAS.so for Linux <br>
as well as header file [here](https://github.com/ganyc717/Darknet-On-OpenCL/tree/master/darknet_cl/clBLAS).<br>
## Build
`Windows`<br>
This project is prepared with Visual Studio 2017, just open darknet_cl.sln<br>
and build it.<br>
To enable OpenCL, please set environment variables OPENCL_SDK first.<br>
To enable OpenCV, please set environment variables OPENCV_INCLUDE_DIR<br>
and OPENCV_LIB first.<br>
`Linux`<br>
mkdir build && cd build<br>
cmake ../<br>
make<br>

### MacOSX
OpenCL is already installed on MacOSX

#### cBLAS
Use brew to install clblas

```
brew install clblas
```

#### Fix OpenCL Includes

```
# find all files with the wrong include
grep --include=\*.{c,h} -rnw './darknet_cl/include' -e "#include <CL/cl.h>"
```

Replace with:

```
#if defined(__APPLE__) || defined(__MACOSX)
#include <OpenCL/cl.h>
#else
#include <CL/cl.h>
#endif
```

#### Build

Please change the opencv path to your installation folder (the folder should contain following file: `OpenCVConfig.cmake`).

```
mkdir build && cd build
cmake -DOpenCV_DIR="/usr/local/Cellar/opencv/3.4.1_2/share/OpenCV" ..
make
```

Copy `libclBLAS.dylib` into include and replace `libclBLAS.co` with `libclBLAS.dylib` in following files:

```
link.txt
build.make
```

Then run:

```
make
```

## Usage
Once you compiled this project, it has the same usage as darknet,<br>
you can find it [here](https://pjreddie.com/darknet/).<br>
If you compile the project depend on the clBLAS library I provided, you'd better<br>
copy dependent library clBLAS.dll or libclBLAS.so to<br>
system lib path.(C:\\Windows\\System32 or /usr/lib).<br>
## Attention
This project didn't build the DarkGo into the darknet_cl, maybe support it later.<br>
