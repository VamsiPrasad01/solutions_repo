import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D
from matplotlib.animation import FuncAnimation
from ipywidgets import interact, FloatSlider, IntSlider, Dropdown, ToggleButtons
import ipywidgets as widgets
from scipy.integrate import odeint
from sklearn.ensemble import RandomForestRegressor
from sympy import symbols, Eq, init_printing, latex
import random
from tqdm import tqdm
import seaborn as sns
from PIL import Image
from matplotlib.offsetbox import OffsetImage, AnnotationBbox

# =============================================
# 1. PHYSICS ENGINE WITH ALL REAL-WORLD EFFECTS
# =============================================

class ProjectileSimulator:
    def __init__(self):
        # Physical constants
        self.g_earth = 9.80665  # m/s²
        self.g_moon = 1.62      # m/s²
        self.g_mars = 3.721     # m/s²
        self.rho_earth = 1.225  # kg/m³ (air density)
        self.Cd_sphere = 0.47   # Drag coefficient
        
        # Visual settings
        self.colors = {
            'earth': '#2E86AB',
            'moon': '#A5A5A5',
            'mars': '#E2725B',
            'trajectory': '#FF9F1C',
            'highlight': '#FF3366'
        }
        
        self.plt_style = 'seaborn-darkgrid'
        plt.style.use(self.plt_style)
        
    def simulate_trajectory(self, v0, theta, g=9.81, h0=0, 
                          drag=False, spin=0, wind=0, dt=0.001):
        """Advanced physics simulation with multiple effects"""
        theta_rad = np.radians(theta)
        t = 0
        x, y = 0, h0
        vx = v0 * np.cos(theta_rad)
        vy = v0 * np.sin(theta_rad)
        
        # Store trajectory points
        x_points, y_points, t_points = [x], [y], [t]
        
        while y >= 0:
            if drag:
                # Calculate drag force (quadratic model)
                v = np.sqrt(vx**2 + vy**2)
                Fd = 0.5 * self.rho_earth * self.Cd_sphere * 0.018 * v**2
                
                # Calculate Magnus effect (simplified)
                Fm = 0.25 * self.rho_earth * 0.018 * (spin*2*np.pi/60) * v
                
                # Acceleration components
                ax = -Fd/m * vx/v + Fm/m * vy/v + wind
                ay = -g - Fd/m * vy/v - Fm/m * vx/v
            else:
                ax = wind
                ay = -g
            
            # Update velocity and position
            vx += ax * dt
            vy += ay * dt
            x += vx * dt
            y += vy * dt
            t += dt
            
            # Store points
            x_points.append(x)
            y_points.append(y)
            t_points.append(t)
            
        return np.array(x_points), np.array(y_points), np.array(t_points)

    def calculate_range(self, v0, theta, **kwargs):
        """Calculate the horizontal range"""
        x, y, _ = self.simulate_trajectory(v0, theta, **kwargs)
        return x[-1] if len(x) > 0 else 0

# =============================================
# 2. VISUALIZATION ENGINE WITH SPECIAL EFFECTS
# =============================================

