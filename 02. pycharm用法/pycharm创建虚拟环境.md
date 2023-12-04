# pycharm创建虚拟环境
在 PyCharm 中创建虚拟环境可以通过以下步骤：

1. 打开 PyCharm，并打开你的项目。

2. 在菜单栏中选择 "File"（文件） -> "Settings"（设置）。

3. 在设置窗口左侧，选择 "Project: your_project_name"，然后选择 "Python Interpreter"（Python 解释器）。

4. 在右侧的 "Python Interpreter" 部分，点击项目解释器右侧的齿轮图标，选择 "Add..."（添加）。

5. 在弹出的窗口中，选择 "Virtualenv Environment"（虚拟环境），然后配置虚拟环境的位置和解释器。

   - **Location：** 选择虚拟环境的存放位置。
   - **Base interpreter：** 选择已安装的 Python 解释器版本。
   - **Inherit global site-packages：** 可选项，表示是否继承全局的 site-packages。

6. 点击 "OK" 以创建虚拟环境。

7. 关闭设置窗口。

现在，你的 PyCharm 项目就配置了一个新的虚拟环境。在这个虚拟环境中，你可以安装项目所需的依赖包，而不会影响到全局 Python 环境。