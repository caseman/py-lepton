Particle renderers control the presentation of [Particle](Particle.md)s in a ParticleGroup. Renderers are typically bound to a group and invoked by the group's `draw()` method. Renders may also be invoked separately if desired, which can be useful if particular state changes are required before rendering a particular group.

A renderer object must implement the method `draw()`.

## OpenGL State in Renderers ##

Any assumptions a renderer makes about the OpenGL state should be documented. Relying on particular state should be avoided where possible, since multiple renderers will often be invoked by the application indirectly, and in arbitrary order.

If the renderer modifies the OpenGL state, it should restore it where practical to avoid adversely affecting other renderers that follow or the calling application.

## Built-in Renderers ##

Four particle renderers are currently included with the Lepton engine:

  * PointRenderer -- Simple renderer using OpenGL GL\_POINTs. Does not support individual particle size or rotation. May use textures by enabling point sprites in your application. Can be textured by using a [Texturizer](Texturizer.md).

  * BillboardRenderer -- A more advanced aligned-quad billboard renderer using OpenGL. natively supports individual particle size and texturing using a [Texturizer](Texturizer.md).

  * PygameFillRenderer -- A simple render that draws particles to a pygame surface using color fills.
  * PygameBlitRenderer -- A more sophisticated renderer for pygame that draws particles by blitting to a surface. Also supports scaling and rotation. This is the pygame equivilant of the BillboardRenderer.
