📌 Problem 1: Investigating the Range as a Function of the Angle of Projection
🌟 Motivation
Projectile motion, while seemingly simple, provides deep insights into physics principles. This exploration aims to analyze how the range of a projectile depends on its launch angle. The equations governing projectile motion, despite their accessibility, can model a broad range of real-world phenomena, from sports to space exploration.

📚 Theoretical Foundation
🔹 Deriving Equations of Motion
Consider a particle launched with an initial velocity 
𝑣
0
v 
0
​
  at an angle 
𝜃
θ with respect to the horizontal.

Initial velocity components:

Horizontal: 
𝑣
0
𝑥
=
𝑣
0
cos
⁡
(
𝜃
)
v 
0x
​
 =v 
0
​
 cos(θ)

Vertical: 
𝑣
0
𝑦
=
𝑣
0
sin
⁡
(
𝜃
)
v 
0y
​
 =v 
0
​
 sin(θ)

Using Newton's second law and neglecting air resistance:

Horizontal acceleration: 
𝑑
2
𝑥
𝑑
𝑡
2
=
0
dt 
2
 
d 
2
 x
​
 =0

Vertical acceleration: 
𝑑
2
𝑦
𝑑
𝑡
2
=
−
𝑔
dt 
2
 
d 
2
 y
​
 =−g

Integrating, we get the velocity components:

𝑣
𝑥
=
𝑣
0
𝑥
=
𝑣
0
cos
⁡
(
𝜃
)
v 
x
​
 =v 
0x
​
 =v 
0
​
 cos(θ)

𝑣
𝑦
=
𝑣
0
𝑦
−
𝑔
𝑡
=
𝑣
0
sin
⁡
(
𝜃
)
−
𝑔
𝑡
v 
y
​
 =v 
0y
​
 −gt=v 
0
​
 sin(θ)−gt

Integrating again, we obtain the position equations:

𝑥
(
𝑡
)
=
𝑣
0
cos
⁡
(
𝜃
)
⋅
𝑡
x(t)=v 
0
​
 cos(θ)⋅t

𝑦
(
𝑡
)
=
𝑣
0
sin
⁡
(
𝜃
)
⋅
𝑡
−
1
2
𝑔
𝑡
2
y(t)=v 
0
​
 sin(θ)⋅t− 
2
1
​
 gt 
2
 

🎯 Analyzing the Range
🔸 Range Formula
The range 
𝑅
R is the horizontal distance covered when the projectile returns to its initial height:

𝑅
=
𝑣
0
2
sin
⁡
(
2
𝜃
)
𝑔
R= 
g
v 
0
2
​
 sin(2θ)
​
 
Key insights:

The range is proportional to 
𝑣
0
2
v 
0
2
​
 .

The range is maximum when 
sin
⁡
(
2
𝜃
)
=
1
sin(2θ)=1 — this occurs at 
𝜃
=
45
∘
θ=45 
∘
 .

Gravity 
𝑔
g inversely affects the range.

🔧 Python Simulation and Visualization
Below is the Python code to simulate and visualize projectile motion for different launch angles.

📁 Code Implementation
python
Copy
Edit
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation

# 🌟 =======================================================
# 🌟 Problem 1: Investigating the Range as a Function of the Angle of Projection
# 🌟 =======================================================

# 🚀 Motivation:
# Projectile motion is a classic topic in physics that explores the path of an object thrown in the air.
# Analyzing the range as a function of the projection angle reveals interesting insights into kinematics.

# 📚 Theoretical Foundation:
# Initial velocity components:
# - Horizontal: v0x = v0 * cos(theta)
# - Vertical: v0y = v0 * sin(theta)

# Equations of motion:
# x(t) = v0 * cos(theta) * t
# y(t) = v0 * sin(theta) * t - 0.5 * g * t²

# 🎯 Range Formula:
# R = (v0² * sin(2 * theta)) / g

# =======================================================
# 🔧 Simulation Code
# =======================================================

def calculate_trajectory(v0, theta_deg, g=9.8, h0=0, time_step=0.01):
    """
    Calculate the trajectory of a projectile.
    
    Args:
        v0 (float): Initial velocity (m/s)
        theta_deg (float): Launch angle (degrees)
        g (float): Gravitational acceleration (m/s²)
        h0 (float): Initial height (m)
        time_step (float): Simulation time step (s)
    
    Returns:
        x, y (ndarray): Horizontal and vertical positions
        t_flight (float): Time of flight
    """
    theta = np.radians(theta_deg)
    v0x = v0 * np.cos(theta)
    v0y = v0 * np.sin(theta)

    discriminant = v0y**2 + 2 * g * h0
    if discriminant < 0:
        return [], [], 0  # No real solutions (no flight)

    t_flight = (v0y + np.sqrt(discriminant)) / g
    t = np.arange(0, t_flight + time_step, time_step)

    x = v0x * t
    y = h0 + v0y * t - 0.5 * g * t**2

    return x, y, t_flight

def calculate_range(v0, theta_deg, g=9.8, h0=0):
    """
    Calculate the range of a projectile.
    """
    x, y, _ = calculate_trajectory(v0, theta_deg, g, h0)
    if len(x) > 0:
        landing_idx = np.where(y < 0)[0]
        if len(landing_idx) > 0:
            idx = landing_idx[0]
            if idx > 0:
                x_range = x[idx - 1] + (x[idx] - x[idx - 1]) * (-y[idx - 1]) / (y[idx] - y[idx - 1])
                return x_range
        return x[-1]
    return 0

# 📊 Visualization
v0 = 20  # Initial velocity (m/s)
theta_values = np.arange(5, 86, 5)  # Angles from 5° to 85°
g = 9.8  # Gravity (m/s²)

ranges = [calculate_range(v0, theta, g) for theta in theta_values]
max_range = max(ranges)
optimal_angle = theta_values[np.argmax(ranges)]

# 📈 Range vs Launch Angle
plt.figure(figsize=(10, 6))
plt.plot(theta_values, ranges, 'b-', label='Range vs Angle')
plt.plot(optimal_angle, max_range, 'ro', label=f'Max Range: {max_range:.2f} m at {optimal_angle}°')
plt.xlabel('Launch Angle (degrees)')
plt.ylabel('Range (m)')
plt.title('🎯 Range vs Launch Angle')
plt.grid(True)
plt.legend()

# 📉 Trajectories for Selected Angles
angles = [15, 30, 45, 60, 75]
colors = ['r', 'g', 'b', 'c', 'm']
plt.figure(figsize=(12, 6))

for angle, color in zip(angles, colors):
    x, y, _ = calculate_trajectory(v0, angle, g)
    plt.plot(x, y, color=color, label=f'{angle}°')

plt.xlabel('Horizontal Distance (m)')
plt.ylabel('Height (m)')
plt.title('🎯 Trajectories for Different Launch Angles')
plt.grid(True)
plt.legend()
plt.ylim(0)
plt.show()
🔎 Conclusion
This exploration combined theoretical analysis and Python-based simulation to understand the relationship between launch angle and range. The maximum range is achieved at a 45° launch angle under ideal conditions, matching theoretical predictions. Adjusting parameters like initial velocity or gravity allows for further exploration of real-world scenarios.