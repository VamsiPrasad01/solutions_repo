# Problem 1

# Investigating the Range as a Function of the Angle of Projection

## Motivation
Projectile motion is a classic physics problem that bridges simplicity and complexity. By studying how the range depends on the angle of projection, we uncover a mix of linear and quadratic relationships, governed by initial conditions like velocity, gravity, and height. This exploration connects to real-world scenarios—think of a basketball shot, a cannonball, or even a spacecraft launch!

## 1. Theoretical Foundation
Let’s derive the equations from Newton’s laws. For a projectile launched with initial velocity \( v_0 \) at an angle \( \theta \) from the horizontal, with no air resistance:

- **Horizontal motion**: \( x(t) = v_0 \cos(\theta) t \)
- **Vertical motion**: \( y(t) = v_0 \sin(\theta) t - \frac{1}{2} g t^2 \)

Time of flight (\( T \)) is when \( y = 0 \) (assuming launch and landing at same height):
\[ 0 = v_0 \sin(\theta) T - \frac{1}{2} g T^2 \]
\[ T = \frac{2 v_0 \sin(\theta)}{g} \]

Range (\( R \)) is the horizontal distance:
\[ R = v_0 \cos(\theta) \cdot T = v_0 \cos(\theta) \cdot \frac{2 v_0 \sin(\theta)}{g} \]
Using the identity \( \sin(2\theta) = 2 \sin(\theta) \cos(\theta) \):
\[ R = \frac{v_0^2 \sin(2\theta)}{g} \]

This is the family of solutions—range depends on \( \theta \), \( v_0 \), and \( g \).

## 2. Analysis of the Range
- **Angle Dependence**: \( R \) peaks at \( \theta = 45^\circ \) (since \( \sin(2\theta) = 1 \) at \( 90^\circ \)).
- **Parameters**: Higher \( v_0 \) increases \( R \), while stronger \( g \) reduces it.

## 3. Practical Applications
- **Sports**: Optimizing a javelin throw.
- **Engineering**: Artillery range calculations.
- **Uneven Terrain**: Adjust \( y_0 \) (initial height) in the model.

## 4. Implementation
Let’s simulate this in Python with stunning visuals using `matplotlib` and `numpy`.

### Python Code
```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import style

# Set a sleek style
style.use('seaborn-darkgrid')

# Constants
g = 9.81  # m/s^2 (gravity)

# Function to calculate range
def projectile_range(v0, theta_deg, g=9.81):
    theta_rad = np.radians(theta_deg)
    return (v0**2 * np.sin(2 * theta_rad)) / g

# Simulation parameters
angles = np.arange(0, 91, 1)  # 0 to 90 degrees
v0_values = [10, 20, 30]      # Different initial velocities (m/s)
g_values = [9.81, 1.62]       # Earth and Moon gravity (m/s^2)

# Plot 1: Range vs Angle for different velocities
plt.figure(figsize=(10, 6))
for v0 in v0_values:
    ranges = [projectile_range(v0, theta) for theta in angles]
    plt.plot(angles, ranges, label=f'v0 = {v0} m/s', linewidth=2.5)
plt.title('Range vs Angle of Projection (g = 9.81 m/s²)', fontsize=14, pad=10)
plt.xlabel('Angle (degrees)', fontsize=12)
plt.ylabel('Range (meters)', fontsize=12)
plt.legend(fontsize=10)
plt.grid(True, linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()

# Plot 2: Range vs Angle for different gravities
plt.figure(figsize=(10, 6))
for g in g_values:
    ranges = [projectile_range(20, theta, g) for theta in angles]
    plt.plot(angles, ranges, label=f'g = {g} m/s²', linewidth=2.5)
plt.title('Range vs Angle of Projection (v0 = 20 m/s)', fontsize=14, pad=10)
plt.xlabel('Angle (degrees)', fontsize=12)
plt.ylabel('Range (meters)', fontsize=12)
plt.legend(fontsize=10)
plt.grid(True, linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()

# Plot 3: Trajectory visualization for selected angles
def trajectory(v0, theta_deg, g=9.81):
    theta_rad = np.radians(theta_deg)
    t_flight = (2 * v0 * np.sin(theta_rad)) / g
    t = np.linspace(0, t_flight, 100)
    x = v0 * np.cos(theta_rad) * t
    y = v0 * np.sin(theta_rad) * t - 0.5 * g * t**2
    return x, y

plt.figure(figsize=(12, 7))
angles_to_plot = [15, 45, 75]
colors = ['#FF6F61', '#6B5B95', '#88B04B']
for theta, color in zip(angles_to_plot, colors):
    x, y = trajectory(20, theta)
    plt.plot(x, y, label=f'{theta}°', color=color, linewidth=2.5)
plt.title('Projectile Trajectories (v0 = 20 m/s, g = 9.81 m/s²)', fontsize=14, pad=10)
plt.xlabel('Range (meters)', fontsize=12)
plt.ylabel('Height (meters)', fontsize=12)
plt.legend(fontsize=10)
plt.grid(True, linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()

