### Shorter Term (Probably in 1.0) ###

**Dynamic parameters** -- [ParticleController](ParticleController.md)s and possibly other components would support dynamic parameters that can change over time, either via a step function, linear interpolator, easing or spline.

**Improved domains** -- native code [Domain](Domain.md)s with support for more shapes, such as curves, toroids, heightmaps and tri-meshes. _(in progress)_

**Texturizers** -- Built-in support for texture atlases, texture coordinate generation, texture animation and 3D textures. _(in progress)_

**Display list renderer** -- Enable particles that have complex geometry, shading and texturing

**SSE optimizations** -- Vector code written using SSE intrinsics where supported

**More Controllers** -- avoid, weathervane, target.

**Particle Effects** -- High-level parameterized class API for complete drop-in effects orchestrating groups, controllers and renderers. Effects will include things like explosions, debris, fire, smoke, lightning, beams, rain, etc.

### Longer Term (post 1.0) ###

**Particle-Particle Collision** -- For collective and liquid effects.

**String and Cloth Simulation** -- Special particle groups/controllers for simulation of cloth, fibers, etc.

**Flocking/swarm behavior** -- For simulating collective flying behavior, like bees and birds

**Noise integration** -- Support for more complex behaviors and parameterization using Perlin noise

**Vector fields** -- Enables particle attribute dependancies, like velocity based on position, color based on velocity, etc.

**GUI Effect Editor** -- Point-and-click goodness