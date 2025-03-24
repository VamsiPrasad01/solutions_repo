import numpy as np
import matplotlib.pyplot as plt
from matplotlib import style
from mpl_toolkits.mplot3d import Axes3D
from ipywidgets import interact, FloatSlider, IntSlider
import ipywidgets as widgets

# ========================
# 1. SETUP & CONSTANTS
# ========================
style.use('seaborn-v0_8-darkgrid')
plt.rcParams['font.family'] = 'serif'
plt.rcParams['axes.titlesize'] = 14
plt.rcParams['axes.labelsize'] = 12

# Constants
g_earth = 9.81  # m/s²
g_moon = 1.62   # m/s²

# ========================
# 2. CORE FUNCTIONS
# ========================
def projectile_range(v0, theta_deg, g=g_earth):
    """Calculate range of projectile."""
    theta_rad = np.radians(theta_deg)
    return (v0**2 * np.sin(2 * theta_rad)) / g

def trajectory(v0, theta_deg, g=g_earth, n_points=100):
    """Generate (x,y) trajectory coordinates."""
    theta_rad = np.radians(theta_deg)
    t_flight = (2 * v0 * np.sin(theta_rad)) / g
    t = np.linspace(0, t_flight, n_points)
    x = v0 * np.cos(theta_rad) * t
    y = v0 * np.sin(theta_rad) * t - 0.5 * g * t**2
    return x, y

# ========================
# 3. INTERACTIVE PLOTS
# ========================
def interactive_plot():
    """Launch interactive widget for trajectory exploration."""
    style = {'description_width': '150px'}
    
    interact_manual(
        plot_trajectory,
        v0=FloatSlider(value=20, min=5, max=50, step=1, 
                       description="Initial Velocity (m/s):", style=style),
        theta=IntSlider(value=45, min=0, max=90, step=1, 
                       description="Launch Angle (deg):", style=style),
        g=FloatSlider(value=9.81, min=1.62, max=9.81, step=0.1,
                      description="Gravity (m/s²):", style=style),
        show_range=widgets.Checkbox(value=True, description="Show Max Range Angle")
    )

def plot_trajectory(v0=20, theta=45, g=9.81, show_range=True):
    """Plot single trajectory with interactive controls."""
    fig, ax = plt.subplots(figsize=(10, 6))
    x, y = trajectory(v0, theta, g)
    ax.plot(x, y, 'r-', lw=3, label=f'v₀={v0} m/s, θ={theta}°')
    
    ax.set_title(f"Projectile Trajectory (g={g} m/s²)")
    ax.set_xlabel("Horizontal Distance (m)")
    ax.set_ylabel("Vertical Height (m)")
    ax.grid(True, alpha=0.3)
    
    if show_range:
        max_range_angle = 45 if g == 9.81 else np.degrees(0.5 * np.arcsin(g/9.81 * np.sin(np.radians(90))))
        ax.axvline(projectile_range(v0, max_range_angle, g), 
                   color='gray', linestyle='--', alpha=0.5)
        ax.annotate(f'Max range at {max_range_angle:.1f}°', 
                    xy=(projectile_range(v0, max_range_angle, g), 0), 
                    xytext=(10, 10), textcoords='offset points')
    
    ax.legend()
    plt.tight_layout()
    plt.show()

# ========================
# 4. 3D VISUALIZATION
# ========================
def plot_3d_surface():
    """Create 3D surface of range vs v0 and theta."""
    fig = plt.figure(figsize=(14, 8))
    ax = fig.add_subplot(111, projection='3d')
    
    v0_values = np.linspace(10, 50, 50)
    angles = np.linspace(0, 90, 50)
    V0, THETA = np.meshgrid(v0_values, angles)
    R = (V0**2 * np.sin(2 * np.radians(THETA))) / g_earth
    
    surf = ax.plot_surface(V0, THETA, R, cmap='viridis', 
                          rstride=1, cstride=1, alpha=0.8)
    
    ax.set_title("Range as Function of Velocity and Angle", pad=20)
    ax.set_xlabel("Initial Velocity (m/s)")
    ax.set_ylabel("Launch Angle (deg)")
    ax.set_zlabel("Range (m)")
    
    fig.colorbar(surf, shrink=0.5, aspect=10, label='Range (m)')
    
    # Highlight 45° line
    ax.plot(v0_values, [45]*len(v0_values), 
            [projectile_range(v, 45, g_earth) for v in v0_values],
            'r-', lw=3, label='Optimal 45°')
    
    ax.legend()
    plt.tight_layout()
    plt.savefig('3d_surface.png', dpi=300)
    plt.show()

# ========================
# 5. RUN ALL VISUALIZATIONS
# ========================
if __name__ == "__main__":
    print("🚀 Launching interactive projectile analyzer...")
    
    # Generate static plots
    plot_3d_surface()
    
    # Launch interactive widget (works in Jupyter)
    print("✅ Static plots generated. Launching interactive widget...")
    interactive_plot()