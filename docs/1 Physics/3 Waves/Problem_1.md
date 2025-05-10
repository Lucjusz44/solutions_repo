# 🌊 Interference Patterns on a Water Surface

## 🎯 Motivation

Interference occurs when waves from different sources meet and overlap. On a water surface, we can clearly see these effects when ripples from different points intersect. The resulting pattern helps us understand how waves **reinforce** or **cancel** each other out.

Studying these patterns:

* Makes wave physics visual and intuitive
* Helps us learn about **wave phase**, **superposition**, and **wave behavior**
* Can be used in real-world applications like acoustics, optics, and engineering

---

## 📌 Problem Statement

We want to simulate the **interference pattern** formed by **multiple point wave sources** placed at the **corners of a regular polygon** (like a square or triangle).

Each source creates circular waves that spread across a surface. At each point on the surface, we’ll calculate the **combined wave effect** from all the sources using **wave equations** and **superposition**.

---

## 📐 Formula for a Circular Wave

To calculate the wave height (displacement) at any point on the surface, use:

$$
\text{Wave from 1 source:} \quad h_i(x, y) = A \cdot \cos(k \cdot r_i - \omega \cdot t + \phi)
$$

Where:

* $A$ is the wave amplitude
* $r_i = \sqrt{(x - x_i)^2 + (y - y_i)^2}$ is the distance from point $(x, y)$ to the source at $(x_i, y_i)$
* $k = \frac{2\pi}{\lambda}$ is the wave number (related to wavelength $\lambda$)
* $\omega = 2\pi f$ is the angular frequency (related to frequency $f$)
* $\phi$ is the wave’s initial phase
* $t$ is time

---

## 🔗 Superposition: Adding the Waves Together

To find the **total wave height** at any point on the grid, we add the contributions from all sources:

$$
\text{Total wave:} \quad h_{\text{total}}(x, y) = \sum_{i=1}^{N} A \cdot \cos(k \cdot r_i - \omega \cdot t + \phi)
$$

---
### 6. **Visualize**
We'll use Python + Matplotlib to simulate and visualize the interference.

---

## 🧪 Python Simulation

```python
import numpy as np
import matplotlib.pyplot as plt

# Wave parameters
A = 1
lambda_ = 1
f = 1
omega = 2 * np.pi * f
k = 2 * np.pi / lambda_
phi = 0
radius = 5
num_sources = 4  # square
grid_size = 200

# Grid
x = np.linspace(-10, 10, grid_size)
y = np.linspace(-10, 10, grid_size)
X, Y = np.meshgrid(x, y)

# Source positions (square shape)
theta = np.linspace(0, 2 * np.pi, num_sources, endpoint=False)
source_x = radius * np.cos(theta)
source_y = radius * np.sin(theta)

# Time snapshot
t = 0

# Wave function
def wave(x, y, sx, sy):
    r = np.sqrt((x - sx)**2 + (y - sy)**2)
    return A * np.cos(k * r - omega * t + phi)

# Total wave from all sources
total = np.zeros_like(X)
for i in range(num_sources):
    total += wave(X, Y, source_x[i], source_y[i])
plt.figure(figsize=(8, 6))
plt.imshow(total, extent=[-10, 10, -10, 10], cmap='coolwarm', origin='lower')
plt.colorbar(label='Wave Displacement')
plt.scatter(source_x, source_y, color='yellow', edgecolors='black', s=100, label='Sources')
plt.title("Interference Pattern (Heatmap)", fontsize=14)
plt.xlabel("X")
plt.ylabel("Y")
plt.grid(True, linestyle='--', alpha=0.3)
plt.legend()
plt.show()
plt.figure(figsize=(8, 6))
plt.contour(X, Y, total, levels=20, cmap='plasma')
plt.scatter(source_x, source_y, color='yellow', edgecolors='black', s=100)
plt.title("Contour Lines of Wave Interference")
plt.xlabel("X")
plt.ylabel("Y")
plt.grid(True, linestyle='--', alpha=0.3)
plt.show()
![alt text](image-1.png)
