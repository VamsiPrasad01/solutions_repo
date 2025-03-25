# **Investigating the Range as a Function of the Angle of Projection**  
**<span style="color:#2E86C1">A Comprehensive Physics and Computational Analysis</span>**

---

## **<span style="color:#E74C3C">1. Theoretical Foundation</span>**  
### **<span style="color:#28B463">1.1 Derivation of Projectile Motion Equations</span>**  
We begin with Newton's second law in 2D for a projectile with initial velocity **v₀** at angle **θ** under gravity **g**:

**<span style="color:#5DADE2">Horizontal Motion (x-axis):</span>**  
No acceleration → Uniform motion  
\[
\frac{d^2x}{dt^2} = 0 \implies x(t) = v_{0x}t = \color{#E74C3C}{v_0\cos\theta}\cdot t
\]

**<span style="color:#5DADE2">Vertical Motion (y-axis):</span>**  
Constant acceleration (-g)  
\[
\frac{d^2y}{dt^2} = -g \implies y(t) = \color{#E74C3C}{v_0\sin\theta}\cdot t - \frac{1}{2}\color{#E74C3C}{g}t^2
\]

### **<span style="color:#28B463">1.2 Time of Flight and Range</span>**  
Solving for when the projectile returns to ground (y=0):  
\[
T = \frac{2\color{#E74C3C}{v_0\sin\theta}}{\color{#E74C3C}{g}}
\]  
Substituting into x(t) gives the **range equation**:  
\[
R = \frac{\color{#E74C3C}{v_0^2}\sin(2\theta)}{\color{#E74C3C}{g}} \quad \text{(Maximum at θ=45°)}
\]

### **<span style="color:#28B463">1.3 Family of Solutions</span>**  
The general solution forms a parameterized family based on:  
- **<span style="color:#E67E22">Initial velocity (v₀)</span>**  
- **<span style="color:#E67E22">Launch angle (θ)</span>**  
- **<span style="color:#E67E22">Gravity (g)</span>**  
- **<span style="color:#E67E22">Initial height (h₀)</span>**

---

## **<span style="color:#E74C3C">2. Range Analysis</span>**  
### **<span style="color:#28B463">2.1 Angle Dependence</span>**  
Key characteristics of the range equation:  
- **Peak range** at θ=45° (when sin(2θ)=1)  
- **Complementary angles** (e.g., 30° & 60°) give equal ranges  
- **Zero range** at θ=0° and θ=90°  

### **<span style="color:#28B463">2.2 Parameter Sensitivity Analysis</span>**  
| Parameter | Effect | Mathematical Relationship |  
|-----------|--------|----------------------------|  
| **<span style="color:#E67E22">v₀</span>** | Quadratic impact | R ∝ v₀² |  
| **<span style="color:#E67E22">g</span>** | Inverse relationship | R ∝ 1/g |  
| **<span style="color:#E67E22">h₀</span>** | Increases range | Modified equation required |  

---

## **<span style="color:#E74C3C">3. Practical Applications</span>**  
### **<span style="color:#28B463">3.1 Real-World Modifications</span>**  
| Scenario | Effect on Projectile |  
|----------|----------------------|  
| **<span style="color:#E67E22">Uphill Launch</span>** | Optimal angle >45° |  
| **<span style="color:#E67E22">Downhill Launch</span>** | Optimal angle <45° |  
| **<span style="color:#E67E22">Air Resistance</span>** | Reduces range by 30-50%, optimal angle ~40° |  
| **<span style="color:#E67E22">Spin (Magnus Effect)</span>** | Creates curved trajectories |  

---

## **<span style="color:#E74C3C">4. Computational Implementation</span>**  
### **<span style="color:#28B463">4.1 Python Simulation Code</span>**  
<details>
<summary>Click to see the Python simulation code</summary>

```python
import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import interact

def plot_trajectory(v0=20, theta=45, g=9.81, h0=0):
    theta_rad = np.radians(theta)
    t_flight = (v0*np.sin(theta_rad) + np.sqrt((v0*np.sin(theta_rad))**2 + 2*g*h0))/g
    t = np.linspace(0, t_flight, 100)
    
    x = v0*np.cos(theta_rad)*t
    y = h0 + v0*np.sin(theta_rad)*t - 0.5*g*t**2
    
    plt.figure(figsize=(10,5))
    plt.plot(x, y, 'b-', linewidth=2)
    plt.title(f'Projectile Trajectory (θ={theta}°, v₀={v0}m/s)')
    plt.xlabel('Horizontal Distance (m)')
    plt.ylabel('Height (m)')
    plt.grid()
    plt.ylim(0, max(y)*1.2)

interact(plot_trajectory, v0=(5,50,5), theta=(0,90,5), g=(1.62,24.79,0.1), h0=(0,20,1))
```
<details>
### **<span style="color:#28B463">4.2 Key Visualizations</span>**  
1. **<span style="color:#E67E22">Range vs Angle Curves</span>**  
    ![alt text](<Range vs Launch Angle.jpg>)
2. **<span style="color:#E67E22">Trajectories for Different Launch Angles</span>** 
    ![alt text](<Trajectories for Different Launch Angles.jpg>)

---

## **<span style="color:#E74C3C">5. Deliverables</span>**  
### **<span style="color:#28B463">5.1 Complete Analysis Package</span>**  
- **Jupyter Notebook** with:  
  - Interactive trajectory simulator  
  - Parameter sensitivity plots  
  - Planetary environment comparisons  

### **<span style="color:#28B463">5.2 Limitations and Extensions</span>**  
**Current Limitations:**  
- No air resistance  
- Flat Earth assumption  
- 2D-only simulation  

**Advanced Extensions:**  
1. **<span style="color:#E67E22">Drag Force Model</span>**  
   \[
   F_{drag} = -\frac{1}{2}C_d\rho Av^2
   \]
2. **<span style="color:#E67E22">Wind Effects</span>**  
   - Crosswind compensation  
3. **<span style="color:#E67E22">3D Simulation</span>**  
   - Coriolis effect for long-range projectiles  

---

**<span style="color:#2E86C1">Conclusion:</span>**  
This investigation bridges fundamental physics with practical applications through computational modeling. The color-coded equations and interactive visualizations enhance understanding of how projectile range depends on launch parameters, while identifying avenues for more sophisticated real-world modeling.  

**<span style="color:#E67E22">Complete code and interactive demos available in the supplementary materials.</span>**