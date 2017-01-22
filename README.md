
**1. Build**

```
mkdir build && cd build && cmake .. && make
```


Windows :

Install Visual studio 2013

Install latest cmake

Install boost 1.55 from here : http://boost.teeks99.com/bin/1.55.0/boost_1_55_0-msvc-12.0-64.exe

Clone this repo and the xmd-coin repo in it

start cmake gui and define the wallet folder and the wallet/build as the build folder.

Add Entry and add 2 PATH cache entries :

C:/local/boost_1_55_0/lib64-msvc-12.0 (BOOST_LIBRARYDIR)

C:/local/boost_1_55_0 (BOOST_ROOT)


on cmake gui click Configure and select the Visual studio 12 2013 Win64

on cmake gui click Generate and then Open Project

In visual studio 2013 make sure you in Release and compile!

You will find the xmdcoin.exe in the Release folder in the build folder!
