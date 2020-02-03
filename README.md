# libpmt
The polymorphic type library from GNU Radio as a standalone library!

# What...and...Why
## 1. What
This is the polymorphic type library, that is currently also integrated in GNU Radio itself.
It is used as a serialization/deserialization tool that is used in the GNURadio mesage passing system.

## 2. Why
At the GNU Radio Hackfest 2020 at ESA ESTEC I learned that you need to build/install the whole of GNU Radio, if you have a
machine that wants to talk to a GNU Radio flowgraph on a different machine, e.g. via thrift. I though that was insane.
So with GNU Radio maintainer blessing I started to rip libpmt out of GNU Radio.

# Attribution
LibPMT is basically just the pmt directories of GNU Radio, bend in shape to compile and build in a standalone fashion, without any GNU Radio dependency. Therefore 95% (total guess!) of the code comes directly from [GNU Radio](https://github.com/gnuradio/gnuradio). 
I also have to give a huge shoutout and thanks to [Josh Morman](https://github.com/mormj) for his pybind11 branch of GNU Radio (https://github.com/mormj/gnuradio/tree/pybind11). I bFixasically just had to copy his prior work to get libpmt up and running!

# Installation

## Get the source
```sh
git clone https://github.com/SpectreJan/libpmt.git
```

## Build the library
```sh
cd libpmt
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/home/user/opt/ -G Ninja ../
ninja
ninja test
ninja install
```
