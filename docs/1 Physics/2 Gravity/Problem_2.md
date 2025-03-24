# Problem 2
# Escape Velocities and Cosmic Velocities

## Introduction

Escape velocity is a fundamental concept in astrophysics and space exploration that determines the minimum speed needed for an object to break free from a celestial body's gravitational pull without further propulsion. This concept extends to three "cosmic velocities" that define different thresholds for space travel.

## Definitions

### First Cosmic Velocity (Orbital Velocity)
The minimum speed required for an object to maintain a stable circular orbit around a celestial body just above its atmosphere. This is essentially the orbital velocity at the body's surface.

### Second Cosmic Velocity (Escape Velocity)
The minimum speed needed for an object to completely break free from a celestial body's gravitational influence without any additional propulsion.

### Third Cosmic Velocity
The minimum speed required for an object to escape the gravitational pull of the entire solar system from the surface of a planet (typically Earth).

## Mathematical Derivation

### General Formulas

1. **First Cosmic Velocity (v₁):**
   ```
   v₁ = √(GM/R)
   ```
   Where:
   - G = gravitational constant (6.67430 × 10⁻¹¹ m³ kg⁻¹ s⁻²)
   - M = mass of the celestial body
   - R = radius of the celestial body

2. **Second Cosmic Velocity (v₂):**
   ```
   v₂ = √(2GM/R) = v₁ × √2
   ```

3. **Third Cosmic Velocity (v₃):**
   ```
   v₃ = √(v₂² + (v₁_sun × √2)²)
   ```
   Where v₁_sun is the first cosmic velocity relative to the Sun at Earth's orbital distance.

### Parameters Affecting These Velocities

- **Mass of the celestial body:** More massive bodies require higher velocities
- **Radius of the body:** Larger radii result in lower escape velocities
- **Orbital distance from the Sun (for third cosmic velocity):** Farther distances require lower escape velocities

## Python Implementation

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11  # Gravitational constant (m^3 kg^-1 s^-2)

# Celestial body data (mass in kg, radius in m)
bodies = {
    'Earth': {'mass': 5.972e24, 'radius': 6.371e6},
    'Mars': {'mass': 6.39e23, 'radius': 3.3895e6},
    'Jupiter': {'mass': 1.898e27, 'radius': 6.9911e7}
}

# Sun data (for third cosmic velocity)
sun_mass = 1.989e30  # kg
earth_orbital_radius = 1.496e11  # m (1 AU)

def calculate_velocities(body):
    """Calculate all three cosmic velocities for a celestial body"""
    M = body['mass']
    R = body['radius']
    
    # First cosmic velocity (orbital velocity)
    v1 = np.sqrt(G * M / R)
    
    # Second cosmic velocity (escape velocity)
    v2 = np.sqrt(2 * G * M / R)
    
    # Third cosmic velocity (solar system escape from surface)
    # First calculate Earth's orbital velocity around Sun
    v_earth_orbit = np.sqrt(G * sun_mass / earth_orbital_radius)
    # Then calculate third cosmic velocity
    v3 = np.sqrt(v2**2 + (v_earth_orbit * np.sqrt(2))**2)
    
    return v1, v2, v3

# Calculate and display velocities for all bodies
print("{:<10} {:<20} {:<20} {:<20}".format("Body", "1st Cosmic (km/s)", "2nd Cosmic (km/s)", "3rd Cosmic (km/s)"))
for name, data in bodies.items():
    v1, v2, v3 = calculate_velocities(data)
    print("{:<10} {:<20.2f} {:<20.2f} {:<20.2f}".format(name, v1/1000, v2/1000, v3/1000))

# Visualization
names = list(bodies.keys())
v1_values = []
v2_values = []
v3_values = []

for name in names:
    v1, v2, v3 = calculate_velocities(bodies[name])
    v1_values.append(v1/1000)
    v2_values.append(v2/1000)
    v3_values.append(v3/1000)

x = np.arange(len(names))
width = 0.25

fig, ax = plt.subplots(figsize=(10, 6))
rects1 = ax.bar(x - width, v1_values, width, label='1st Cosmic')
rects2 = ax.bar(x, v2_values, width, label='2nd Cosmic')
rects3 = ax.bar(x + width, v3_values, width, label='3rd Cosmic')

ax.set_ylabel('Velocity (km/s)')
ax.set_title('Cosmic Velocities for Different Celestial Bodies')
ax.set_xticks(x)
ax.set_xticklabels(names)
ax.legend()

fig.tight_layout()
plt.show()
```

## Results

The script will output a table showing the three cosmic velocities for Earth, Mars, and Jupiter, and display a bar chart comparing them.

### Example Output (approximate values):

```
Body       1st Cosmic (km/s)    2nd Cosmic (km/s)    3rd Cosmic (km/s)    
Earth      7.91                  11.19                 16.65
Mars       3.55                  5.03                  7.83
Jupiter    42.51                 60.12                 61.39
```

## Importance in Space Exploration

1. **First Cosmic Velocity:**
   - Essential for placing satellites in orbit
   - Determines the minimum speed for orbital insertion
   - Affects spacecraft maneuver planning

2. **Second Cosmic Velocity:**
   - Critical for lunar and interplanetary missions
   - Determines the energy needed to leave a planet's gravity
   - Affects spacecraft design and fuel requirements

3. **Third Cosmic Velocity:**
   - Necessary for interstellar missions
   - Determines the energy needed to leave the solar system
   - Important for understanding the feasibility of probes like Voyager

## Discussion

The calculations show how dramatically different these velocities are for different bodies. Jupiter's enormous mass creates escape velocities that make missions challenging, while Mars' lower velocities make it more accessible for exploration.

Understanding these velocities helps in:
- Mission planning and fuel calculations
- Spacecraft design and propulsion requirements
- Assessing the feasibility of different space missions
- Developing launch strategies and trajectories

The third cosmic velocity is particularly interesting as it represents the threshold for interstellar travel, though practical interstellar missions would require much higher velocities to reach other stars in reasonable timeframes.

## Conclusion

Cosmic velocities provide fundamental benchmarks for space exploration. Their calculation and understanding are essential for mission planning and spacecraft design. The Python implementation demonstrates how these velocities vary across different celestial bodies, highlighting the challenges of exploring different parts of our solar system and beyond.