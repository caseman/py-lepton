Particle controllers affect the behavior of [Particle](Particle.md)s. They are bound to [ParticleGroup](ParticleGroup.md)s and typically invoked by their group's `update()` method.

A controller is a simple callable object (i.e., function-like) with the following signature:
```
   my_controller(time_delta, group)
```

When invoked, the controller is free to manipulate the group and its particles anyway it likes. Typically, a controller will iterate the group's particles and modify them in some way. For example, here is a controller that changes the direction of a particles movement based on time, making them travel in a meandering path:

```
from lepton import ParticleGroup
from math import sin, cos

def meanderer(td, group):
	for particle in group:
		particle.position.x += sin((particle.age + particle.velocity.x) / 10) * 5
		particle.position.y += cos((particle.age + particle.velocity.y) / 10) * 5

group = ParticleGroup(controllers=[meanderer])
```

That's it! The simplest controllers can be functions, like the above. If the controller needs to store configuration state, you can simply define it as a class with an `__call__` method.

Note that a single controller object may be bound to multiple groups (or bound to a system, which is effectively the same thing). Controller code should not assume that it will always be called with the same group object argument. If possible, controllers should avoid having group-specific state, but if that is necessary, it will need to be implemented such that the controller can be bound to many groups.

## Built-in Controllers ##

The following controllers come with Lepton:

  * LifetimeController -- Kills particles older than a specified age.
  * GravityController -- Imparts a fixed acceleration to all particles in a group
  * MovementController -- Updates the position of particles based on their velocity.
  * ColorBlenderController -- Modulates the color of particles over time.
  * FaderController -- Fades the alpha component of particles in and out over time.
  * CollectorController -- Kills particles as the enter or exit a [Domain](Domain.md).
  * BounceController -- Deflects particles off the surface of a [Domain](Domain.md).
  * MagnetController -- Attracts or repels particles to or from a [Domain](Domain.md).
  * DragController -- Simulates drag forces for particles moving through a fluid (i.e., air or water).