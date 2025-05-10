### **Simulating the Effects of the Lorentz Force**

#### **Motivation**:

The Lorentz force, expressed as:

$$
\mathbf{F} = q (\mathbf{E} + \mathbf{v} \times \mathbf{B})
$$

governs the motion of charged particles in electric and magnetic fields. It is foundational in fields like plasma physics, particle accelerators, and astrophysics. By focusing on simulations, we can explore the practical applications and visualize the complex trajectories that arise due to this force.

---

### **Task**:

1. **Exploration of Applications**:

   * Identify systems where the Lorentz force plays a key role (e.g., particle accelerators, mass spectrometers, plasma confinement).
   * Discuss the relevance of electric ($\mathbf{E}$) and magnetic ($\mathbf{B}$) fields in controlling the motion of charged particles.

2. **Simulating Particle Motion**:

   * Implement a simulation to compute and visualize the trajectory of a charged particle under:

     * A uniform magnetic field.
     * Combined uniform electric and magnetic fields.
     * Crossed electric and magnetic fields.
   * Simulate the particle’s circular, helical, or drift motion based on initial conditions and field configurations.

3. **Parameter Exploration**:

   * Allow variations in:

     * Field strengths ($E$, $B$).
     * Initial particle velocity ($v_0$).
     * Charge and mass of the particle ($q$, $m$).
   * Observe how these parameters influence the trajectory.

4. **Visualization**:

   * Create clear, labeled plots showing the particle’s path in 2D and 3D for different scenarios.
   * Highlight physical phenomena such as the Larmor radius and drift velocity.

---

### **Formulas**:

The Lorentz force equation is given by:

$$
\mathbf{F} = q (\mathbf{E} + \mathbf{v} \times \mathbf{B})
$$

Where:

* $\mathbf{F}$ is the force acting on the charged particle.
* $q$ is the charge of the particle.
* $\mathbf{E}$ is the electric field.
* $\mathbf{v}$ is the velocity of the particle.
* $\mathbf{B}$ is the magnetic field.
* $\times$ represents the cross product between velocity and the magnetic field.

For a particle moving in a uniform magnetic field, its trajectory can be described by circular motion, and the radius of this motion (Larmor radius) is given by:

$$
r = \frac{mv_{\perp}}{qB}
$$

Where:

* $m$ is the mass of the particle.
* $v_{\perp}$ is the component of the velocity perpendicular to the magnetic field.
* $q$ is the charge of the particle.
* $B$ is the magnetic field strength.

---

### **Visualization of Particle Motion**:

Below is a Python simulation to visualize the particle's trajectory under the influence of a magnetic field.

---

### **Python Code**:

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
q = 1.6e-19  # Charge of particle (Coulombs)
m = 9.11e-31  # Mass of particle (kg)
E = np.array([0, 0])  # Electric field (V/m) [no electric field in this example]
B = np.array([0, 0, 1])  # Magnetic field (T), along the z-axis
v_initial = np.array([1e5, 0])  # Initial velocity (m/s)
r_initial = np.array([0, 0])  # Initial position (m)

# Time parameters
t_max = 1e-6  # Maximum time (s)
dt = 1e-9  # Time step (s)
time_steps = int(t_max / dt)

# Initialize position and velocity
r = r_initial
v = v_initial

# Store the particle trajectory
trajectory = []

# Function to calculate Lorentz force and update velocity and position
def lorentz_force(v, r, q, m, E, B):
    # Lorentz force F = q(E + v x B)
    v_cross_B = np.cross(v, B)  # Cross product v x B
    F = q * (E + v_cross_B)  # Total force
    a = F / m  # Acceleration (F = ma)
    return a

# Simulate the particle motion
for t in range(time_steps):
    a = lorentz_force(v, r, q, m, E, B)
    v += a * dt  # Update velocity (v = v + a * dt)
    r += v * dt  # Update position (r = r + v * dt)
    trajectory.append(r.copy())

# Convert trajectory to numpy array for easier plotting
trajectory = np.array(trajectory)

# Plotting the particle's trajectory
plt.figure(figsize=(8, 6))
plt.plot(trajectory[:, 0], trajectory[:, 1], label='Particle Path', color='blue')
plt.scatter(0, 0, color='red', label='Initial Position')  # Starting point
plt.title("Particle Trajectory in a Magnetic Field", fontsize=14)
plt.xlabel("X Position (m)")
plt.ylabel("Y Position (m)")
plt.grid(True, linestyle='--', alpha=0.5)
plt.legend()
plt.show()
```

---

This code simulates the motion of a charged particle in a uniform magnetic field and visualizes its trajectory. The plot will show how the particle moves in a circular path due to the Lorentz force.
