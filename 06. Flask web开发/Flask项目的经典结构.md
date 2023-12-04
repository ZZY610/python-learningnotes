# Flask项目的经典结构
在 Flask 项目中，经典的根目录结构通常包含以下主要部分：

1. **`app` 目录:**
   - `__init__.py`: 将目录标记为 Python 包，并可以执行一些初始化工作。
   - `routes.py`: 包含定义路由和视图函数的文件。
   - `models.py`: 包含应用程序的数据库模型。
   - `forms.py`: 定义表单。
   - `templates` 目录: 存放应用程序**模板**文件（HTML 文件）。
       - 模板目录可以使用app.template_folder方法进行更改（相对于app路径，比如 ../templates，可以不放在app目录下）。
   - `static` 目录: 存放应用程序的静态文件（如 CSS、JavaScript、图像等）。

2. **`config` 目录:**
   - `__init__.py`: 包含应用程序配置的初始化，如数据库配置、密钥等。

3. **`migrations` 目录:**
   - 包含数据库迁移脚本，用于在应用程序演进时更新数据库模型。

4. **`tests` 目录:**
   - 包含应用程序的单元测试。

5. **`venv` 目录:**
   - 虚拟环境，用于隔离项目的依赖关系。

6. **顶级文件:**
   - `config.py`: 存放应用程序的配置参数，例如数据库 URL、密钥等。
   - `run.py`: 启动应用程序的脚本。

示例项目结构：

```
/myflaskapp
|-- app
|   |-- __init__.py
|   |-- routes.py
|   |-- models.py
|   |-- views.py
|   |-- forms.py
|   |-- templates
|   |   |-- index.html
|   |   |-- ...
|   |-- static
|       |-- style.css
|       |-- ...
|-- config
|   |-- __init__.py
|   |-- config.py
|-- migrations
|   |-- ...
|-- tests
|   |-- ...
|-- venv
|-- .flaskenv
|-- config.py
|-- run.py
```
