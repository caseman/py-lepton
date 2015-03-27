Particles are members of [ParticleGroup](ParticleGroup.md)s. They cannot exist outside of a group. Once a particle is created in a particular group, it lives its entire life there. Particles can't be moved from one group to another, though they can be easily cloned.

Particles are model objects with no innate behavior or presentation by design. Particle behavior is handled by ParticleController objects. Presentation is handled by [ParticleRenderer](ParticleRenderer.md)s.

# Particle Attributes #

  * **position** (3D Vector) -- Current position of particle
  * **color** (RGBA color) -- Color/alpha of particle
  * **velocity** (3D Vector) -- Current velocity vector
  * **size** (3D Vector) -- Width, height and depth of particle (meaning depends on renderer)
  * **up** (3D Vector) -- Up orientation vector (Euler angles, in radians)
  * **rotation** (3D Vector) -- Angular rate of change of up vector, in radians per unit time
  * **last\_position** (3D Vector) -- Previous particle position (after last update)
  * **last\_velocity** (3D Vector) -- Previous particle velocity (after last update)
  * **age** -- Time elapsed since particle was created
  * **mass** -- Particle mass (can be used for controller physics)

# Accessing Particles #

Particles are accessed by iterating a ParticleGroup, typically within a ParticleController or ParticleRenderer. From within Python code, iterating a group returns ParticleProxy objects which can be used to conveniently manipulate them. You cannot store long-term references to particles outside of groups, however. See ParticleProxy for more details.