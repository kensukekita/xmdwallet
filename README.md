
**4. Build**

```
mkdir build && cd build && cmake .. && make
```

On windows, install visual studio 2012,visual studio 2013, latest cmake, boost 1.59. prepare with cmake (point to boost), compile with visual studio 2013
Windows :

Install Visual studio 2012

Install latest cmake

Install boost 1.55 from here : http://boost.teeks99.com/bin/1.55.0/boost_1_55_0-msvc-12.0-64.exe

start cmake and define the wallet folder and the wallet/build as the build folder.

Add Entry and add 2 PATH cache entries :

C:/local/boost_1_55_0/lib64-msvc-12.0 (BOOST_LIBRARYDIR)

C:/local/boost_1_55_0 (BOOST_ROOT)
