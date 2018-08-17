# Cross GCC-4.9 toolchain for Raspberry Pi 

## Description

Sysroot contains: Opencv3.4.2, dlib 19.15, boost_1_68, tensorflow 1.9  
  
## To build all libraries

### Build opencv
(0) Add to file platforms/linux/arm-gnueabi.toolchain.cmake
```bash
set( CMPL_PATH /home/pi/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/bin)
set( CMPL_PREF arm-linux-gnueabihf)
set( CMAKE_C_COMPILER   "${CMPL_PATH}/${CMPL_PREF}-gcc"    CACHE FILEPATH "")
set( CMAKE_CXX_COMPILER "${CMPL_PATH}/${CMPL_PREF}-g++"    CACHE FILEPATH "")
set( CMAKE_AR           "${CMPL_PATH}/${CMPL_PREF}-ar"     CACHE FILEPATH "")
set( CMAKE_RANLIB       "${CMPL_PATH}/${CMPL_PREF}-ranlib" CACHE FILEPATH "")

set( RPI_PKGCONFIG_LIBDIR "${RPI_PKGCONFIG_LIBDIR}:${RPI_ROOTFS}/usr/lib/arm-linux-gnueabihf/pkgconfig" )
set( RPI_PKGCONFIG_LIBDIR "${RPI_PKGCONFIG_LIBDIR}:${RPI_ROOTFS}/usr/share/pkgconfig" )
set( RPI_PKGCONFIG_LIBDIR "${RPI_PKGCONFIG_LIBDIR}:${RPI_ROOTFS}/opt/vc/lib/pkgconfig" )

set( PKG_CONFIG_EXECUTABLE "/usr/bin/pkg-config" CACHE FILEPATH "")
set( ENV{PKG_CONFIG_DIR}         "" CACHE FILEPATH "")
set( ENV{PKG_CONFIG_LIBDIR}      "${RPI_PKGCONFIG_LIBDIR}" CACHE FILEPATH "")
set( ENV{PKG_CONFIG_SYSROOT_DIR} "${RPI_ROOTFS}" CACHE FILEPATH "")

# setup rpi (target) directories for compiler
set( RPI_INCLUDE_DIR "${RPI_INCLUDE_DIR} -isystem ${RPI_ROOTFS}/usr/include/arm-linux-gnueabihf")
set( RPI_INCLUDE_DIR "${RPI_INCLUDE_DIR} -isystem ${RPI_ROOTFS}/usr/include")
set( RPI_INCLUDE_DIR "${RPI_INCLUDE_DIR} -isystem ${RPI_ROOTFS}/usr/local/include")

set( RPI_LIBRARY_DIR "${RPI_LIBRARY_DIR} -Wl,-rpath ${RPI_ROOTFS}/usr/lib")
```

(1) Build release with NEON, V4L and VFPV3:
```bash
cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_V4L=ON -D WITH_LIBV4L=ON -D ENABLE_NEON=ON -D ENABLE_VFPV3=ON -D CMAKE_TOOLCHAIN_FILE=../platforms/linux/arm-gnueabi.toolchain.cmake ..
```

(2) Build and install
```bash
make -j8
make install
```

### Build dlib

(0) Build without NEON:
```bash
cmake -DCMAKE_C_COMPILER=/home/pi/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc -DCMAKE_CXX_COMPILER=/home/pi/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/bin/arm-linux-gnueabihf-g++ -DCMAKE_CXX_FLAGS="-std=c++11"  -DCMAKE_INSTALL_PREFIX=install –build–config Release ..
```

Build with NEON:
```bash
cmake -DCMAKE_C_FLAGS="-O3 -mfpu=neon -fprofile-use -DENABLE_NEON" -DPNG_ARM_NEON_OPT=OFF -DNEON=ON -DCMAKE_C_COMPILER=/home/pi/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc -DCMAKE_CXX_COMPILER=/home/pi/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/bin/arm-linux-gnueabihf-g++ -DCMAKE_CXX_FLAGS="-std=c++11" -DCMAKE_INSTALL_PREFIX=install –build–config Release ..
```

(1) Build and install
```bash
make -j8
make install
```

