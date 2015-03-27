Groups contain particles. [ParticleController](ParticleController.md)s are bound to groups to define particle behavior. A group may have a ParticleRenderer to define presentation for the particles in the group.

## New Particle Semantics ##

When a new particle is created in a group, it is not immediately visible. `len(group)` and `iter(group)` do not reflect the addition of the new particle until the next time the group is updated. The update method incorporates new particles into the group so that they are visible and active.

New particles are not visible and active at first because they are often created in the middle of an update iteration. If they were immediately visible, they would get updated by any controllers that had not yet been invoked, but they would not get updated by controllers that were already finished for that iteration. This could result in inconsistent particle behavior or presentation. After incorporation in the next update, all controllers will have an opportunity to update the new particles before they are rendered.

## Mutating Groups During Iteration ##

It is safe to create and kill particles while iterating a group. Unlike other mutable containers like dictionaries and lists, groups are specifically designed to handle this case.

It is also safe to bind or unbind controllers from a group during update. The change to the controller list will not go into effect until the next update, however. Thus it is impossible to prevent a controller from executing by unbinding it once an update has started.

## Class Reference ##

```
class ParticleGroup(object)
 |  Group of particles that share behavior via controllers
 |  and are rendered as a unit
 |  
 |  ParticleGroup(controllers=(), renderer=None, system=lepton.default_system)
 |  
 |  Initialize the particle group, binding the supplied
 |  controllers to it and setting the renderer.
 |  
 |  If a system is specified, the group is added to that particle system
 |  automatically. By default, the group is added to the default particle
 |  system (particle.default_system). If you do not wish to bind the group to a
 |  system immediately, pass None for the system.
 |  
 |  ----------------------------------------------------------------------
 |  Data descriptors defined here:
 |  
 |  controllers
 |      Controllers bound to this group
 |  
 |  renderer
 |      Renderer bound to this group
 |  
 |  system
 |      Particle system this group belongs to
 |  
 |  ----------------------------------------------------------------------
 |  Methods defined here:
 |  
 |  __iter__()
 |      Return an iterator that generates ParticleProxy objects for each active particle
 |  
 |  __len__()
 |      Return the number of active particles in the group.
 |  
 |  bind_controller(*controller)
 |      Bind one or more controllers to the group
 |  
 |  draw()
 |      Draw the group using its renderer (if any)
 |  
 |  kill(particle)
 |      Destroy a particle in the group.
 |  
 |  killed_count()
 |      Return number of killed particles not yet reclaimed
 |      Useful for engine debugging and profiling, not typically
 |      used by applications
 |  
 |  new(template)
 |      Create a new particle in the group with attributes
 |      copied from the specified template particle.
 |      Return a ParticleProxy for the new particle.
 |      Note new particles are not visible until
 |      they are incorporated by calling the update()
 |      method.
 |  
 |  new_count()
 |      Return the number of new particles not yet incorporated
 |  
 |  set_renderer(renderer)
 |      Set the renderer for the group. This replaces any existing
 |      renderer. Pass None to disable rendering
 |  
 |  unbind_controller(controller)
 |      Unbind a controller from the group so it is no longer
 |      invoked on update. If the controller is not bound to the group
 |      raise ValueError
 |  
 |  update(time_delta)
 |      Incorporate new particles added since the last update,
 |      and optimize the particle list. Then invoke the controllers
 |      bound to the group to update the particles
```