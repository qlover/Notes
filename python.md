


Python 操作文件时，我们一般要先判断指定的文件或目录是否存在，不然容易产生异常。

os.path.exists() 方法来检测文件是否存在：
```py
import os.path
os.path.isfile(fname)
```
如果你要确定他是文件还是目录，从 Python 3.4 开始可以使用 pathlib 

检测是否为一个目录：
```py
if my_file.is_dir():
    # 指定的目录存在
```

如果要检测路径是一个文件或目录可以使用 exists() 方法：
```py
if my_file.exists():
    # 指定的文件或目录存在
```

在 try 语句块中你可以使用 resolve() 方法来判断：
```py
try:
    my_abs_path = my_file.resolve()
except FileNotFoundError:
    # 不存在
else:
    # 存在
```