class ProjectileVisualizer:
    def __init__(self):
        self.sim = ProjectileSimulator()
        self.fig = None
        self.ax = None
        
        # Custom colormaps
        self.cmap1 = cm.get_cmap('plasma')
        self.cmap2 = cm.get_cmap('viridis')
        
        # Load celestial body images
        self.planet_imgs = {
            'earth': Image.open('earth_texture.jpg'),
            'moon': Image.open('moon_texture.jpg'),
            'mars': Image.open('mars_texture.jpg')
        }
        
    def _add_planet_bg(self, planet):
        """Add planetary background to plot"""
        if planet in self.planet_imgs:
            img = self.planet_imgs[planet]
            imagebox = OffsetImage(img, zoom=0.2)
            ab = AnnotationBbox(imagebox, (0.5, 0.5), frameon=False)
            self.ax.add_artist(ab)
    
    def plot_trajectory(self, v0=20, theta=45, g=9.81, h0=0,
                       drag=False, spin=0, wind=0, planet='earth'):
        """Plot a single trajectory with special effects"""
        self.fig, self.ax = plt.subplots(figsize=(12, 8))
        
        # Get trajectory data
        x, y, t = self.sim.simulate_trajectory(v0, theta, g, h0, drag, spin, wind)
        
        # Create glowing trajectory effect
        n_points = len(x)
        for i in range(n_points):
            alpha = i/n_points * 0.8 + 0.2
            self.ax.plot(x[:i+1], y[:i+1], color=self.sim.colors['trajectory'], 
                        alpha=alpha, lw=3)
        
        # Add impact explosion effect
        if n_points > 0:
            self.ax.scatter(x[-1], 0, s=500, c='red', 
                          alpha=0.6, marker='*', edgecolors='gold', linewidths=2)
        
        # Add planet background
        self._add_planet_bg(planet)
        
        # Customize plot
        self.ax.set_title(f"Projectile Motion Simulation\n"
                        f"v₀ = {v0} m/s, θ = {theta}°, g = {g} m/s²", 
                        fontsize=16, pad=20)
        self.ax.set_xlabel("Horizontal Distance (m)")
        self.ax.set_ylabel("Height (m)")
        self.ax.grid(True, alpha=0.3)
        
        # Add colorbar for time
        norm = plt.Normalize(t.min(), t.max())
        sm = plt.cm.ScalarMappable(cmap=self.cmap1, norm=norm)
        sm.set_array([])
        cbar = plt.colorbar(sm, ax=self.ax)
        cbar.set_label('Time (s)')
        
        plt.tight_layout()
        plt.show()
    
    def plot_3d_surface(self):
        """Create 3D surface plot of range vs v0 and theta"""
        fig = plt.figure(figsize=(16, 10))
        ax = fig.add_subplot(111, projection='3d')
        
        # Generate data
        v0_values = np.linspace(10, 50, 30)
        theta_values = np.linspace(0, 90, 30)
        V0, THETA = np.meshgrid(v0_values, theta_values)
        
        R = np.zeros_like(V0)
        for i in tqdm(range(len(v0_values))):
            for j in range(len(theta_values)):
                R[j,i] = self.sim.calculate_range(V0[j,i], THETA[j,i])
        
        # Plot surface with special effects
        surf = ax.plot_surface(V0, THETA, R, cmap=self.cmap2,
                              rstride=1, cstride=1, alpha=0.8,
                              edgecolor='none', shade=True)
        
        # Add contour projections
        ax.contourf(V0, THETA, R, zdir='z', offset=R.min(), cmap=self.cmap2, alpha=0.3)
        
        # Customize
        ax.set_title("Range as Function of Velocity and Angle", fontsize=16, pad=20)
        ax.set_xlabel("Initial Velocity (m/s)")
        ax.set_ylabel("Launch Angle (deg)")
        ax.set_zlabel("Range (m)")
        ax.view_init(elev=30, azim=45)
        
        # Add colorbar
        fig.colorbar(surf, shrink=0.5, aspect=10, label='Range (m)')
        
        plt.tight_layout()
        plt.show()

# =============================================
# 3. INTERACTIVE DASHBOARD
# =============================================

