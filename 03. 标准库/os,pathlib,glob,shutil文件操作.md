## 文件操作库

pathlib、glob、shutil、os均用于在Python中进行文件和目录操作。

1. **`os`：** 提供通用的操作系统接口，包括底层的文件和目录操作函数，适用于与操作系统交互的一般性功能。

2. **`pathlib`：** 提供面向对象的路径表示和操作，使路径处理更直观，适合简洁的路径构建和解析。
  
3. **`glob`：** 用于模式匹配文件路径，支持通配符，适合查找符合特定模式的文件列表。

4. **`shutil`：** 提供高级的文件和目录操作工具，包括复制、移动、删除等，适用于复杂的文件系统操作。

# 1. os库

## path子模块

`os.path`是Python标准库`os`的一个子模块，提供了许多用于处理 **文件路径** 的函数。

1. **`os.path.join()`**：用于连接多个路径组件成一个完整路径。它会根据操作系统的规范来自动添加适当的路径分隔符。

   ```python
   #--run--
   import os

   path = os.path.join("folder", "subfolder", "file.txt")
   # On Windows: folder\subfolder\file.txt
   # On Unix-like systems: folder/subfolder/file.txt

   print(path)
   ```

2. **`os.path.abspath()`**：返回一个路径的绝对路径表示。

   ```python
   import os

   absolute_path = os.path.abspath("relative_path/file.txt")
   ```

3. **`os.path.dirname()`**：返回路径的目录部分。

   ```python
   import os

   directory = os.path.dirname("/path/to/file.txt")
   ```

4. **`os.path.basename()`**：返回路径的基本文件名部分。

   ```python
   import os

   filename = os.path.basename("/path/to/file.txt")
   ```

5. **`os.path.exists()`**：检查路径是否存在。

   ```python
   import os

   exists = os.path.exists("/path/to/file.txt")
   ```

6. **`os.path.isfile()`** 和 **`os.path.isdir()`**：分别检查路径是否是文件和目录。

   ```python
   import os

   is_file = os.path.isfile("/path/to/file.txt")
   is_directory = os.path.isdir("/path/to/directory")
   ```

7. **`os.path.splitext()`**：将路径分割成文件名和扩展名部分。

   ```python
   import os

   filename, extension = os.path.splitext("/path/to/file.txt")
   ```

8. **`os.path.normpath()`**：将路径规范化，处理不同操作系统的路径表示差异。

   ```python
   import os

   normalized_path = os.path.normpath("/path/to/../file.txt")
   ```

9. **`os.path.getsize()`**：获取文件大小（字节数）。

   ```python
   import os

   size = os.path.getsize("/path/to/file.txt")
   ```

10. **`os.path.join()`**：拼接多个路径部分，避免手动处理路径分隔符。

    ```python
    import os

    full_path = os.path.join("folder", "subfolder", "file.txt")
    ```

## 常用函数

#### os.getcwd()

返回当前目录的绝对路径。

#### os.mkdir()
创建新目录。

```python
import os

# 指定要创建的新目录的路径
new_directory_path = "/path/to/new_directory"

# 使用os.mkdir()函数创建新目录
try:
    os.mkdir(new_directory_path)
    print(f"成功创建目录：{new_directory_path}")
except OSError as e:
    print(f"创建目录时出错：{e}")
```

- 不能越级创建目录。

    ```bash
    >>> os.mkdir("one")
    >>> os.mkdir("one/two/three/")
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    FileNotFoundError: [Errno 2] No such file or directory: 'one/two/three/'
    >>> os.mkdir("one/two/")
    ```

#### os.makedirs()
递归地创建多层、多个目录。

```python
>>> new_dir="one/two/three/four/five"
>>> os.makedirs(new_dir)
```

#### os.chdir()
切换工作目录。

#### os.system()
在函数中输入命令并执行。
```python
#--run--
import os
os.system('pwd')
```

#### os.rename

#### os.listdir

# 2. pathlib

面向路径操作的对象化表示，包括 Path 类，使得路径的处理更加 Pythonic。
```python
from pathlib import Path

path = Path("/path/to/directory")
file_path = path / "file.txt"
```

## 1. **`Path` 类：** `pathlib` 的核心类，表示文件系统路径。

```python
from pathlib import Path

# 创建路径对象
path = Path('/path/to/directory')

# 获取绝对路径
abs_path = path.resolve()

# 连接路径
new_path = path / 'file.txt'

# 获取路径的字符串表示
str_path = str(path)
```

