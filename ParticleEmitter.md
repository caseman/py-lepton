Particle emitters create particles in a ParticleGroup. They are actually a special type of ParticleController and can be bound to a group, if desired. Like a controller, an emitter is a callable object. An emitter can be configured to emit particles over time. Each time the emitter is called, it will create a certain number of new particles in the specified group.

When an emitter creates particles, the initial value for each attribute is calculated using several possible input sources.

  * Template particle
  * Discrete values
  * [Domain](Domain.md)
  * Deviation particle

The simplest way to set the initial attribute values for emitted particles is via the template particle. If specified alone, all new particles are simply an exact copy of the template. Typically, however you do not want all new particles to have the same initial state for all attributes, and that's where the other inputs come in.

You can specify multiple initial values for specific attributes using discrete values. These are passed to the emitter as keyword arguments matching the names of the particle attributes. The discrete argument values are a sequence of the allowed values for each attribute. When a particle is created by the emitter, one of these discrete values is chosen at random to use as the particle's initial attribute value. Note that discrete values override values specified in the particle template.

Discrete values are useful when you have a few select attribute values that you want, but many times a range of values is needed instead of a discrete set. You can use a [Domain](Domain.md) as an attribute input for vector attributes (e.g., position, velocity, color, etc) to specify a range of possible values. Domains describe a particular region of space, and come in a variety of shapes. You specify a domain using keyword arguments just as with discrete values, however the argument value is a domain object instead of sequence. When the emitter creates a new particle, the domain's `generate()` method is called to generate a random point inside the domain and this value is used as the initial attribute value. In fact, the input object need not be a full-fledged domain, it just needs to define a `generate()` method that returns a sequence of 3 numbers to construct a 3D vector.

The template particle, discrete value and domain inputs can all be augmented via an additional deviation value. These values are specified as attributes of an optional deviation template. Without a deviation template, the emitter uses the attribute inputs directly as the initial values of the cooresponding attributes of the generated particles. With a deviation template, the attribute inputs become the mean attribute values, with the deviation template specifying the desired standard deviation for each attribute from the mean. Attributes with a non-zero deviation value are randomized using a normal random function using the attribute input value as the mean. Vector attributes are specified as a sequence of deviations, so each axis may be given a different deviation, if desired.

A single emitter can source various particle attributes using all of the above methods:
```
emitter = StaticEmitter(
	template=Particle(
		velocity=(0, 10, 0),
		size=(2, 2, 0),
		),
	color=[(1,0,0), (0,1,0), (0,0,1)],
	position=domain.Line((0, 0, 0), (800, 0, 0)),
	deviation=Particle(
		velocity=(2, 1, 2),
		age=2,
		),
	)
```

The emitter above will generate the initial particle attribute values as follows:

  * The `size` of all particles will be (2, 2, 0).
  * The `color` will be red, green or blue chosen at random.
  * The `velocity` will have a mean value of (0, 10, 0) with a random deviation of (2, 1, 2).
  * The `position` will have a random value between (0, 0, 0) and (800, 0, 0).
  * The `age` will have a mean value of 0 (the default) and a random deviation of 2. Note that ages of new particles are not allowed to be negative, and are thus clamped at 0.
  * The remaining attributes (up, rotation, etc.) will be 0 or null vectors (the default if not specified).

In addition to specifying the character of the initial particle attributes, emitters also accepts a `rate` and `time_to_live` value. `rate` specifies how many particles per unit time are emitted into the ParticleGroup that the emitter is bound to. `time_to_live` is an optional value, which specifies how much time the emitter should be bound to its group. After this time expires, the emitter will automatically unbind itself from its group and thus stop emitting particles. Note the `rate` and `time_to_live` may be adjusted after constructing an emitter by modifying their attributes on the emitter object.

Emitters can also create an arbitrary number of particles instantly by calling their `emit()` method. This method accepts a particle count and destination group as its arguments.

Two types of emitters are currently included with lepton:

  * StaticEmitter -- Emits particles at a regular rate over time, or can emit an arbitrary number at once
  * PerParticleEmitter -- Emits particles originating from the positions of all existing particles in a group. Useful for creating trails of particles. See the fireworks example included in lepton.
