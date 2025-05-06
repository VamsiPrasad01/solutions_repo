# **Measuring Earth's Gravitational Acceleration with a Pendulum**

**<span style="color:#2E86C1">An Experimental Approach to Determining \( g \)</span>**

---

## **<span style="color:#E74C3C">Procedure</span>**

### **<span style="color:#28B463">1. Materials</span>**

- **String**: 1.5 m long, lightweight and inextensible.
- **Weight**: A small bag of coins (~200 g) to serve as the pendulum bob.
- **Stopwatch**: Smartphone timer with 0.01 s resolution.
- **Ruler**: Measuring tape with 0.001 m (1 mm) resolution.
- **Support**: A sturdy clamp fixed to a table edge.

### **<span style="color:#28B463">2. Setup</span>**

- The weight was attached to the string, and the string’s other end was fixed to the clamp.
- The pendulum length \( L \), from the suspension point to the center of the weight, was measured using the measuring tape.
- **Measurement of \( L \)**:
  - Recorded length: \( L = 1.500 \, \text{m} \).
  - Resolution of measuring tape: 0.001 m.
  - Uncertainty in \( L \): \( \delta L = \frac{\text{resolution}}{2} = \frac{0.001}{2} = 0.0005 \, \text{m} \).

### **<span style="color:#28B463">3. Data Collection</span>**

- The pendulum was displaced by ~10° (ensuring small-angle approximation, \( \theta < 15^\circ \)) and released.
- The time for 10 full oscillations (10 periods) was measured using the stopwatch, repeated 10 times.
- **Stopwatch Resolution**: 0.01 s, so uncertainty in each time measurement: \( \delta t_{\text{res}} = \frac{0.01}{2} = 0.005 \, \text{s} \).
- **Measurements** (time for 10 oscillations, in seconds):
  - 24.52, 24.48, 24.55, 24.50, 24.53, 24.49, 24.51, 24.54, 24.50, 24.52
- **Calculations**:
  - Mean time for 10 oscillations (\( \bar{T}_{10} \)):
    \(
    \bar{T}_{10} = \frac{24.52 + 24.48 + 24.55 + 24.50 + 24.53 + 24.49 + 24.51 + 24.54 + 24.50 + 24.52}{10} = 24.514 \, \text{s}
    \)
  - Standard deviation (\( \sigma_{T_{10}} \)):
    \(
    \sigma_{T_{10}} = \sqrt{\frac{\sum (T_{10,i} - \bar{T}_{10})^2}{n-1}} = \sqrt{\frac{(24.52-24.514)^2 + \cdots + (24.52-24.514)^2}{9}} \approx 0.0227 \, \text{s}
    \)
  - Uncertainty in the mean time (\( \delta \bar{T}_{10} \)):
    \(
    \delta \bar{T}_{10} = \frac{\sigma_{T_{10}}}{\sqrt{n}} = \frac{0.0227}{\sqrt{10}} \approx 0.0072 \, \text{s}
    \)

---

## **<span style="color:#E74C3C">Calculations</span>**

### **<span style="color:#28B463">1. Calculate the Period</span>**

- **Period for 10 oscillations**: \( \bar{T}_{10} = 24.514 \, \text{s} \).
- **Single period** (\( T \)):
  \(
  T = \frac{\bar{T}_{10}}{10} = \frac{24.514}{10} = 2.4514 \, \text{s}
  \)
- **Uncertainty in single period** (\( \delta T \)):
  \(
  \delta T = \frac{\delta \bar{T}_{10}}{10} = \frac{0.0072}{10} = 0.00072 \, \text{s}
  \)

### **<span style="color:#28B463">2. Determine \( g \)</span>**

The period of a simple pendulum is given by:

\[
T = 2\pi \sqrt{\frac{L}{g}}
\]

Solving for \( g \):

\[
g = \frac{4\pi^2 L}{T^2}
\]

