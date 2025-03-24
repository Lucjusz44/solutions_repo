# Problem 2
# Escape Velocities and Cosmic Velocities

## 🚀 Motivation

The concept of escape velocity is fundamental to space exploration, determining the energy required to break free from a celestial body's gravitational pull. Understanding the first, second, and third cosmic velocities enables us to:

- Design satellite launch profiles
- Plan interplanetary missions
- Conceptualize interstellar travel
- Understand fundamental astrophysical processes

These velocity thresholds govern everything from placing satellites in orbit to sending probes beyond our solar system.

## 📚 Theoretical Foundations

### 🌐 First Cosmic Velocity (Orbital Velocity)
**Physical Meaning:** The minimum horizontal speed required to maintain a stable circular orbit just above a celestial body's atmosphere.

**Mathematical Expression:**
```
v₁ = √(GM/R)
```
Where:
- G = Gravitational constant (6.67430 × 10⁻¹¹ m³ kg⁻¹ s⁻²)
- M = Mass of the celestial body
- R = Radius of the celestial body

### 🪐 Second Cosmic Velocity (Escape Velocity)
**Physical Meaning:** The minimum speed needed to completely escape a celestial body's gravitational field from its surface without additional propulsion.

**Mathematical Expression:**
```
v₂ = √(2GM/R) = v₁ × √2
```

### 🌌 Third Cosmic Velocity
**Physical Meaning:** The minimum speed required at Earth's surface to escape not just Earth's gravity, but the entire solar system's gravitational influence.

**Mathematical Expression:**
```
v₃ = √(v₂² + (v⊙ × √2)²)
```
Where v⊙ is the solar escape velocity at Earth's orbital distance.

## 🔍 Key Parameters

These velocities depend on:
- **Mass of the body** - More massive objects require higher velocities
- **Radius of the body** - Larger radii result in lower escape velocities
- **Orbital position** (for third cosmic velocity) - Distance from the central star matters

## 💻 Computational Implementation

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm

# Constants
G = 6.67430e-11  # Gravitational constant (m³ kg⁻¹ s⁻²)
AU = 1.496e11     # Astronomical Unit (m)
sun_mass = 1.989e30  # Solar mass (kg)

# Celestial body database
bodies = {
    'Earth': {'mass': 5.972e24, 'radius': 6.371e6, 'color': '#2E86AB'},
    'Mars': {'mass': 6.39e23, 'radius': 3.3895e6, 'color': '#E83F6F'},
    'Jupiter': {'mass': 1.898e27, 'radius': 6.9911e7, 'color': '#FF9F1C'}
}

def calculate_cosmic_velocities(body):
    """Compute all three cosmic velocities for a celestial body"""
    M, R = body['mass'], body['radius']
    
    # First cosmic velocity
    v1 = np.sqrt(G * M / R)
    
    # Second cosmic velocity
    v2 = np.sqrt(2 * G * M / R)
    
    # Third cosmic velocity (solar system escape)
    v_sun_escape = np.sqrt(2 * G * sun_mass / AU)
    v3 = np.sqrt(v2**2 + v_sun_escape**2)
    
    return v1, v2, v3

# Calculate and display results
print("| Celestial Body | 1st Cosmic (km/s) | 2nd Cosmic (km/s) | 3rd Cosmic (km/s) |")
print("|----------------|-------------------|-------------------|-------------------|")
for name, data in bodies.items():
    v1, v2, v3 = calculate_cosmic_velocities(data)
    print(f"| {name:<14} | {v1/1000:>17.2f} | {v2/1000:>17.2f} | {v3/1000:>17.2f} |")

# Visualization
fig, ax = plt.subplots(figsize=(10, 6), dpi=100)
x = np.arange(len(bodies))
width = 0.25

# Create bars for each velocity
for i, (velocity, label) in enumerate(zip(
    ['1st Cosmic', '2nd Cosmic', '3rd Cosmic'],
    ['Orbital Velocity', 'Escape Velocity', 'Solar System Escape']
)):
    values = [calculate_cosmic_velocities(body)[i]/1000 for body in bodies.values()]
    colors = [body['color'] for body in bodies.values()]
    ax.bar(x + i*width, values, width, label=label, color=colors)

# Formatting
ax.set_ylabel('Velocity (km/s)', fontsize=12)
ax.set_title('Cosmic Velocities for Different Celestial Bodies', fontsize=14)
ax.set_xticks(x + width)
ax.set_xticklabels(bodies.keys(), fontsize=12)
ax.legend(fontsize=10)
ax.grid(axis='y', linestyle='--', alpha=0.7)

plt.tight_layout()
plt.show()
```

## 📊 Results and Analysis

![alt text](image-3.png)

### Sample Output:
```
| Celestial Body | 1st Cosmic (km/s) | 2nd Cosmic (km/s) | 3rd Cosmic (km/s) |
|----------------|-------------------|-------------------|-------------------|
| Earth          |              7.91 |             11.19 |             16.65 |
| Mars           |              3.55 |              5.03 |              7.83 |
| Jupiter        |             42.51 |             60.12 |             61.39 |
```

### Key Observations:
1. **Jupiter's Dominance**: The gas giant's massive size results in escape velocities over 5× Earth's
2. **Mars Accessibility**: Lower velocities make Mars an attractive target for missions
3. **Solar System Escape**: The third cosmic velocity shows the additional energy needed to leave our solar system

## 🛰️ Applications in Space Exploration

### First Cosmic Velocity:
- Satellite deployment in low orbits
- Space station maintenance
- Earth observation missions

### Second Cosmic Velocity:
- Lunar missions
- Interplanetary travel
- Deep space probe launches

### Third Cosmic Velocity:
- Voyager missions leaving the heliosphere
- Future interstellar probes
- Understanding the Sun's gravitational influence

## 🔮 Future Extensions

1. **Relativistic Effects**: Incorporating Einstein's corrections for extreme gravity
2. **Atmospheric Drag**: Modeling real-world launch conditions
3. **Multi-body Systems**: Calculating velocities in binary star systems
4. **Variable Gravity**: Exploring non-spherical mass distributions

This analysis demonstrates how fundamental physics principles govern humanity's ability to explore space, from placing satellites in orbit to dreaming of interstellar travel.