
# 与numpy结合实现高效的数组操作

下面的代码将图像与滤镜进行二维离散卷积(我相信你可以做得更好,让它用于演示).它是有效的Python和有效的Cython代码.我将其称为Python版本的`convolve_py.py`和Cython版本的`convolve1.pyx`


```python
%%writefile convolve_py.py
import numpy as np
def naive_convolve(f, g):
    # f is an image and is indexed by (v, w)
    # g is a filter kernel and is indexed by (s, t),
    #   it needs odd dimensions
    # h is the output image and is indexed by (x, y),
    #   it is not cropped
    if g.shape[0] % 2 != 1 or g.shape[1] % 2 != 1:
        raise ValueError("Only odd dimensions on filter supported")
    # smid and tmid are number of pixels between the center pixel
    # and the edge, ie for a 5x5 filter they will be 2.
    #
    # The output size is calculated by adding smid, tmid to each
    # side of the dimensions of the input image.
    vmax = f.shape[0]
    wmax = f.shape[1]
    smax = g.shape[0]
    tmax = g.shape[1]
    smid = smax // 2
    tmid = tmax // 2
    xmax = vmax + 2*smid
    ymax = wmax + 2*tmid
    # Allocate result image.
    h = np.zeros([xmax, ymax], dtype=f.dtype)
    # Do convolution
    for x in range(xmax):
        for y in range(ymax):
            # Calculate pixel value for h at (x,y). Sum one component
            # for each pixel (s, t) of the filter g.
            s_from = max(smid - x, -smid)
            s_to = min((xmax - x) - smid, smid + 1)
            t_from = max(tmid - y, -tmid)
            t_to = min((ymax - y) - tmid, tmid + 1)
            value = 0
            for s in range(s_from, s_to):
                for t in range(t_from, t_to):
                    v = x - smid + s
                    w = y - tmid + t
                    value += g[smid - s, tmid - t] * f[v, w]
            h[x, y] = value
    return h
```

    Writing convolve_py.py



```python
import numpy as np
```


```python
N = 100
f = np.arange(N*N, dtype=np.int).reshape((N,N))
g = np.arange(81, dtype=np.int).reshape((9, 9))
```


```python
from convolve_py import naive_convolve
```


```python
naive_convolve(f,g)
```




    array([[      0,       0,       1, ...,    2056,    1477,     792],
           [      0,     109,     329, ...,    8858,    6227,    3275],
           [    900,    2127,    3684, ...,   23106,   16050,    8349],
           ..., 
           [1850400, 3730389, 5639970, ..., 6230334, 4183464, 2106687],
           [1329300, 2678435, 4047407, ..., 4445402, 2983649, 1501849],
           [ 712800, 1435572, 2168317, ..., 2369524, 1589761,  799920]])




```python
%timeit -n10 naive_convolve(f,g)
```

    786 ms ± 16.5 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)


使用cython编译带numpy的代码需要在setup.py中指定`include_dirs=[numpy.get_include()]`

## 第一版迭代--使用cython编译以提高性能

代码不用改,直接编译以提高性能


```python
%%writefile convolve1.pyx

import numpy as np
def naive_convolve_1(f, g):
    # f is an image and is indexed by (v, w)
    # g is a filter kernel and is indexed by (s, t),
    #   it needs odd dimensions
    # h is the output image and is indexed by (x, y),
    #   it is not cropped
    if g.shape[0] % 2 != 1 or g.shape[1] % 2 != 1:
        raise ValueError("Only odd dimensions on filter supported")
    # smid and tmid are number of pixels between the center pixel
    # and the edge, ie for a 5x5 filter they will be 2.
    #
    # The output size is calculated by adding smid, tmid to each
    # side of the dimensions of the input image.
    vmax = f.shape[0]
    wmax = f.shape[1]
    smax = g.shape[0]
    tmax = g.shape[1]
    smid = smax // 2
    tmid = tmax // 2
    xmax = vmax + 2*smid
    ymax = wmax + 2*tmid
    # Allocate result image.
    h = np.zeros([xmax, ymax], dtype=f.dtype)
    # Do convolution
    for x in range(xmax):
        for y in range(ymax):
            # Calculate pixel value for h at (x,y). Sum one component
            # for each pixel (s, t) of the filter g.
            s_from = max(smid - x, -smid)
            s_to = min((xmax - x) - smid, smid + 1)
            t_from = max(tmid - y, -tmid)
            t_to = min((ymax - y) - tmid, tmid + 1)
            value = 0
            for s in range(s_from, s_to):
                for t in range(t_from, t_to):
                    v = x - smid + s
                    w = y - tmid + t
                    value += g[smid - s, tmid - t] * f[v, w]
            h[x, y] = value
    return h
```

    Writing convolve1.pyx



