
**1. Build**

Linux/osx :
```
mkdir build && cd build && cmake .. && make
```


Windows :

Install Visual studio 2013

Install latest cmake

Download and unzip the latest boost zip from boost.org

open a developer command line for visual studio 2013 and go to the boost folder

run bootstrap

run b2 --toolset=msvc-12.0 --with-serialization --with-date_time --with-filesystem --with-program_options --with-regex --with-system --with-thread --with-chrono  --build-type=complete architecture=x86 address-model=64

Clone this repo and the xmd-coin repo in it

start cmake gui and define the wallet folder and the wallet/build as the build folder.

Add Entry and add 2 PATH cache entries :

/boost_x_xx_0/stage/lib (BOOST_LIBRARYDIR)

boost_x_xx_0 (BOOST_ROOT)


on cmake gui click Configure and select the Visual studio 12 2013 Win64

on cmake gui click Generate and then Open Project

In visual studio 2013 make sure you in Release and compile!

You will find the xmdcoin.exe in the Release folder in the build folder!
