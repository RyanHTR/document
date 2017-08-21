本文主要介绍caffe安装及caffe安装、使用过程中可能遇到的问题


# 1.使用
## 1.1 使用步骤
## 1.2 可能遇到的坑
***
运行时提示
```
pb2.text_format.Merge(f.read(), self.solver_param)
AttributeError: 'module' object has no attribute 'text_format'
```

解决办法：

在./lib/fast_rcnn/train.py增加一行import google.protobuf.text_format 即可解决问题
