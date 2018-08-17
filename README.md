# Cross GCC-4.9 toolchain for Raspberry Pi 

## Description

Sysroot contains: Opencv3.4.2, dlib 19.15, boost_1_68, tensorflow 1.9  
  
## To build all libraries

### Build dlib

(0) For build without NEON:
```bash
cmake -DCMAKE_C_COMPILER=/home/pi/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc -DCMAKE_CXX_COMPILER=/home/pi/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/bin/arm-linux-gnueabihf-g++ -DCMAKE_CXX_FLAGS="-std=c++11"  -DCMAKE_INSTALL_PREFIX=install –build–config Release ..
```

For build with NEON:
```bash
cmake -DCMAKE_C_FLAGS="-O3 -mfpu=neon -fprofile-use -DENABLE_NEON" -DPNG_ARM_NEON_OPT=OFF -DNEON=ON -DCMAKE_C_COMPILER=/home/pi/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc -DCMAKE_CXX_COMPILER=/home/pi/tools/arm-bcm2708/arm-rpi-4.9.3-linux-gnueabihf/bin/arm-linux-gnueabihf-g++ -DCMAKE_CXX_FLAGS="-std=c++11" -DCMAKE_INSTALL_PREFIX=install –build–config Release ..
```

(1) Build and install
```bash
make -j8
make install
```

