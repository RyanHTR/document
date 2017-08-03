本文主要介绍caffe安装及caffe安装、使用过程中可能遇到的问题


# 1.安装
## 1.1 安装步骤
## 1.2 可能遇到的坑

deeplab_v2编译提示
```
error: function “atomicAdd(double *, double)” has already been defined
```

解决办法：
找到 common.cuh 文件并修改为下列内容：
```
// Copyright 2014 George Papandreou

#ifndef CAFFE_COMMON_CUH_
#define CAFFE_COMMON_CUH_

#include <cuda.h>
  #if !defined(__CUDA_ARCH__) || __CUDA_ARCH__ >= 600

  #else
// CUDA: atomicAdd is not defined for doubles
static __inline__ __device__ double atomicAdd(double *address, double val) {
  unsigned long long int* address_as_ull = (unsigned long long int*)address;
  unsigned long long int old = *address_as_ull, assumed;
  if (val==0.0)
    return __longlong_as_double(old);
  do {
    assumed = old;
    old = atomicCAS(address_as_ull, assumed, __double_as_longlong(val +__longlong_as_double(assumed)));
  } while (assumed != old);
  return __longlong_as_double(old);
}
  #endif
#endif
```

deeplab_v2编译提示
```
src/caffe/layers/window_data_layer.cpp:26:11: error: ‘const int CV_LOAD_IMAGE_COLOR’ redeclared as different kind of symbol
 const int CV_LOAD_IMAGE_COLOR = cv::IMREAD_COLOR;
```
解决办法：
将 window_data_layer.cpp 中25-27行内容，即
```
#if CV_VERSION_MAJOR == 3
const int CV_LOAD_IMAGE_COLOR = cv::IMREAD_COLOR;
#endif
```
注释掉即可

# 2.使用
