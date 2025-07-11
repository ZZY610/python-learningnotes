# tkinter GUI库
`Tkinter` 是 Python 中的标准 GUI（图形用户界面）库，它提供了创建窗口、按钮、标签等 GUI 元素的组件。

## 1. 创建程序的基本步骤
- 导入模块
- 创建主窗口对象并配置控件
- 添加事件处理函数
- 启动主事件循环

## 2. 基础组件
### 2.1 ttk
`ttk`（Themed Tkinter）是 Tkinter 的一个模块，它提供了一套现代化的控件集合，这些控件在外观和行为上与操作系统的本地控件一致。`ttk` 的目的是让 Tkinter 创建的应用程序看起来更加现代化和专业化。

Tkinter 中的标准控件在不同的操作系统下可能有不同的外观，而 `ttk` 模块解决了这个问题，它提供的控件在不同的操作系统下都具有统一的外观，从而使得应用程序更加统一和美观。

`ttk` 中包含了一系列控件，比如按钮、标签、复选框、单选框、文本框、下拉列表框、进度条等，它们的使用方式与 Tkinter 中的标准控件类似，但是提供了更多的样式设置和主题配置选项。

总的来说，`ttk` 是 Tkinter 的一个增强版，提供了更加现代化和美观的控件，能够让 Tkinter 创建的应用程序在不同的操作系统下都具有一致的外观和行为。

### 2.2 菜单栏
在程序界面的上方增加菜单栏。
![Img](./FILES/tkinter%20GUI库.md/img-20240720233540.png)

```python
#--run--
# 创建菜单栏目
import tkinter as tk
root=tk.Tk()
menu_bar = tk.Menu(root)
root.config(menu=menu_bar)

# 添加菜单项
file_menu = tk.Menu(menu_bar, tearoff=0)
file_menu.add_command(label="New")
file_menu.add_command(label="Open")
file_menu.add_separator()
file_menu.add_command(label="Exit", command=root.quit)
menu_bar.add_cascade(label="File", menu=file_menu)
root.mainloop()
```

### 2.3 容器层级
在Tkinter中，容器组件用于组织和布局其他小部件（widgets），从而构建复杂的用户界面。常用的容器组件包括：

1. **Frame（框架）**：
   - Frame是Tkinter中最常用的容器组件之一，用于将其他小部件组织在一起，并提供布局和边框支持。
   - 可以设置背景颜色、边框样式等属性。
   - 通常用于划分界面、创建布局结构等。

2. **Toplevel（顶级窗口）**：
   - Toplevel是一个独立的顶级窗口，可以包含其他小部件。
   - 可以作为独立的应用程序窗口或模态对话框使用。

3. **PanedWindow（分割窗口）**：
   - PanedWindow允许用户通过拖动分隔条来调整子窗口的大小。
   - 可以水平或垂直分割，并包含一个或多个子窗口。

4. **Notebook（选项卡）**：
   - Notebook允许在同一区域显示多个页面，并通过选项卡切换页面。
   - 每个选项卡页通常包含不同的内容或功能。

5. **Labelframe（标签框架）**：
   - Labelframe是一个带有标题的框架，用于将相关小部件组织在一起。
   - 常用于创建分组框架，以便对界面进行分组和组织。

6. **Canvas（画布）**：
   - Canvas是一个可绘制图形和小部件的区域，可以用于创建自定义图形、绘图和交互式元素。
   - 可以绘制图形对象、文本、图像等，并响应用户输入事件。

7. **ScrolledWindow（滚动窗口）**：
   - ScrolledWindow是一个带有滚动条的窗口，用于显示超出可见区域的内容。
   - 可以包含其他小部件，并在需要时自动添加滚动条。

8. **Labelframe（标签框架）**：
   - Labelframe是一个带有标题的框架，用于将相关小部件组织在一起。
   - 常用于创建分组框架，以便对界面进行分组和组织。

以上是常见的Tkinter容器组件，它们可以组合使用，以实现复杂的用户界面布局和交互效果。
### 2.4 主窗口
```python
#--run--
import tkinter as tk

root = tk.Tk()  # 创建主窗口对象
root.title("My GUI")  # 设置窗口标题
```

