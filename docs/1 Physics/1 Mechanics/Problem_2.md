# **<span style="color:#2E86C1">Investigating the Dynamics of a Forced Damped Pendulum</span>**

## **<span style="color:#E74C3C">Theoretical Foundation</span>**

The forced damped pendulum is governed by the following nonlinear differential equation:

```
θ'' + (b/m)θ' + (g/L)sinθ = (F/mL)cos(ω_d t)
```

Where:
- θ is the angular displacement
- b is the damping coefficient
- m is the mass of the bob
- L is the length of the pendulum
- g is gravitational acceleration
- F is the amplitude of the driving force
- ω_d is the driving frequency

### **<span style="color:#28B463">Small-angle approximation</span>**

For small oscillations (θ << 1), we can approximate sinθ ≈ θ, which linearizes the equation:

```
θ'' + (b/m)θ' + (g/L)θ = (F/mL)cos(ω_d t)
```

This is the equation of a driven, damped harmonic oscillator with:
- Natural frequency ω_0 = √(g/L)
- Damping ratio γ = b/(2m)

### **<span style="color:#28B463">Resonance conditions</span>**

The system exhibits resonance when the driving frequency ω_d approaches the damped natural frequency:

```
ω_res = √(ω_0² - 2γ²)
```

At resonance, the system absorbs maximum energy from the driving force, leading to large amplitude oscillations. The quality factor Q = ω_0/(2γ) determines the sharpness of the resonance peak.

---

## **<span style="color:#E74C3C">Analysis of Dynamics</span>**

The behavior of the forced damped pendulum depends critically on three parameters:
1. **Damping coefficient** (b/m) - determines how quickly oscillations decay
2. **Driving amplitude** (F/mL) - controls the energy input to the system
3. **Driving frequency** (ω_d) - relative to natural frequency affects resonance

### **<span style="color:#28B463">Regular vs. chaotic motion</span>**

For small driving forces, the system settles into periodic motion synchronized with the driving frequency (period-1 or higher periodic orbits). As the driving amplitude increases, the system can transition to:
- Quasiperiodic motion (two incommensurate frequencies)
- Chaotic motion (sensitive dependence on initial conditions)

---

## **<span style="color:#E74C3C">Practical Applications</span>**

1. **Energy harvesting devices**: Pendulum-based systems convert mechanical vibrations to electrical energy
2. **Suspension bridges**: Modeling resonant effects from wind or pedestrian loading
3. **Oscillating circuits**: Electrical analogs (RLC circuits with AC sources)
4. **Seismology**: Modeling building responses to earthquake vibrations
5. **Biological rhythms**: Circadian clocks under external forcing

---

## **<span style="color:#E74C3C">Implementation</span>**

<details>
  <summary><span style="color:#28B463">See Code</span></summary>

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Parameters
g = 9.81  # gravity (m/s^2)
L = 1.0   # pendulum length (m)
b = 0.5   # damping coefficient (kg/s)
m = 1.0   # mass (kg)
F = 1.2   # driving amplitude (N)
ω_d = 2.0 # driving frequency (rad/s)

# Differential equation
def forced_pendulum(t, y):
    θ, ω = y
    dθdt = ω
    dωdt = -(b/m)*ω - (g/L)*np.sin(θ) + (F/(m*L))*np.cos(ω_d*t)
    return [dθdt, dωdt]

# Initial conditions and time span
y0 = [0.1, 0]  # initial angle and angular velocity
t_span = (0, 100)  # simulation time
t_eval = np.linspace(*t_span, 3000)  # evaluation points

# Solve the ODE
sol = solve_ivp(forced_pendulum, t_span, y0, t_eval=t_eval, rtol=1e-6, atol=1e-8)

# Plotting
plt.figure(figsize=(12, 8))

# Time series
plt.subplot(2, 2, 1)
plt.plot(sol.t, sol.y[0], label='θ(t)')
plt.xlabel('Time (s)')
plt.ylabel('Angle (rad)')
plt.title('Angular Displacement vs Time')
plt.grid()

# Phase portrait
plt.subplot(2, 2, 2)
plt.plot(sol.y[0], sol.y[1])
plt.xlabel('Angle θ (rad)')
plt.ylabel('Angular velocity ω (rad/s)')
plt.title('Phase Portrait')
plt.grid()

