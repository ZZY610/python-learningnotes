# test
```python
#--run--
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation

# 傅里叶变换函数
def fourier_transform(x, t, n_terms):
    T = t[-1] - t[0]
    omega = 2 * np.pi / T
    result = np.zeros_like(t, dtype=complex)
    for n in range(-n_terms, n_terms + 1):
        cn = np.trapz(x * np.exp(-1j * n * omega * t), t) / T
        result += cn * np.exp(1j * n * omega * t)
    return result

# 定义时间和信号
t = np.linspace(0, 2 * np.pi, 1000)
x = np.sin(t) + 0.5 * np.sin(3 * t)

# 初始化绘图
fig, (ax1, ax2) = plt.subplots(2, 1)
line1, = ax1.plot(t, x, label='Original signal')
line2, = ax2.plot(t, np.real(x), label='Reconstructed signal')
ax1.legend()
ax2.legend()

# 更新函数
def update(frame):
    n_terms = frame // 10
    x_reconstructed = fourier_transform(x, t, n_terms)
    line2.set_ydata(np.real(x_reconstructed))
    ax2.set_title(f"Fourier Transform with {n_terms} terms")
    return line1, line2,

# 创建动画
ani = FuncAnimation(fig, update, frames=range(0, 100, 10), blit=True)
plt.show()

```

```python
#--run--
import matplotlib.pyplot as plt
import numpy as np


fig = plt.figure()
ax = fig.add_subplot(projection='3d')

# Make data
u = np.linspace(0, 2 * np.pi, 100)
v = np.linspace(0, np.pi, 100)
x = 10 * np.outer(np.cos(u), np.sin(v))
y = 10 * np.outer(np.sin(u), np.sin(v))
z = 10 * np.outer(np.ones(np.size(u)), np.cos(v))

# Plot the surface
ax.plot_surface(x, y, z)

# Set an equal aspect ratio
ax.set_aspect('auto')

plt.show()

```

```python
#--run--
from numpy import sin, cos
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from collections import deque

G = 9.8  # acceleration due to gravity, in m/s^2
L1 = 1.0  # length of pendulum 1 in m
L2 = 1.0  # length of pendulum 2 in m
L = L1 + L2  # maximal length of the combined pendulum
M1 = 1.0  # mass of pendulum 1 in kg
M2 = 1.0  # mass of pendulum 2 in kg
t_stop = 2.5  # how many seconds to simulate
history_len = 500  # how many trajectory points to display


def derivs(t, state):
    dydx = np.zeros_like(state)

    dydx[0] = state[1]

    delta = state[2] - state[0]
    den1 = (M1+M2) * L1 - M2 * L1 * cos(delta) * cos(delta)
    dydx[1] = ((M2 * L1 * state[1] * state[1] * sin(delta) * cos(delta)
                + M2 * G * sin(state[2]) * cos(delta)
                + M2 * L2 * state[3] * state[3] * sin(delta)
                - (M1+M2) * G * sin(state[0]))
               / den1)

    dydx[2] = state[3]

    den2 = (L2/L1) * den1
    dydx[3] = ((- M2 * L2 * state[3] * state[3] * sin(delta) * cos(delta)
                + (M1+M2) * G * sin(state[0]) * cos(delta)
                - (M1+M2) * L1 * state[1] * state[1] * sin(delta)
                - (M1+M2) * G * sin(state[2]))
               / den2)

    return dydx

# create a time array from 0..t_stop sampled at 0.02 second steps
dt = 0.01
t = np.arange(0, t_stop, dt)

# th1 and th2 are the initial angles (degrees)
# w10 and w20 are the initial angular velocities (degrees per second)
th1 = 120.0
w1 = 0.0
th2 = -10.0
w2 = 0.0

# initial state
state = np.radians([th1, w1, th2, w2])

# integrate the ODE using Euler's method
y = np.empty((len(t), 4))
y[0] = state
for i in range(1, len(t)):
    y[i] = y[i - 1] + derivs(t[i - 1], y[i - 1]) * dt

# A more accurate estimate could be obtained e.g. using scipy:
#
#   y = scipy.integrate.solve_ivp(derivs, t[[0, -1]], state, t_eval=t).y.T

x1 = L1*sin(y[:, 0])
y1 = -L1*cos(y[:, 0])

x2 = L2*sin(y[:, 2]) + x1
y2 = -L2*cos(y[:, 2]) + y1

fig = plt.figure(figsize=(5, 4))
ax = fig.add_subplot(autoscale_on=False, xlim=(-L, L), ylim=(-L, 1.))
ax.set_aspect('equal')
ax.grid()

line, = ax.plot([], [], 'o-', lw=2)
trace, = ax.plot([], [], '.-', lw=1, ms=2)
time_template = 'time = %.1fs'
time_text = ax.text(0.05, 0.9, '', transform=ax.transAxes)
history_x, history_y = deque(maxlen=history_len), deque(maxlen=history_len)


def animate(i):
    thisx = [0, x1[i], x2[i]]
    thisy = [0, y1[i], y2[i]]

    if i == 0:
        history_x.clear()
        history_y.clear()

    history_x.appendleft(thisx[2])
    history_y.appendleft(thisy[2])

    line.set_data(thisx, thisy)
    trace.set_data(history_x, history_y)
    time_text.set_text(time_template % (i*dt))
    return line, trace, time_text


ani = animation.FuncAnimation(
    fig, animate, len(y), interval=dt*1000, blit=True)
plt.show()
```
```python

# --run--

import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation

# 太阳系八大行星数据（名称、轨道周期、轨道半径、半径、颜色）
planets = [
    ("Mercury", 88, 57.9, 1, "gray"),
    ("Venus", 224.7, 108.2, 1.5, "yellow"),
    ("Earth", 365, 149.6, 2, "blue"),
    ("Mars", 687, 227.9, 1, "red"),
    ("Jupiter", 4332, 778.3, 4, "orange"),
    ("Saturn", 10759, 1427, 3, "brown"),
    ("Uranus", 30687, 2871, 2, "cyan"),
    ("Neptune", 60190, 4498, 2, "darkblue")
]

# 初始化绘图
fig, ax = plt.subplots()
ax.set_aspect('equal')
ax.set_xlim(-5000, 5000)
ax.set_ylim(-5000, 5000)

sun = plt.Circle((0, 0), 6, color='yellow', label="Sun")
ax.add_artist(sun)

planet_artists = []
planet_trails = []
for name, period, distance, radius, color in planets:
    planet = plt.Circle((0, 0), radius, color=color, label=name)
    ax.add_artist(planet)
    planet_artists.append(planet)
    
    trail, = plt.plot([], [], color=color, linewidth=0.5)
    planet_trails.append(trail)

# 更新函数
def update(frame):
    for i, (name, period, distance, radius, color) in enumerate(planets):
        t = frame / 100
        omega = 2 * np.pi / period
        x = distance * np.cos(omega * t)
        y = distance * np.sin(omega * t)
        planet_artists[i].center = (x, y)
        
        # 更新轨迹
        x_trail, y_trail = planet_trails[i].get_data()
        x_trail = np.append(x_trail, x)
        y_trail = np.append(y_trail, y)
        planet_trails[i].set_data(x_trail[-300:], y_trail[-300:])
    
    return planet_artists + planet_trails

# 创建动画
ani = FuncAnimation(fig, update, frames=range(100000), interval=20, blit=True)

# 显示图例
handles, labels = ax.get_legend_handles_labels()
ax.legend(handles, labels, loc="upper right", fontsize="small")

# 保存为GIF
# ani.save("solar_system.gif", writer="imagemagick", fps=30)

plt.show()

```