#### 指定窗口的大小和位置
root.geometry(width x width +a+b)

```python
geometry(400x400+100+100)
# 注意是x不是乘号
```

### 2.5 布局管理器
在 Tkinter 中，有两种常用的布局管理器：`pack` 和 `grid`。它们分别用于在窗口中排列和定位控件。

#### 1. `pack` 布局管理器：
`pack` 布局管理器会按照添加控件的顺序，逐个排列它们。它提供了一些选项来控制控件的位置和大小。一般情况下，`pack` 布局适用于简单的排列方式。

##### 主要方法：
- `pack(side=...)`：指定控件的放置方向，可选的值包括 `TOP`、`BOTTOM`、`LEFT` 和 `RIGHT`。
- `pack(fill=...)`：指定控件沿着所在容器的方向填充，可选的值包括 `BOTH`、`X` 和 `Y`。
- `pack(expand=True)`：设置控件是否可以在空间允许的情况下扩展。
- `pack(anchor=...)`：指定控件相对于父容器的对齐方式，可选的值包括 `N`、`S`、`W`、`E`、`NW`、`NE`、`SW` 和 `SE`。

##### 示例：
```python
label1.pack(side="top", fill="x")
label2.pack(side="left", fill="y", expand=True)
button1.pack(side="bottom", anchor="e")
```

#### 2. `grid` 布局管理器：
`grid` 布局管理器允许使用网格来排列控件，类似于表格布局。通过指定行和列来放置控件，可以更灵活地控制控件的位置和大小。一般情况下，`grid` 布局适用于需要更精确控制布局的情况。

##### 主要方法：
- `grid(row=..., column=...)`：指定控件所在的行和列。
- `grid(rowspan=..., columnspan=...)`：指定控件占据的行数和列数。
- `grid(sticky=...)`：设置控件在所在的网格中的对齐方式，可选的值包括 `N`、`S`、`W`、`E`、`NW`、`NE`、`SW` 和 `SE`。

##### 示例：
```python
label1.grid(row=0, column=0, sticky="w")
label2.grid(row=1, column=0, rowspan=2, sticky="nsew")
button1.grid(row=2, column=1, sticky="e")
```

#### 区别和选择：
- `pack` 布局管理器简单易用，适合用于快速布局。
- `grid` 布局管理器更加灵活，适用于需要精确控制布局的情况。

### 2.5 标签
![Img](./FILES/tkinter%20GUI库.md/img-20240720233659.png)

```python
#--run--
import tkinter as tk
root = tk.Tk()  # 创建主窗口对象
label = tk.Label(root, text="Hello, Tkinter!")  # 创建标签对象
label.pack()  # 将标签放置在窗口中，pack() 是一种简单的布局方法
root.mainloop()
```
#### 创建标签
tk.Label()


### 2.6 按钮

```python
def button_clicked():
    label.config(text="Button Clicked!")

button = tk.Button(root, text="Click Me", command=button_clicked)  # 创建按钮对象
button.pack()

# Button方法第一个参数是标签Frame。
```

### 2.7 输入框（Entry）

```python
entry = tk.Entry(root)  # 创建输入框对象
entry.pack()
```

#### 获取输入框的值

```python
def get_entry_value():
    value = entry.get()
    label.config(text=f"Entry Value: {value}")

submit_button = tk.Button(root, text="Submit", command=get_entry_value)
submit_button.pack()
```

### 2.8 创建列表框（Listbox）

```python
listbox = tk.Listbox(root)  # 创建列表框对象
listbox.pack()

# 向列表框中添加项
for item in ["Item 1", "Item 2", "Item 3"]:
    listbox.insert(tk.END, item)
```

### 2.9 下拉列表框
`ttk.Combobox` 是 Tkinter 中的一个组合框控件，它是一个下拉列表框，用户可以通过点击下拉箭头选择其中的一项，也可以手动输入内容。`ttk.Combobox` 继承自 `ttk.Widget`，因此具有许多通用的方法和属性，同时还有一些特有的属性和事件。

以下是 `ttk.Combobox` 的一些常用属性和方法：

