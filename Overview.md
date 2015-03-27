This should give you a basic overview of the terminology and top-level constructs of the Lepton particle engine.

## Groups ##

ParticleGroup objects are the lowest level construct. Groups consist of an arbitrary number of [Particle](Particle.md)s. Although in many ways a group is like a particle container, particles cannot exist independently outside of a group. All particles are created and destroyed via their group's methods. A particle spends its entire existence within a single group.

Groups may be iterated to access individual particles. The objects returned through iteration are ParticleProxy objects that serve as a convenient way to manipulate individual particles within the group. Ultimately, however, particles are not independent first-class objects.

All particles in a group have the same general behavior and are rendered together using the same renderer.

## Controllers ##

ParticleController objects control the behavior of particles in a group. Typically, particle controllers are bound to a particular ParticleGroup or ParticleSystem. The controller modifies the particles in the group or system it is bound to at each update iteration. An update iteration is typically scheduled at a regular interval, or can simply be done for each animation frame.

Several controller types are included in Lepton, and it is easy to create your own as well. Some commonly used controllers are:

  * LifetimeController -- Kills particles older than a specified age.
  * GravityController -- Imparts a fixed acceleration to all particles in a group
  * MovementController -- Updates the position of particles based on their velocity.
  * ColorBlenderController -- Modulates the color of particles over time.
  * FaderController -- Fades the alpha component of particles in and out over time.
  * CollectorController -- Kills particles as the enter or exit a [Domain](Domain.md).
  * BounceController -- Deflects particles off the surface of a [Domain](Domain.md).
  * MagnetController -- Attracts or repels particles to or from a [Domain](Domain.md).
  * DragController -- Simulates drag forces for particles moving through a fluid (i.e., air or water).

Controllers are a major focus of Lepton's development and more will be added over time.

## Emitters ##

[ParticleEmitter](ParticleEmitter.md)s are a special type of controller that create new particles in a group. They can be configured to emit particles at a specific rate, or can be used to emit a burst of particles at will.

As factories for particles, emitters have several features for specifying the initial particle parameters. A template particle is provided as a basis for the particles emitted. Specified alone, each particle emitted is an exact clone of this template. The basic template may be augmented by a deviation template, which specifies the statisical deviation of the [Particle](Particle.md) attributes. This allows you to easily express how much, and in what way each particle differs from one another.

Particle attribute values may also be expressed as a sequence of discrete values (e.g., the colors of the rainbow, discrete sizes, etc). Additionally, particle attribute values may be derived from a spacial [Domain](Domain.md), allowing expressive control over the desired range of composite attributes, such as position, velocity and color vectors.

Two types of emitters are currently included with lepton:

  * StaticEmitter -- Emits particles at a regular rate over time, or can emit an arbitrary number at once
  * PerParticleEmitter -- Emits particles originating from the positions of all existing particles in a group. Useful for creating trails of particles. See the fireworks example included in lepton.

## Renderers ##

A [ParticleRenderer](ParticleRenderer.md) object can be bound to a group, defining how the particles of the group are drawn. Renderers can be invoked via the ParticleSystem, each ParticleGroup or individually as desired. Like [ParticleController](ParticleController.md)s, particle renderers have a simple API and are easy to create for yourself. Four are currently included with the Lepton engine:

  * PointRenderer -- Simple renderer using OpenGL GL\_POINTs. Does not support individual particle size or rotation. Can be textured by using a [Texturizer](Texturizer.md).
  * BillboardRenderer -- A more advanced aligned-quad billboard renderer using OpenGL. natively supports individual particle size, rotation.  Can be textured by using a [Texturizer](Texturizer.md).
  * PygameFillRenderer -- A simple render that draws particles to a pygame surface using color fills.
  * PygameBlitRenderer -- A more sophisticated renderer for pygame that draws particles by blitting to a surface. Also supports scaling and rotation. This is the pygame equivilant of the BillboardRenderer.

Renderers are a major focus of Lepton's development, expect more advanced and varied options soon.

## Domains ##

[Domain](Domain.md)s provide a way to describe spatial regions for use by particle controllers and emitters. Controllers, such as the BounceController and CollectorController affect particle behavior as they move into or out of the domain in space. [ParticleEmitter](ParticleEmitter.md)s can use domains to express a geometric range of values that may be used as initial particle attribute values.

Several domains are included with Lepton, and more are planned:

  * LineDomain -- Simple line segment
  * PlaneDomain -- Infinite 2D plane dividing space
  * AABoxDomain -- Axis-aligned rectangular prism
  * SphereDomain -- Spherical region of space
  * DiscDomain -- Circular flat disc
  * CylinderDomain -- Capped circular cylinder
  * ConeDomain -- Right circular cone

## Systems ##

A ParticleSystem is the highest-level construct in Lepton. A system can contain [ParticleGroup](ParticleGroup.md)s, and you can bind global controllers to it to be applied to all groups in the system. The system has methods for conveniently updating and rendering all of the groups it contains.

A default particle system is created for you when you import lepton. By default,  groups are automatically added to the default particle system when they are created.  Most applications can simply use the default particle system object, but more complex applications can create their own systems as needed. A multi-window application might need separate systems for each window, or separately systems can be used if different graphics are updated in different timelines.

The ParticleSystem object contains an update method which is designed to be scheduled to regularly invoke the groups' controllers. It also has a draw method which can be called within the main loop or "on draw" event handler of the application. This allows the application to remain decoupled from the individual groups, controllers and renderers which may change dynamically at run-time.