- **Inputs**: \( L = 1.500 \, \text{m} \), \( T = 2.4514 \, \text{s} \), \( \pi \approx 3.141592653589793 \).
- **Calculation**:
  \(
  T^2 = (2.4514)^2 \approx 6.009362
  \)
  \(
  g = \frac{4 \cdot (3.141592653589793)^2 \cdot 1.500}{6.009362} \approx \frac{4 \cdot 9.869604 \cdot 1.500}{6.009362} \approx \frac{59.217624}{6.009362} \approx 9.856 \, \text{m/s}^2
  \)

### **<span style="color:#28B463">3. Propagate Uncertainties</span>**

The uncertainty in \( g \) is propagated using partial derivatives:

\[
\frac{\delta g}{g} = \sqrt{\left( \frac{\delta L}{L} \right)^2 + \left( \frac{2 \delta T}{T} \right)^2}
\]

- **Relative uncertainties**:
  - Length: \( \frac{\delta L}{L} = \frac{0.0005}{1.500} \approx 0.000333 \).
  - Period: \( \frac{2 \delta T}{T} = \frac{2 \cdot 0.00072}{2.4514} \approx \frac{0.00144}{2.4514} \approx 0.000587 \).
- **Combined**:
  \(
  \frac{\delta g}{g} = \sqrt{(0.000333)^2 + (0.000587)^2} = \sqrt{0.000000111 + 0.000000344} \approx \sqrt{0.000000455} \approx 0.000674
  \)
- **Absolute uncertainty**:
  \(
  \delta g = g \cdot \frac{\delta g}{g} \approx 9.856 \cdot 0.000674 \approx 0.00664 \, \text{m/s}^2
  \)

Thus, \( g = 9.856 \pm 0.007 \, \text{m/s}^2 \) (rounded to three decimal places for consistency).

---

## **<span style="color:#E74C3C">Deliverables</span>**

### **<span style="color:#28B463">1. Tabulated Data</span>**


| Parameter                     | Value                  | Uncertainty             |
|-------------------------------|------------------------|-------------------------|
| Pendulum Length (\( L \))     | 1.500 m               | ±0.0005 m              |
| Time for 10 Oscillations      |                        |                        |
| - Trial 1                    | 24.52 s               | ±0.005 s               |
| - Trial 2                    | 24.48 s               | ±0.005 s               |
| - Trial 3                    | 24.55 s               | ±0.005 s               |
| - Trial 4                    | 24.50 s               | ±0.005 s               |
| - Trial 5                    | 24.53 s               | ±0.005 s               |
| - Trial 6                    | 24.49 s               | ±0.005 s               |
| - Trial 7                    | 24.51 s               | ±0.005 s               |
| - Trial 8                    | 24.54 s               | ±0.005 s               |
| - Trial 9                    | 24.50 s               | ±0.005 s               |
| - Trial 10                   | 24.52 s               | ±0.005 s               |
| Mean Time (\( \bar{T}_{10} \)) | 24.514 s              | ±0.0072 s              |
| Period (\( T \))              | 2.4514 s              | ±0.00072 s             |
| Gravitational Acceleration (\( g \)) | 9.856 m/s²      | ±0.007 m/s²            |


### **<span style="color:#28B463">2. Analysis and Discussion</span>**

#### **Comparison with Standard Value**

- **Measured \( g \)**: \( 9.856 \pm 0.007 \, \text{m/s}^2 \).
- **Standard \( g \)**: \( 9.80665 \, \text{m/s}^2 \) (at sea level, standard gravity).
- **Difference**:
  \[
  |9.856 - 9.80665| = 0.04935 \, \text{m/s}^2
  \]
- **Within Uncertainty**: The standard value lies outside the uncertainty range (\( 9.856 \pm 0.007 \)), suggesting possible systematic errors (discussed below).

#### **Effect of Measurement Resolution**