- `values`：设置下拉列表框中的选项。可以传入一个列表或元组，例如 `values=['选项1', '选项2', '选项3']`。
- `textvariable`：将一个 `StringVar` 变量与下拉列表框绑定，以便在程序中获取或设置用户选择的值。
- `get()`：获取用户当前选择的值。
- `set(value)`：设置下拉列表框的当前选项。
- `state`：设置下拉列表框的状态，可以为 `readonly`（只读，不可编辑）或 `normal`（可编辑）。
- `bind(event, handler)`：绑定事件处理函数，比如 `'<ComboboxSelected>'` 事件表示用户选择了下拉列表框中的某一项。

#### 参数说明

- master：父容器，即组合框所属的容器控件。
- textvariable：用于绑定组合框的变量，当用户选择或输入值时，该变量的值会自动更新。
- values：组合框中的选项列表，可以是字符串列表或其他可迭代对象。
- state：指定组合框的状态，可选值有 "readonly"、"normal" 和 "disabled"，默认为 "normal"。
- style：指定组合框的样式。
- width：组合框的宽度，以字符为单位，默认为标准宽度。
- height：组合框的高度，以字符为单位，默认为标准高度。
- font：指定组合框中文本的字体。
- foreground：组合框中文本的前景色（颜色）。
- background：组合框的背景色。
- borderwidth：组合框的边框宽度。
- relief：组合框的边框样式，可选值有 "flat"、"raised"、"sunken"、"ridge" 和 "solid"。
- takefocus：指定是否可以通过 Tab 键切换到该组合框，布尔值，默认为 True。
- exportselection：指定是否允许选择的内容被复制到剪贴板，布尔值，默认为 True。
- validate：指定组合框的验证方式，可选值有 "key"、"focus"、"all" 和 None，默认为 None。
- validatecommand：用于指定验证组合框值的函数或命令。
- invalidcommand：指定在验证失败时执行的命令或函数。
- postcommand：在打开下拉列表后执行的命令或函数。
- readonlybackground：当组合框处于只读模式时的背景色。
- text：显示在组合框上的文本。
- xscrollcommand：与水平滚动条关联的命令或函数。
- yscrollcommand：与垂直滚动条关联的命令或函数。
```python
#--run--
import tkinter as tk
from tkinter import ttk

root = tk.Tk()

# 创建一个字符串变量，用于绑定组合框的值
combo_var = tk.StringVar()

# 创建组合框，绑定到 combo_var，并设置下拉选项
combo = ttk.Combobox(root, textvariable=combo_var, values=['Option 1', 'Option 2', 'Option 3'])

# 设置组合框的宽度和样式
combo.config(width=20, style='TCombobox')

# 设置组合框的状态为只读
combo.state(['readonly'])

# 设置组合框的字体和前景色
combo.config(font=('Arial', 12), foreground='blue')

# 将组合框放置到窗口中
combo.pack(pady=10)

root.mainloop()
```

### 2.10 树状视图
`tkinter` 的 `Treeview` 组件是一个强大的小部件，用于以树状结构展示数据。你可以在 `Treeview` 中显示层级结构的数据，支持列和行，并允许用户进行排序、选择等操作。以下是 `Treeview` 组件的一些基本用法和常见操作。

#### 1. 创建 Treeview

要使用 `Treeview`，你需要从 `tkinter.ttk` 模块导入它。然后可以创建一个 `Treeview` 实例，指定它的父容器（如 `Frame` 或 `Tk` 窗口）。

```python
#--run--
from tkinter import Tk
from tkinter import ttk

root = Tk()
tree = ttk.Treeview(root)
tree.pack()
root.mainloop()
```

#### 2. 配置列

通过 `columns` 参数配置 `Treeview` 的列。列的定义不包括树状结构的列（根节点），但你可以通过 `heading` 方法设置列的标题。

```python
#--run--
from tkinter import Tk
from tkinter import ttk

root = Tk()
tree = ttk.Treeview(root, columns=('col1', 'col2'))
tree.heading('#0', text='Tree Column')  # 树状列标题
tree.heading('col1', text='Column 1')
tree.heading('col2', text='Column 2')
tree.pack()
root.mainloop()
```

#### 3. 插入数据

使用 `insert` 方法向 `Treeview` 中插入数据。`insert` 方法可以用于插入根节点（没有父节点）或者子节点（有父节点）。