```python
%%writefile setup.py

from distutils.core import setup
from Cython.Build import cythonize
from distutils.extension import Extension
from Cython.Distutils import build_ext
import numpy

extension = Extension(
           "convolve1",
           sources=["convolve1.pyx"],
           include_dirs=[numpy.get_include()], # 如果用到numpy
           language="c++"
)

setup(
        cmdclass = {'build_ext': build_ext},
        ext_modules = cythonize(extension),
)

```

    Writing setup.py



```python
!python setup.py build_ext --inplace
```

    Compiling convolve1.pyx because it changed.
    [1/1] Cythonizing convolve1.pyx
    running build_ext
    building 'convolve1' extension
    creating build
    creating build/temp.macosx-10.7-x86_64-3.6
    gcc -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -I/Users/huangsizhe/LIB/CONDA/anaconda/include -arch x86_64 -I/Users/huangsizhe/LIB/CONDA/anaconda/include -arch x86_64 -I/Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include -I/Users/huangsizhe/LIB/CONDA/anaconda/include/python3.6m -c convolve1.cpp -o build/temp.macosx-10.7-x86_64-3.6/convolve1.o
    g++ -bundle -undefined dynamic_lookup -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -arch x86_64 build/temp.macosx-10.7-x86_64-3.6/convolve1.o -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -o /Users/huangsizhe/WORKSPACE/Blog/Docs/Python/TutorialForPythonDataAnalysis/ipynbs/numpy/convolve1.cpython-36m-darwin.so
    clang: [0;1;35mwarning: [0mlibstdc++ is deprecated; move to libc++ with a minimum deployment target of OS X 10.9[0m



```python
from convolve1 import naive_convolve_1
```


```python
naive_convolve_1(f,g)
```




    array([[      0,       0,       1, ...,    2056,    1477,     792],
           [      0,     109,     329, ...,    8858,    6227,    3275],
           [    900,    2127,    3684, ...,   23106,   16050,    8349],
           ..., 
           [1850400, 3730389, 5639970, ..., 6230334, 4183464, 2106687],
           [1329300, 2678435, 4047407, ..., 4445402, 2983649, 1501849],
           [ 712800, 1435572, 2168317, ..., 2369524, 1589761,  799920]])




```python
%timeit -n10 naive_convolve_1(f,g)
```

    594 ms ± 14.2 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)


第一版什么也不改就可以提高1/4的性能

## 第二版迭代--静态化参数

将函数的参数以及中间变量都申明为静态类型以提高运行效率


```python
%%writefile convolve2.pyx
import numpy as np##必须为c类型和python类型的数据都申明一个np

cimport numpy as np

DTYPE = np.int

ctypedef np.int_t DTYPE_t

# 参数静态化
def naive_convolve_2(np.ndarray f, np.ndarray g):
    if g.shape[0] % 2 != 1 or g.shape[1] % 2 != 1:
        raise ValueError("Only odd dimensions on filter supported")
    assert f.dtype == DTYPE and g.dtype == DTYPE
    
    #将中间变量都静态化
    cdef int vmax = f.shape[0]
    cdef int wmax = f.shape[1]
    cdef int smax = g.shape[0]
    cdef int tmax = g.shape[1]
    cdef int smid = smax // 2
    cdef int tmid = tmax // 2
    cdef int xmax = vmax + 2*smid
    cdef int ymax = wmax + 2*tmid
    cdef np.ndarray h = np.zeros([xmax, ymax], dtype=DTYPE)
    cdef int x, y, s, t, v, w
    cdef int s_from, s_to, t_from, t_to
    cdef DTYPE_t value
    
    for x in range(xmax):
        for y in range(ymax):
            s_from = max(smid - x, -smid)
            s_to = min((xmax - x) - smid, smid + 1)
            t_from = max(tmid - y, -tmid)
            t_to = min((ymax - y) - tmid, tmid + 1)
            value = 0
            for s in range(s_from, s_to):
                for t in range(t_from, t_to):
                    v = x - smid + s
                    w = y - tmid + t
                    value += g[smid - s, tmid - t] * f[v, w]
            h[x, y] = value
    return h
```

    Writing convolve2.pyx



```python
%%writefile setup.py

from distutils.core import setup
from Cython.Build import cythonize
from distutils.extension import Extension
from Cython.Distutils import build_ext
import numpy

extension = Extension(
           "convolve2",
           sources=["convolve2.pyx"],
           include_dirs=[numpy.get_include()], # 如果用到numpy
           language="c++"
)

setup(
        cmdclass = {'build_ext': build_ext},
        ext_modules = cythonize(extension),
)

```

    Overwriting setup.py



```python
!python setup.py build_ext --inplace
```

    Compiling convolve2.pyx because it changed.
    [1/1] Cythonizing convolve2.pyx
    running build_ext
    building 'convolve2' extension
    gcc -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -I/Users/huangsizhe/LIB/CONDA/anaconda/include -arch x86_64 -I/Users/huangsizhe/LIB/CONDA/anaconda/include -arch x86_64 -I/Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include -I/Users/huangsizhe/LIB/CONDA/anaconda/include/python3.6m -c convolve2.cpp -o build/temp.macosx-10.7-x86_64-3.6/convolve2.o
    In file included from convolve2.cpp:465:
    In file included from /Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/arrayobject.h:4:
    In file included from /Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/ndarrayobject.h:18:
    In file included from /Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/ndarraytypes.h:1777:
    [1m/Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/npy_1_7_deprecated_api.h:15:2: [0m[0;1;35mwarning: [0m[1m
          "Using deprecated NumPy API, disable it by "          "#defining
          NPY_NO_DEPRECATED_API NPY_1_7_API_VERSION" [-W#warnings][0m
    #warning "Using deprecated NumPy API, disable it by " \
    [0;1;32m ^
    [0m1 warning generated.
    g++ -bundle -undefined dynamic_lookup -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -arch x86_64 build/temp.macosx-10.7-x86_64-3.6/convolve2.o -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -o /Users/huangsizhe/WORKSPACE/Blog/Docs/Python/TutorialForPythonDataAnalysis/ipynbs/numpy/convolve2.cpython-36m-darwin.so
    clang: [0;1;35mwarning: [0mlibstdc++ is deprecated; move to libc++ with a minimum deployment target of OS X 10.9[0m



```python
from convolve2 import naive_convolve_2
```


```python
naive_convolve_2(f,g)
```




    array([[      0,       0,       1, ...,    2056,    1477,     792],
           [      0,     109,     329, ...,    8858,    6227,    3275],
           [    900,    2127,    3684, ...,   23106,   16050,    8349],
           ..., 
           [1850400, 3730389, 5639970, ..., 6230334, 4183464, 2106687],
           [1329300, 2678435, 4047407, ..., 4445402, 2983649, 1501849],
           [ 712800, 1435572, 2168317, ..., 2369524, 1589761,  799920]])




```python
%timeit -n10 naive_convolve_2(f,g)
```

    593 ms ± 16.1 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)


## 第三版迭代--“缓冲”语法

提高np数组的效率,我们用一个特殊的“缓冲”语法来做到这一点，它必须告诉数据类型（第一个参数）和维数（“ndim”仅关键字参数，如果不提供，则假定一维


```python
%%writefile convolve3.pyx
import numpy as np##必须为c类型和python类型的数据都申明一个np

cimport numpy as np

DTYPE = np.int

ctypedef np.int_t DTYPE_t
# “缓冲”语法
def naive_convolve_3(np.ndarray[DTYPE_t, ndim=2] f, np.ndarray[DTYPE_t, ndim=2] g):
    if g.shape[0] % 2 != 1 or g.shape[1] % 2 != 1:
        raise ValueError("Only odd dimensions on filter supported")
    assert f.dtype == DTYPE and g.dtype == DTYPE
   
    cdef int vmax = f.shape[0]
    cdef int wmax = f.shape[1]
    cdef int smax = g.shape[0]
    cdef int tmax = g.shape[1]
    cdef int smid = smax // 2
    cdef int tmid = tmax // 2
    cdef int xmax = vmax + 2*smid
    cdef int ymax = wmax + 2*tmid
    # “缓冲”语法
    cdef np.ndarray[DTYPE_t, ndim=2] h = np.zeros([xmax, ymax], dtype=DTYPE)
    
    cdef int x, y, s, t, v, w

    cdef int s_from, s_to, t_from, t_to
 
    cdef DTYPE_t value
    for x in range(xmax):
        for y in range(ymax):
            s_from = max(smid - x, -smid)
            s_to = min((xmax - x) - smid, smid + 1)
            t_from = max(tmid - y, -tmid)
            t_to = min((ymax - y) - tmid, tmid + 1)
            value = 0
            for s in range(s_from, s_to):
                for t in range(t_from, t_to):
                    v = x - smid + s
                    w = y - tmid + t
                    value += g[smid - s, tmid - t] * f[v, w]
            h[x, y] = value
    return h
```

    Overwriting convolve3.pyx



```python
%%writefile setup.py

from distutils.core import setup
from Cython.Build import cythonize
from distutils.extension import Extension
from Cython.Distutils import build_ext
import numpy

extension = Extension(
           "convolve3",
           sources=["convolve3.pyx"],
           include_dirs=[numpy.get_include()], # 如果用到numpy
           language="c++"
)

setup(
        cmdclass = {'build_ext': build_ext},
        ext_modules = cythonize(extension),
)

```

    Overwriting setup.py



```python
!python setup.py build_ext --inplace
```

    Compiling convolve3.pyx because it changed.
    [1/1] Cythonizing convolve3.pyx
    running build_ext
    building 'convolve3' extension
    gcc -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -I/Users/huangsizhe/LIB/CONDA/anaconda/include -arch x86_64 -I/Users/huangsizhe/LIB/CONDA/anaconda/include -arch x86_64 -I/Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include -I/Users/huangsizhe/LIB/CONDA/anaconda/include/python3.6m -c convolve3.cpp -o build/temp.macosx-10.7-x86_64-3.6/convolve3.o
    In file included from convolve3.cpp:465:
    In file included from /Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/arrayobject.h:4:
    In file included from /Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/ndarrayobject.h:18:
    In file included from /Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/ndarraytypes.h:1777:
    [1m/Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/npy_1_7_deprecated_api.h:15:2: [0m[0;1;35mwarning: [0m[1m
          "Using deprecated NumPy API, disable it by "          "#defining
          NPY_NO_DEPRECATED_API NPY_1_7_API_VERSION" [-W#warnings][0m
    #warning "Using deprecated NumPy API, disable it by " \
    [0;1;32m ^
    [0m1 warning generated.
    g++ -bundle -undefined dynamic_lookup -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -arch x86_64 build/temp.macosx-10.7-x86_64-3.6/convolve3.o -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -o /Users/huangsizhe/WORKSPACE/Blog/Docs/Python/TutorialForPythonDataAnalysis/ipynbs/numpy/convolve3.cpython-36m-darwin.so
    clang: [0;1;35mwarning: [0mlibstdc++ is deprecated; move to libc++ with a minimum deployment target of OS X 10.9[0m



```python
from convolve3 import naive_convolve_3
```


```python
naive_convolve_3(f,g)
```




    array([[      0,       0,       1, ...,    2056,    1477,     792],
           [      0,     109,     329, ...,    8858,    6227,    3275],
           [    900,    2127,    3684, ...,   23106,   16050,    8349],
           ..., 
           [1850400, 3730389, 5639970, ..., 6230334, 4183464, 2106687],
           [1329300, 2678435, 4047407, ..., 4445402, 2983649, 1501849],
           [ 712800, 1435572, 2168317, ..., 2369524, 1589761,  799920]])




```python
%timeit -n10 naive_convolve_3(f,g)
```

    3.23 ms ± 452 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)


提高了150倍的性能

## 第四版迭代--调整cython设置进一步的提高数组效率



```python
%%writefile convolve4.pyx
import numpy as np##必须为c类型和python类型的数据都申明一个np

cimport numpy as np

DTYPE = np.int

ctypedef np.int_t DTYPE_t

cimport cython
@cython.boundscheck(False) # 关闭边界检查
@cython.wraparound(False)  # 关闭负指数换行
def naive_convolve_4(np.ndarray[DTYPE_t, ndim=2] f, np.ndarray[DTYPE_t, ndim=2] g):
    if g.shape[0] % 2 != 1 or g.shape[1] % 2 != 1:
        raise ValueError("Only odd dimensions on filter supported")
    assert f.dtype == DTYPE and g.dtype == DTYPE
    
    cdef int vmax = f.shape[0]
    cdef int wmax = f.shape[1]
    cdef int smax = g.shape[0]
    cdef int tmax = g.shape[1]
    cdef int smid = smax // 2
    cdef int tmid = tmax // 2
    cdef int xmax = vmax + 2*smid
    cdef int ymax = wmax + 2*tmid
    # “缓冲”语法
    cdef np.ndarray[DTYPE_t, ndim=2] h = np.zeros([xmax, ymax], dtype=DTYPE)
    cdef int s, t
    cdef unsigned int x, y, v, w 
   
    cdef int s_from, s_to, t_from, t_to
   
    cdef DTYPE_t value
    for x in range(xmax):
        for y in range(ymax):
            s_from = max(smid - x, -smid)
            s_to = min((xmax - x) - smid, smid + 1)
            t_from = max(tmid - y, -tmid)
            t_to = min((ymax - y) - tmid, tmid + 1)
            value = 0
            for s in range(s_from, s_to):
                for t in range(t_from, t_to):
                    v = <unsigned int>(x - smid + s)#指定类型
                    w = <unsigned int>(y - tmid + t)#指定类型
                    value += g[<unsigned int>(smid - s), <unsigned int>(tmid - t)] * f[v, w]#指定类型
            h[x, y] = value
    return h
```

    Overwriting convolve4.pyx



```python
%%writefile setup.py

from distutils.core import setup
from Cython.Build import cythonize
from distutils.extension import Extension
from Cython.Distutils import build_ext
import numpy

extension = Extension(
           "convolve4",
           sources=["convolve4.pyx"],
           include_dirs=[numpy.get_include()], # 如果用到numpy
           language="c++"
)

setup(
        cmdclass = {'build_ext': build_ext},
        ext_modules = cythonize(extension),
)

```

    Overwriting setup.py



```python
!python setup.py build_ext --inplace
```

    Compiling convolve4.pyx because it changed.
    [1/1] Cythonizing convolve4.pyx
    running build_ext
    building 'convolve4' extension
    gcc -Wno-unused-result -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -I/Users/huangsizhe/LIB/CONDA/anaconda/include -arch x86_64 -I/Users/huangsizhe/LIB/CONDA/anaconda/include -arch x86_64 -I/Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include -I/Users/huangsizhe/LIB/CONDA/anaconda/include/python3.6m -c convolve4.cpp -o build/temp.macosx-10.7-x86_64-3.6/convolve4.o
    In file included from convolve4.cpp:465:
    In file included from /Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/arrayobject.h:4:
    In file included from /Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/ndarrayobject.h:18:
    In file included from /Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/ndarraytypes.h:1777:
    [1m/Users/huangsizhe/LIB/CONDA/anaconda/lib/python3.6/site-packages/numpy/core/include/numpy/npy_1_7_deprecated_api.h:15:2: [0m[0;1;35mwarning: [0m[1m
          "Using deprecated NumPy API, disable it by "          "#defining
          NPY_NO_DEPRECATED_API NPY_1_7_API_VERSION" [-W#warnings][0m
    #warning "Using deprecated NumPy API, disable it by " \
    [0;1;32m ^
    [0m[1mconvolve4.cpp:1901:33: [0m[0;1;35mwarning: [0m[1mcomparison of integers of different signs:
          'unsigned int' and 'int' [-Wsign-compare][0m
      for (__pyx_t_9 = 0; __pyx_t_9 < __pyx_t_8; __pyx_t_9+=1) {
    [0;1;32m                      ~~~~~~~~~ ^ ~~~~~~~~~
    [0m[1mconvolve4.cpp:1912:37: [0m[0;1;35mwarning: [0m[1mcomparison of integers of different signs:
          'unsigned int' and 'int' [-Wsign-compare][0m
        for (__pyx_t_11 = 0; __pyx_t_11 < __pyx_t_10; __pyx_t_11+=1) {
    [0;1;32m                         ~~~~~~~~~~ ^ ~~~~~~~~~~
    [0m[1mconvolve4.cpp:1924:24: [0m[0;1;35mwarning: [0m[1mcomparison of integers of different signs: 'int'
          and 'unsigned int' [-Wsign-compare][0m
          if (((__pyx_t_12 > __pyx_t_13) != 0)) {
    [0;1;32m            ~~~~~~~~~~ ^ ~~~~~~~~~~
    [0m[1mconvolve4.cpp:1956:24: [0m[0;1;35mwarning: [0m[1mcomparison of integers of different signs: 'int'
          and 'unsigned int' [-Wsign-compare][0m
          if (((__pyx_t_12 > __pyx_t_14) != 0)) {
    [0;1;32m            ~~~~~~~~~~ ^ ~~~~~~~~~~
    [0m5 warnings generated.
    g++ -bundle -undefined dynamic_lookup -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -arch x86_64 build/temp.macosx-10.7-x86_64-3.6/convolve4.o -L/Users/huangsizhe/LIB/CONDA/anaconda/lib -o /Users/huangsizhe/WORKSPACE/Blog/Docs/Python/TutorialForPythonDataAnalysis/ipynbs/numpy/convolve4.cpython-36m-darwin.so
    clang: [0;1;35mwarning: [0mlibstdc++ is deprecated; move to libc++ with a minimum deployment target of OS X 10.9[0m



```python
from convolve4 import naive_convolve_4
```


```python
naive_convolve_4(f,g)
```




    array([[      0,       0,       1, ...,    2056,    1477,     792],
           [      0,     109,     329, ...,    8858,    6227,    3275],
           [    900,    2127,    3684, ...,   23106,   16050,    8349],
           ..., 
           [1850400, 3730389, 5639970, ..., 6230334, 4183464, 2106687],
           [1329300, 2678435, 4047407, ..., 4445402, 2983649, 1501849],
           [ 712800, 1435572, 2168317, ..., 2369524, 1589761,  799920]])




```python
%timeit -n10 naive_convolve_4(f,g)
```

    1.83 ms ± 275 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)


又提高了将近1倍的速度

从最原始的版本到最终的版本我们的的代码效率对比786ms/1.83ms,几乎是400倍的效率提高
