# MVC模型的基础知识

## 一、MVC 原则

| 层次 | 一句话职责 | 记住关键词 |
|---|---|---|
| **M** Model | **只负责数据**（存、取、结构） | 数据库、字段、关系 |
| **V** View | **只负责展示**（窗口、按钮、颜色） | Tkinter、布局、事件绑定 |
| **C** Controller | **只负责调度**（把用户的点击翻译成对 Model 的操作） | 业务函数、校验、流程 |

> 口诀：**“数据不碰界面，界面不碰数据，中间人做翻译。”**

---

## 二、示例

| 你的目录 | 对应 MVC | 具体职责 | 例子 |
|---|---|---|---|
| `models/` | **M** | 定义表结构 & 最小 CRUD | `User.create(username='alice')` |
| `views/` | **V** | 所有 Tkinter 窗口、控件、事件绑定 | 登录窗口、主界面按钮 |
| `services/` | **C** | 业务函数（校验、计算、事务） | `register(username, password)` |
| `config.py` | 全局常量 | 数据库路径、常量配置 | `DATABASE_PATH` |
| `main.py` | 启动器 | 建表一次 + 启动 Tkinter 主循环 | `root.mainloop()` |
| `resources/` | 静态资源 | 图标、图片、样式文件 | `logo.png` |
| `utils/` | 工具函数 | 通用小工具（格式转换、加密） | `format_currency()` |

---

## 三、一个按钮点击的完整 MVC 流程

1. **用户点击注册按钮**  
   → `views/register_view.py` 里的 `on_register()` 被触发。
2. **View 收集数据**  
   → `username = entry.get(); password = entry.get()`
3. **View 调用 Controller**  
   → `services.user_service.register(username, password)`
4. **Controller 做业务**  
   → 校验 → 调用 Model → `User.create(username=username, password=password)`
5. **Model 写库**  
   → `INSERT INTO user ...`
6. **Controller 返回结果**  
   → 成功 → View 弹 `messagebox.showinfo("注册成功")`

---

## 常见问题

| 错误做法 | 正确做法 |
|---|---|
| 在 `views/` 里写 `User.create()` | 交给 `services/` |
| 在 `models/` 里弹窗提示 | 交给 `views/` |
| 在 `main.py` 里写业务逻辑 | 只放启动代码 |

---

> **views 只干两件事：收集用户输入 + 触发 services；main.py 只做“启动 tkinter 主循环”。**

---


| 目录 | 职责 | 禁止做的事 |
|---|---|---|
| **views** | 1️⃣ 收集界面数据（Entry.get() 等）<br>2️⃣ 调用 services 函数 | ❌ 不写任何业务规则、不直接改数据库 |
| **services** | 执行业务逻辑（校验、计算、事务） | ❌ 不操作界面 |
| **models** | 纯数据定义 & 最小 CRUD | ❌ 不写业务 |
| **main.py** | 建表（一次）+ 启动 `tk.Tk().mainloop()` | ❌ 不做业务计算 |

---

所以你的 `main.py` 通常长这样：

```python
from scripts.init_tables import init_all      # 建表（第一次）
from views.main_window import MainWindow      # 你设计的根窗口类

if __name__ == '__main__':
    init_all()            # 只跑一次
    app = MainWindow()    # 里面是 tk.Tk()
    app.mainloop()        # 一直跑
```

UI 事件里只写：

```python
def on_register():
    username = entry.get()
    password = pwd.get()
    try:
        user_service.register(username, password)
    except ValueError as e:
        messagebox.showerror("错误", str(e))
```

这样就完全符合 MVC/MVVM 的桌面分层套路。