```python
# 插入根节点
tree.insert('', 'end', text='Root Node', values=('Value1', 'Value2'))

# 插入子节点
parent_id = tree.insert('', 'end', text='Parent Node', values=('ParentValue1', 'ParentValue2'))
tree.insert(parent_id, 'end', text='Child Node', values=('ChildValue1', 'ChildValue2'))
```

#### 4. 获取和操作节点

你可以使用 `get_children` 方法获取当前节点的所有子节点，使用 `item` 方法获取或设置节点的数据。

```python
# 获取所有子节点
children = tree.get_children()

# 获取某个节点的数据
node_data = tree.item(some_node_id)
```

#### 5. 删除节点

使用 `delete` 方法删除节点及其子节点。

```python
tree.delete(node_id)  # 删除指定的节点
```

#### 6. 修改节点

使用 `item` 方法修改节点的数据，传入新的 `text` 或 `values` 参数。

```python
tree.item(node_id, text='New Text', values=('New Value1', 'New Value2'))
```

#### 7. 排序

`Treeview` 组件本身不提供排序功能，你可以通过获取数据、排序，然后重新插入数据来实现排序。

```python
def sort_tree(tree, col, reverse):
    items = [(tree.item(child)['values'], child) for child in tree.get_children('')]
    items.sort(reverse=reverse)
    for index, (values, child) in enumerate(items):
        tree.move(child, '', index)
```

#### 完整示例

以下是一个完整的示例，演示了如何创建 `Treeview` 组件、插入数据、修改节点、删除节点等操作：

```python
#--run--
import tkinter as tk
from tkinter import ttk

def add_item():
    tree.insert('', 'end', text='New Node', values=('New Value1', 'New Value2'))

def update_item():
    selected_item = tree.selection()[0]
    tree.item(selected_item, text='Updated Node', values=('Updated Value1', 'Updated Value2'))

def delete_item():
    selected_item = tree.selection()[0]
    tree.delete(selected_item)

root = tk.Tk()
tree = ttk.Treeview(root, columns=('col1', 'col2'))
tree.heading('#0', text='Tree Column')
tree.heading('col1', text='Column 1')
tree.heading('col2', text='Column 2')

tree.pack()

ttk.Button(root, text='Add Item', command=add_item).pack()
ttk.Button(root, text='Update Item', command=update_item).pack()
ttk.Button(root, text='Delete Item', command=delete_item).pack()

root.mainloop()
```

这个示例创建了一个 `Treeview` 组件，添加了三列，插入了一个根节点，并提供了按钮来添加、更新和删除节点。
### 2.11 创建滚动条（Scrollbar）：

```python
scrollbar = tk.Scrollbar(root, command=listbox.yview)  # 创建垂直滚动条对象
listbox.config(yscrollcommand=scrollbar.set)  # 关联列表框和滚动条
scrollbar.pack(side=tk.RIGHT, fill=tk.Y)  # 将滚动条放置在窗口右侧
```

### 2.12 选项卡tab页
```python
#--run--
import tkinter as tk
root = tk.Tk()
tabs = tk.Notebook(root)

```
### 2.13 启动主事件循环

```python
root.mainloop()  # 启动 Tkinter 主事件循环
```

## 变量类型
`StringVar()` 和 `DoubleVar()` 是 Tkinter 中的变量类型，用于与界面中的控件绑定，并且能够跟踪控件的值的变化。

- `StringVar()` 用于绑定字符串类型的控件，比如 `Entry`、`Label`、`Button` 等控件，可以获取或设置这些控件的文本内容。
- `DoubleVar()` 用于绑定浮点数类型的控件，通常用于与 `Entry` 控件绑定，可以获取或设置这些控件中的浮点数值。

当你使用 `textvariable` 参数将这些变量绑定到控件时，控件的值将会与变量的值保持同步。如果你修改了变量的值，那么绑定的控件也会相应地更新，反之亦然。这样的设计可以方便地在界面和程序逻辑之间进行数据交换。

## 组件示例代码
![Img](./FILES/tkinter%20GUI库.md/img-20240720233339.png)