# Poincaré section (stroboscopic map at driving period)
drive_period = 2*np.pi/ω_d
poincare_times = np.arange(50, 100, drive_period)  # skip transient
poincare_points = []
for t in poincare_times:
    idx = np.argmin(np.abs(sol.t - t))
    poincare_points.append(sol.y[:, idx])
poincare_points = np.array(poincare_points).T

plt.subplot(2, 2, 3)
plt.scatter(poincare_points[0], poincare_points[1], s=10)
plt.xlabel('θ at Poincaré section')
plt.ylabel('ω at Poincaré section')
plt.title('Poincaré Section (Stroboscopic Map)')
plt.grid()

# Frequency spectrum (FFT)
plt.subplot(2, 2, 4)
n = len(sol.y[0])
freq = np.fft.fftfreq(n, d=sol.t[1]-sol.t[0])
fft_vals = np.fft.fft(sol.y[0])
plt.plot(freq[:n//2], np.abs(fft_vals[:n//2]))
plt.xlabel('Frequency (Hz)')
plt.ylabel('Amplitude')
plt.title('Frequency Spectrum')
plt.grid()

plt.tight_layout()
plt.show()
```
</details>

---

## **<span style="color:#E74C3C">Results and Analysis</span>**

The simulation shows several characteristic behaviors:

1. **Periodic motion**: For small F, the pendulum synchronizes with the driving frequency
2. **Subharmonic response**: Period doubling and higher-order periods appear as F increases
3. **Chaotic motion**: At certain parameters, the Poincaré section shows fractal-like structure

---

## **<span style="color:#E74C3C">Bifurcation Analysis</span>**

To visualize the transition to chaos, we can create a bifurcation diagram by varying one parameter (e.g., driving force F):

<details>
  <summary><span style="color:#28B463">See Code</span></summary>

```python
# Bifurcation diagram (varying F)
F_values = np.linspace(0.5, 1.5, 200)
bifurcation_points = []

for F in F_values:
    sol = solve_ivp(forced_pendulum, (0, 500), y0, t_eval=np.linspace(0, 500, 5000), 
                   rtol=1e-6, atol=1e-8)
    # Record θ values at driving period after transient
    transient = 300
    θ_points = []
    for t in np.arange(transient, 500, 2*np.pi/ω_d):
        idx = np.argmin(np.abs(sol.t - t))
        θ_points.append(sol.y[0, idx])
    bifurcation_points.append(θ_points[-50:])  # last 50 points

plt.figure(figsize=(10, 6))
for i, F in enumerate(F_values):
    plt.plot([F]*len(bifurcation_points[i]), bifurcation_points[i], 
            'b.', markersize=1, alpha=0.5)
plt.xlabel('Driving Force Amplitude F (N)')
plt.ylabel('Stable Points in Poincaré Section')
plt.title('Bifurcation Diagram (Varying Driving Force)')
plt.grid()
plt.show()
```
</details>

---

## **<span style="color:#E74C3C">Limitations and Extensions</span>**

1. **Limitations**:
   - Small-angle approximation breaks down for large oscillations
   - Assumes point mass and massless rod
   - Neglects air resistance nonlinearities
   - Doesn't account for elastic deformations

2. **Extensions**:
   - Nonlinear damping terms (e.g., quadratic air resistance)
   - Double pendulum or coupled pendulums
   - Non-periodic driving forces (e.g., stochastic forcing)
   - Parametric excitation (moving pivot point)

---

## **<span style="color:#E74C3C">Conclusion</span>**

The forced damped pendulum exhibits a rich variety of behaviors from simple periodic motion to complex chaos. Numerical simulations reveal how parameter variations lead to qualitatively different dynamics, with applications across physics and engineering. The transition to chaos through period doubling is a fundamental route observed in many nonlinear systems.

Further investigations could explore:
- Basins of attraction for different initial conditions
- Lyapunov exponents to quantify chaotic behavior
- Control strategies to stabilize unstable periodic orbits
- Experimental validation with physical pendulum setups

---

With these updates, the Python code is now hidden behind clickable sections, and the section headings are color-coded to visually enhance the content.