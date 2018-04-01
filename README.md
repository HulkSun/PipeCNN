# PipeCNN
 
Fork from [PipeCNN](https://github.com/doonny/PipeCNN/)

## Enveriment
- Centos7
- Cmake3

## Usage
```bash
$ git clone https://github.com/HULKSUN/PipeCNN.git 
$ cd PipeCNN
$ mkdir build
$ cd build
$ cmake3 .. && make
$ cd bin
$ env CL_CONTEXT_EMULATOR_DEVICE_ALTERA=1 ./pipeCNN conv.aocx
```