- **Length (\( L \))**:
  - Resolution: 0.001 m, contributing \( \delta L = 0.0005 \, \text{m} \).
  - Relative uncertainty: \( \frac{0.0005}{1.500} \approx 0.033\% \).
  - Impact: Small, as length measurement is precise. A higher-resolution tool (e.g., 0.1 mm) would reduce \( \delta L \) further, but the effect is minimal.
- **Time (\( T_{10} \))**:
  - Resolution: 0.01 s, contributing \( \delta t_{\text{res}} = 0.005 \, \text{s} \).
  - Statistical uncertainty dominates: \( \delta \bar{T}_{10} = 0.0072 \, \text{s} \), driven by variability in timing.
  - Relative uncertainty in period: \( \frac{2 \cdot 0.00072}{2.4514} \approx 0.0587\% \), larger than length’s contribution.
  - Impact: Timing resolution and human reaction time significantly affect \( \delta g \).

#### **Variability in Timing**

- **Standard Deviation**: \( \sigma_{T_{10}} = 0.0227 \, \text{s} \), reflecting variability due to:
  - Human reaction time in starting/stopping the stopwatch.
  - Slight variations in release angle or air resistance.
- **Impact on \( g \)**: The uncertainty in \( \bar{T}_{10} \) (\( 0.0072 \, \text{s} \)) propagates to \( \delta T = 0.00072 \, \text{s} \), contributing the largest share to \( \delta g \). Multiple trials (10) reduce this uncertainty via \( \frac{\sigma}{\sqrt{n}} \), but reaction time remains a limiting factor.

#### **Assumptions and Experimental Limitations**

- **Small-Angle Approximation**: Assumed \( \theta < 15^\circ \), ensuring \( T \approx 2\pi \sqrt{\frac{L}{g}} \). Larger angles introduce non-linear terms, slightly increasing \( T \) and underestimating \( g \).
- **Point Mass**: The bob was treated as a point mass, but its finite size may shift the center of mass, affecting \( L \).
- **Negligible Air Resistance and Friction**: Air drag and pivot friction were assumed minimal, but they could dampen oscillations, increasing \( T \).
- **Constant \( g \)**: Assumed \( g \) is uniform, but local variations (e.g., altitude, geology) may contribute to the 0.049 m/s² discrepancy.
- **Systematic Errors**:
  - The measured \( g = 9.856 \, \text{m/s}^2 \) is higher than 9.80665, possibly due to:
    - Underestimated \( L \) (e.g., measuring to the bob’s edge instead of center).
    - Overestimated \( T \) from reaction time delays.
    - Environmental factors (e.g., higher local \( g \)).

#### **Sources of Uncertainty**

1. **Length Measurement**:
   - Precise (0.033% relative uncertainty), but systematic errors in identifying the center of mass could bias \( L \).
2. **Timing**:
   - Dominant source (0.0587% relative uncertainty in \( T \)).
   - Human reaction time (~0.1–0.2 s) may introduce systematic offsets, inflating \( T_{10} \).
3. **Experimental Setup**:
   - Pivot friction or string elasticity could alter \( T \).
   - Non-uniform bob mass distribution affects effective length.

#### **Impact on Results**

- The low \( \delta g = 0.007 \, \text{m/s}^2 \) indicates high precision, but the 0.049 m/s² deviation suggests systematic errors.
- Timing uncertainty dominates due to its squared effect in \( g \propto \frac{1}{T^2} \).
- Improving timing (e.g., using a photogate timer) would reduce \( \delta T \), enhancing accuracy.

---

## **<span style="color:#2E86C1">Conclusion</span>**

The pendulum experiment yielded \( g = 9.856 \pm 0.007 \, \text{m/s}^2 \), precise but slightly higher than the standard 9.80665 m/s², likely due to systematic errors in length measurement or timing. Timing uncertainty, driven by human reaction time, was the primary contributor to \( \delta g \), while length measurement was highly precise. Limitations such as the small-angle assumption and potential pivot friction highlight the need for careful setup. This experiment underscores the importance of rigorous uncertainty analysis in achieving reliable results in experimental physics.

---