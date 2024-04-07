###### 除主模块外，与主模块同级的模块也必须使用绝对导入。

###### 顶层模块不能使用相对导入。

###### 文件夹被python解释器视作package需满足两个条件：1. 有init.py文件；2. 不能作为顶层模块来执行该文件夹中的py文件

###### 主模块所在的包不会被python解释器视作package，在python解释器看来，主模块所在的包就是一个未知的父包

###### 若要导入顶层包更上层的模块，需要将文件路径加入sys.path

###### python文件加载的方式：顶层脚步、模块导入

- 顶层脚本：直接执行的程序，主模块
- 模块导入：在其他文件中使用import来导入

###### 当一个文件被当作一个主模块来执行，它的名字就是____main____, 当它被当作一个模块加载，那么它的名称就是文件名称，加上包名，以——及所有顶层包名。

###### 包目录中包含了____init____.py时， 当用import导入该目录时，会执行____init____.py里面的代码。

###### 包下的____init____.py中导入平级包时，应从其父包开始，如下：

```python
# __init__.py
from father_package.package import file01
from father_package.package import file02
```

###### 否则，会报出如下异常：

```python
ImportError: No module named 'subpackage_1'
```
