# Problem 1
### Problem 1: Interference Patterns on a Water Surface

### Motivation:
Interference patterns form when waves from different sources overlap, creating new wave patterns on a surface. On water, this can be easily observed when ripples from different points meet and combine. Depending on their phase relationship, the waves may either reinforce (constructive interference) or cancel each other out (destructive interference). This concept helps us visualize wave behavior, understand phase relationships, and provides insight into real-world applications such as sound waves, electromagnetic waves, and water ripples.

### Task Breakdown:

#### 1. **Select a Regular Polygon:**
We begin by selecting a regular polygon (for example, an equilateral triangle, square, or pentagon) where point sources of waves will be placed at the vertices.

#### 2. **Position the Sources:**
We then place wave sources at the vertices of the polygon, assuming that each source emits a wave with the same amplitude, wavelength, and frequency. 

#### 3. **Wave Equations:**
Each point source emits a wave that can be described by the equation for a circular wave in the water surface. The displacement at a point \((x, y)\) on the water surface due to a point source at \((x_i, y_i)\) is given by:

\[
\text{Displacement} = A \cos(k r - \omega t + \phi)
\]

Where:
- \(A\) is the amplitude,
- \(k = \frac{2\pi}{\lambda}\) is the wave number,
- \(\lambda\) is the wavelength,
- \(r = \sqrt{(x - x_i)^2 + (y - y_i)^2}\) is the distance from the source to the point,
- \(\omega = 2\pi f\) is the angular frequency,
- \(f\) is the frequency, and
- \(\phi\) is the initial phase of the wave.

#### 4. **Superposition of Waves:**
The principle of superposition states that the total displacement at any point is the sum of the displacements from each source. Therefore, for \(N\) sources placed at the vertices of the polygon, the total displacement is:

\[
\text{Total Displacement} = \sum_{i=1}^{N} A \cos(k r_i - \omega t + \phi)
\]

Where \(r_i\) is the distance from the \(i\)-th source to the point \((x, y)\).

#### 5. **Analyze Interference Patterns:**
The superimposed wave function will exhibit regions of constructive interference (where the displacements add up to amplify the wave) and destructive interference (where the displacements cancel each other out). By analyzing this, we can understand the pattern of wave interaction.

#### 6. **Visualization:**
We can visualize the interference patterns by plotting the total displacement at each point on a grid, where the x and y axes represent spatial coordinates on the water surface.

---

### Implementation in Python:

Below is the Python code to simulate and visualize the interference patterns caused by multiple point sources located at the vertices of a regular polygon (e.g., square). This simulation assumes that all sources emit waves with the same parameters.

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
A = 1           # Amplitude of the wave
lambda_ = 1     # Wavelength
f = 1           # Frequency
omega = 2 * np.pi * f  # Angular frequency
k = 2 * np.pi / lambda_  # Wave number
phi = 0         # Initial phase of the wave
radius = 5      # Radius of the circle where the sources are placed
num_sources = 4 # Number of sources (e.g., 4 for a square)
grid_size = 200  # Grid size for the plot (increase for higher resolution)

# Create a grid of points (x, y)
x = np.linspace(-10, 10, grid_size)
y = np.linspace(-10, 10, grid_size)
X, Y = np.meshgrid(x, y)

# Calculate the coordinates of the sources (vertices of the regular polygon)
theta = np.linspace(0, 2 * np.pi, num_sources, endpoint=False)
source_x = radius * np.cos(theta)
source_y = radius * np.sin(theta)

# Function to calculate wave displacement at a point (x, y) due to a source
def wave_displacement(x, y, source_x, source_y, A, k, omega, t, phi):
    r = np.sqrt((x - source_x)**2 + (y - source_y)**2)  # Distance from the source
    return A * np.cos(k * r - omega * t + phi)  # Wave displacement equation

# Create a grid to store the displacement
displacement = np.zeros_like(X)

# Time
t = 0

# Calculate the total displacement due to all sources using superposition
for i in range(num_sources):
    displacement += wave_displacement(X, Y, source_x[i], source_y[i], A, k, omega, t, phi)

# Plotting the interference pattern
plt.figure(figsize=(10, 8))
plt.contourf(X, Y, displacement, cmap='twilight', levels=100)  # Contour plot with twilight colormap
plt.colorbar(label="Wave Displacement", pad=0.01, aspect=10)  # Color bar

# Plot sources as bright markers (bigger and highlighted)
plt.scatter(source_x, source_y, color="yellow", edgecolors="black", s=100, label="Wave Sources", zorder=5)

# Add title and labels with improved font style
plt.title("Interference Pattern on a Water Surface", fontsize=16, fontweight='bold')
plt.xlabel("X Position (Units)", fontsize=12)
plt.ylabel("Y Position (Units)", fontsize=12)

# Grid and axis customization
plt.grid(True, which='both', linestyle='--', linewidth=0.5, color='gray', alpha=0.7)
plt.axhline(0, color='black',linewidth=1)
plt.axvline(0, color='black',linewidth=1)

# Add a legend with a customized style
plt.legend(fontsize=12, frameon=True, facecolor='white', edgecolor='black')

# Add a subtitle explaining the plot
plt.figtext(0.5, 0.02, "This plot shows the interference pattern formed by waves from a regular polygon of sources.", 
            ha="center", fontsize=12, color='darkslategray')

# Show the plot
plt.tight_layout()
plt.show()
```
![alt text](image.png)

### Key Features of the Code:

1. **Grid Creation**: 
   - A mesh grid is created over a specified range of x and y values. This represents the water surface where the interference pattern will be plotted.

2. **Wave Function**: 
   - The displacement due to each source is calculated based on the wave equation.
   - The distance between each point on the grid and the source is calculated to determine the wave displacement.

3. **Superposition**: 
   - The total wave displacement is calculated by summing the contributions from each point source.

4. **Visualization**: 
   - The `contourf` function is used to plot the interference pattern, which is color-coded to show regions of constructive and destructive interference.
   - The sources are marked as yellow points for clear identification.

5. **Customization**:
   - The plot is customized with labels, grid lines, legends, and a color bar for better clarity and aesthetics.

---

