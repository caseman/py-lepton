Particle systems are containers for [ParticleGroup](ParticleGroup.md)s and provide convenient methods for updating and drawing all groups they contain. You can also bind [ParticleController](ParticleController.md)s to a particle system. These particle controllers are applied to all groups, and are useful if your application has global particle behavior shared by all particles (such as movement or gravity). Groups may bind additional controllers to specialize their [Particle](Particle.md)'s behavior.

The use of particle systems is for convenience and is entirely optional. Applications can instead opt to update and draw particle groups individually, if desired.

The methods for ParticleSystem objects are as follows:

```
class ParticleSystem
 |  
 |  __contains__(self, group)
 |      Return True if the specified group is in the system
 |  
 |  __init__(self, global_controllers=())
 |      Initialize the particle system, adding the specified global
 |      controllers and renderers, if any
 |  
 |  __iter__(self)
 |      Iterate the system's particle groups
 |  
 |  __len__(self)
 |      Return the number of particle groups in the system
 |  
 |  add_global_controller(self, *controllers)
 |      Add a global controller applied to all groups on update
 |  
 |  add_group(self, group)
 |      Add a particle group to the system
 |  
 |  draw(self)
 |      Draw all particle groups in the system using their renderers.
 |      
 |      This method is convenient to call from you Pyglet window's
 |      on_draw handler to redraw particles when needed.
 |  
 |  remove_group(self, group)
 |      Remove a particle group from the system, raise ValueError
 |      if the group is not in the system
 |  
 |  run_ahead(self, time, framerate)
 |      Run the particle system for the specified time frame at the 
 |      specified framerate to move time forward as quickly as possible.
 |      Useful for "warming up" the particle system to reach a steady-state
 |      before anything is drawn or to simply "skip ahead" in time.
 |      
 |      time -- The amount of simulation time to skip over.
 |      
 |      framerate -- The framerate of the simulation in updates per unit 
 |      time. Higher values will increase simulation accuracy, 
 |      but will take longer to compute.
 |  
 |  update(self, time_delta)
 |      Update all particle groups in the system. time_delta is the
 |      time since the last update (in arbitrary time units).
 |      
 |      When updating, first the global controllers are applied to
 |      all groups. Then update(time_delta) is called for all groups.
 |      
 |      This method can be conveniently scheduled using the Pyglet
 |      scheduler method: pyglet.clock.schedule_interval
```