class InteractiveDashboard:
    def __init__(self):
        self.vis = ProjectileVisualizer()
        self.setup_widgets()
        
    def setup_widgets(self):
        """Create interactive controls"""
        self.v0_slider = FloatSlider(
            value=20, min=5, max=100, step=1,
            description='Initial Velocity (m/s):',
            style={'description_width': 'initial'},
            continuous_update=False
        )
        
        self.theta_slider = IntSlider(
            value=45, min=0, max=90, step=1,
            description='Launch Angle (deg):',
            style={'description_width': 'initial'}
        )
        
        self.g_dropdown = Dropdown(
            options={'Earth (9.81 m/s²)': 9.81,
                    'Moon (1.62 m/s²)': 1.62,
                    'Mars (3.72 m/s²)': 3.72},
            value=9.81,
            description='Gravity:',
            style={'description_width': 'initial'}
        )
        
        self.drag_toggle = ToggleButtons(
            options=['No Drag', 'With Drag'],
            value='No Drag',
            description='Air Resistance:',
            style={'description_width': 'initial'}
        )
        
        self.spin_slider = FloatSlider(
            value=0, min=-5000, max=5000, step=100,
            description='Spin (rpm):',
            style={'description_width': 'initial'},
            disabled=True
        )
        
        self.wind_slider = FloatSlider(
            value=0, min=-10, max=10, step=0.5,
            description='Wind Speed (m/s):',
            style={'description_width': 'initial'},
            disabled=True
        )
        
        # Link widgets
        def update_drag_options(change):
            self.spin_slider.disabled = change['new'] == 'No Drag'
            self.wind_slider.disabled = change['new'] == 'No Drag'
            
        self.drag_toggle.observe(update_drag_options, names='value')
        
    def launch(self):
        """Start interactive dashboard"""
        interact_manual(
            self.vis.plot_trajectory,
            v0=self.v0_slider,
            theta=self.theta_slider,
            g=self.g_dropdown,
            drag=widgets.fixed(self.drag_toggle.value == 'With Drag'),
            spin=self.spin_slider,
            wind=self.wind_slider,
            planet=widgets.fixed('earth')
        )

# =============================================
# 4. MACHINE LEARNING OPTIMIZATION
# =============================================

class ProjectileOptimizer:
    def __init__(self):
        self.model = None
        self.train_model()
        
    def generate_training_data(self, n_samples=1000):
        """Create dataset for ML model"""
        X = []
        y = []
        
        for _ in range(n_samples):
            v0 = random.uniform(5, 100)
            h0 = random.uniform(0, 50)
            g = random.uniform(1.62, 24.79)  # Moon to Jupiter gravity
            
            # Find optimal angle numerically
            angles = np.linspace(0, 90, 91)
            ranges = [self.calculate_range(v0, a, g, h0) for a in angles]
            opt_angle = angles[np.argmax(ranges)]
            
            X.append([v0, h0, g])
            y.append(opt_angle)
            
        return np.array(X), np.array(y)
    
    def train_model(self):
        """Train Random Forest regressor"""
        X, y = self.generate_training_data()
        self.model = RandomForestRegressor(n_estimators=100)
        self.model.fit(X, y)
        
    def predict_optimal_angle(self, v0, h0, g):
        """Predict optimal launch angle using ML"""
        return self.model.predict([[v0, h0, g]])[0]

# =============================================
# 5. MAIN EXECUTION
# =============================================

if __name__ == "__main__":
    print("🚀 Launching Ultimate Projectile Motion Simulator")
    
    # Initialize components
    simulator = ProjectileSimulator()
    visualizer = ProjectileVisualizer()
    dashboard = InteractiveDashboard()
    optimizer = ProjectileOptimizer()
    
    # Example: Predict optimal angle for Mars conditions
    v0_example = 30
    h0_example = 10
    g_mars = 3.721
    
    opt_angle = optimizer.predict_optimal_angle(v0_example, h0_example, g_mars)
    print(f"\n✨ ML Prediction: For v0={v0_example} m/s, h0={h0_example} m, g={g_mars} m/s²")
    print(f"   Optimal launch angle = {opt_angle:.1f}°")
    
    # Generate all visualizations
    print("\n🖼️ Generating visualizations...")
    visualizer.plot_trajectory(v0=25, theta=45, g=9.81, drag=True, spin=2000)
    visualizer.plot_3d_surface()
    
    # Launch interactive dashboard
    print("\n🎮 Launching interactive dashboard...")
    dashboard.launch()
    
    print("\n✅ Simulation complete! Explore the interactive widgets above.")

# =============================================
# 6. UNIT TESTS AND VALIDATION
# =============================================

def run_tests():
    """Validate physics calculations"""
    print("\n🔬 Running unit tests...")
    
    # Basic projectile motion
    x, y, t = simulator.simulate_trajectory(20, 45)
    assert abs(x[-1] - 40.77) < 0.1, "Basic range calculation failed"
    
    # Zero gravity edge case
    x, y, t = simulator.simulate_trajectory(10, 45, g=1e-9)
    assert x[-1] > 1e6, "Zero gravity behavior incorrect"
    
    print("✅ All tests passed!")

run_tests()