```python
#--run--

#以下是一个包含了尽可能多常用容器组件层级的示例代码：
import tkinter as tk
from tkinter import ttk

# 创建主窗口
root = tk.Tk()
root.geometry("600x400")
root.title("容器组件层级示例")

# 创建 Frame 1
frame1 = tk.Frame(root, bg="lightblue", bd=5, relief="sunken")
frame1.pack(side="top", fill="both", expand=True)

# Label 1 在 Frame 1 内
label1 = tk.Label(frame1, text="Frame 1", bg="lightblue", font=("Arial", 12, "bold"))
label1.pack(side="top", fill="x")

# 创建 Frame 2，位于 Frame 1 内
frame2 = tk.Frame(frame1, bg="lightgreen", bd=5, relief="ridge")
frame2.pack(side="left", fill="both", expand=True)

# Label 2 在 Frame 2 内
label2 = tk.Label(frame2, text="Frame 2", bg="lightgreen", font=("Arial", 12, "bold"))
label2.pack(side="top", fill="x")

# 创建 Notebook，位于 Frame 2 内
notebook = ttk.Notebook(frame2)
notebook.pack(side="left", fill="both", expand=True)

# Tab 1 在 Notebook 内
tab1 = tk.Frame(notebook)
notebook.add(tab1, text="Tab 1")

# Label 3 在 Tab 1 内
label3 = tk.Label(tab1, text="Tab 1 Content", font=("Arial", 12))
label3.pack(side="top", pady=10)

# 创建 Frame 3，位于 Frame 2 内
frame3 = tk.Frame(frame2, bg="lightyellow", bd=5, relief="raised")
frame3.pack(side="right", fill="both", expand=True)

# Label 4 在 Frame 3 内
label4 = tk.Label(frame3, text="Frame 3", bg="lightyellow", font=("Arial", 12, "bold"))
label4.pack(side="top", fill="x")

# 创建 Button，位于 Frame 3 内
button1 = tk.Button(frame3, text="Button 1", width=10)
button1.pack(side="top", pady=10)

# 创建 Frame 4，位于 Frame 1 的右侧
frame4 = tk.Frame(frame1, bg="lightcoral", bd=5, relief="groove")
frame4.pack(side="right", fill="both", expand=True)

# Label 5 在 Frame 4 内
label5 = tk.Label(frame4, text="Frame 4", bg="lightcoral", font=("Arial", 12, "bold"))
label5.pack(side="top", fill="x")

# 创建 Notebook，位于 Frame 4 内
notebook2 = ttk.Notebook(frame4)
notebook2.pack(fill="both", expand=True)

# Tab 2 在 Notebook2 内
tab2 = tk.Frame(notebook2)
notebook2.add(tab2, text="Tab 2")

# Label 6 在 Tab 2 内
label6 = tk.Label(tab2, text="Tab 2 Content", font=("Arial", 12))
label6.pack(side="top", pady=10)

# 创建下拉框，位于 Frame 4 内
options = ["Option 1", "Option 2", "Option 3"]
combo = ttk.Combobox(frame4, values=options)
combo.pack(side="top", pady=10)

# 创建列表框，位于 Frame 4 内
listbox = tk.Listbox(frame4)
for i in range(20):
    listbox.insert(tk.END, f"Item {i+1}")
listbox.pack(side="top", fill="both", expand=True)

# 创建滚动条，位于 Frame 4 内
scrollbar = tk.Scrollbar(frame4, orient="vertical", command=listbox.yview)
scrollbar.pack(side="right", fill="y")

# 关联滚动条和列表框
listbox.config(yscrollcommand=scrollbar.set)

root.mainloop()

```

这个示例中，我们包含了以下容器组件：
1. 主窗口 (`root`)
2. 第一个 `Frame` (`frame1`)
3. 第二个 `Frame` (`frame2`)，在第一个 `Frame` 内
4. 第三个 `Frame` (`frame3`)，也在第一个 `Frame` 内
5. 第四个 `Frame` (`frame4`)，在第一个 `Frame` 的右侧
6. `Notebook` (`notebook`)，在第二个 `Frame` 内
7. `Tab` (`tab1`)，在 `Notebook` 内

在每个容器中都添加了标签 (`Label`)，以展示容器的名称和层级关系。此外，还添加了一些按钮 (`Button`) 和输入框 (`Entry`) 作为容器中的内容。

