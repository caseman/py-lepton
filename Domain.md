Domains provide a way to describe regions of space in Lepton. These regions can be used for things like collision detection or to constrain the initial values for attributes of new particles created via a ParticleEmitter.

Below is an abstract base class for domains, describing the contracts it must fulfil:

```
class Domain(object):
	"""Domain abstract base class"""

	def generate(self):
		"""Return a point within the domain as a 3-tuple. For domains with a
		non-zero volume, 'domain.generate() in domain' is guaranteed to return true. 
		"""
	
	def __contains__(self, point):
		"""Return true if point is inside the domain, false if not."""

	def closest_point_to(self, point):
		"""Return the closest point in the domain to the given point
		and the surface normal vector at that point. If the given
		point is in the domain, return the point and a null normal
		vector.

		Note the closest point may not in the domain, if there are
		multiple points in the domain that are closest. In that case
		return the average of the closest points and a null normal
		vector.
		"""
	
	def intersect(self, start_point, end_point):
		"""For the line segment defined by the start and end point specified
		(coordinate 3-tuples), return the point closest to the start point
		where the line segment intersects surface of the domain, and the
		surface normal unit vector at that point in a 2-tuple.  If the line
		segment does not intersect the domain, return the 2-tuple (None,
		None).

		Only 2 or 3 dimensional domains may be intersected.

		Note performance is more important than absolute accuracy with this
		method, so approximations are acceptable.
		"""
```

Several [ParticleController](ParticleController.md)s use domains:

  * CollectorController -- Kills particles as they enter or exit a Domain.
  * BounceController -- Deflects particles off the surface of a Domain.
  * MagnetController -- Attracts or repels particles to or from a Domain.

## Built-in Domains ##

Several domains are included in Lepton and more are planned.

  * LineDomain -- Simple line segment
  * PlaneDomain -- Infinite 2D plane dividing space
  * AABoxDomain -- Axis-aligned rectangular prism
  * SphereDomain -- Spherical region of space
  * DiscDomain -- Circular flat disc
  * CylinderDomain -- Capped circular cylinder
  * ConeDomain -- Right circular cone