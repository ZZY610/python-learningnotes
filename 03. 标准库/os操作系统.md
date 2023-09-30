# os库

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

### 常用函数

- `os.getcwd()`：返回当前目录的绝对路径。

- `os.mkdir()`：创建新目录。
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

- `os.makedirs()`：递归地创建多层、多个目录。

    ```python
    >>> new_dir="one/two/three/four/five"
    >>> os.makedirs(new_dir)
    ```

- `os.chdir()` ：切换工作目录。

- os.walk