`Path`对象是Python标准库中`pathlib`模块提供的一个类，用于处理文件路径和目录路径。它提供了许多属性和方法，用于执行文件系统操作。

### 常用属性

1. **`Path.parts`**：
   - 返回路径的各个部分作为元组。

2. **`Path.parent`**：
   - 返回路径的父目录。

3. **`Path.name`**：
   - 返回路径的最后一个组成部分，即文件名或目录名。

4. **`Path.suffix`**：
   - 返回路径的后缀名，包括`.`。

5. **`Path.stem`**：
   - 返回路径的文件名部分，不包括后缀。

6. **`Path.exists()`**：
   - 返回布尔值，指示路径是否存在。

7. **`Path.is_file()`**：
   - 返回布尔值，指示路径是否是文件。

8. **`Path.is_dir()`**：
   - 返回布尔值，指示路径是否是目录。

9. **`Path.stat()`**：
   - 返回包含文件或目录的统计信息的`os.stat_result`对象。

10. **`Path.cwd()`**：
    - 返回当前工作目录的`Path`对象。

11. **`Path.home()`**：
    - 返回当前用户的主目录的`Path`对象。

### 常用方法

1. **`Path.joinpath(*paths)`**：
   - 将路径与其他路径连接，并返回新的`Path`对象。

2. **`Path.resolve()`**：
   - 返回路径的绝对路径。

3. **`Path.iterdir()`**：
   - 返回目录中所有条目的迭代器。

4. **`Path.glob(pattern)`**：
   - 返回与指定模式匹配的所有路径的生成器。

5. **`Path.mkdir(mode=0o777, parents=False, exist_ok=False)`**：
   - 创建目录。

6. **`Path.rename(target)`**：
   - 重命名路径，参数是完整的绝对路径。
   - 1、所在目录的Path对象 / 新命名：`i.rename(directory / 'sdfefd.txt')`
   - 2、绝对路径的字符串

7. **`Path.unlink()`**：
   - 删除文件或空目录。

8. **`Path.rmdir()`**：
   - 删除空目录。

9. **`Path.touch(mode=0o666, exist_ok=True)`**：
   - 创建文件。

10. **`Path.read_text(encoding=None, errors=None)`**：
    - 以文本模式读取文件的内容。

11. **`Path.write_text(data, encoding=None, errors=None)`**：
    - 以文本模式写入数据到文件。

这些属性和方法使得`Path`对象非常方便地处理文件和目录路径，简化了对文件系统的操作。

2. **`exists()` 方法：** 判断路径是否存在。该函数也可以判断某目录下是否存在某文件。

   ```python
   if path.exists():
       print("Path exists!")
   ```

3. **`is_file()` 和 `is_dir()` 方法：** 判断路径是否为文件或目录。

   ```python
   if path.is_file():
       print("It's a file!")
   elif path.is_dir():
       print("It's a directory!")
   ```

4. **`mkdir()` 和 `mkdir(parents=True, exist_ok=True)` 方法：** 创建目录。

   ```python
   path.mkdir()  # 创建单个目录
   path.mkdir(parents=True, exist_ok=True)  # 创建多层目录，如果目录已存在不报错
   ```

5. **`rglob(pattern)` 方法：** 递归查找文件。

   ```python
   for file_path in path.rglob('*.txt'):
       print(file_path)
   ```

6. **`iterdir()` 方法：** 遍历目录中的文件和子目录。

   ```python
   for item in path.iterdir():
       print(item)
   ```

7. **`read_text()` 和 `write_text()` 方法：** 读取和写入文本文件。

   ```python
   content = path.read_text()
   path.write_text("Hello, World!")
   ```

8. **`rename(target)` 方法：** 重命名文件或目录。

   ```python
   new_path = path.with_name('new_name.txt')  # 构造新路径
   path.rename(new_path)
   ```

9. **`stat()` 方法：** 获取文件或目录的状态信息。

   ```python
   file_info = path.stat()
   print(f"Size: {file_info.st_size} bytes")
   ```

10. **`glob(pattern)` 方法：** 查找匹配指定模式的文件。

    ```python
    for file_path in path.glob('*.txt'):
        print(file_path)
    ```

这只是 `pathlib` 的一小部分功能，更多功能和方法可以查阅[官方文档](https://docs.python.org/3/library/pathlib.html)。 `pathlib` 提供了一种更直观和面向对象的路径操作方式，使得处理文件系统路径更加方便和可读。

# 3. glob

# 4. shutil