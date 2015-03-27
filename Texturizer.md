Texturizers work with OpenGL renderers to apply textures to particles during rendering. A texturizer object is invoked by a renderer from its `draw()` method and performs the following duties:

  * Make the appropriate OpenGL state changes to enable texturing
  * Generate the proper texture coordinates for each particle
  * Restore the OpenGL state

## Texturizer Types ##

Lepton includes two types of texturizers:

_SpriteTexturizer_

> Applies a set of static texture coordinates from a single resident texture to a particle group. Note that different coordinates from the texture can be assigned to different particles, allowing you to use a sprite sheet to texture particles in the same group differently. If no specific texture coordinates are supplied, the entire texture is applied to all particles in the group by default.

_FlipBookTexturizer_

> Animates a set of texture coordinates from a single resident "flipbook" texture for each particle in a group according to each particle's age. Each frame is given as a set of texture coordinates. The duration of the frames may be uniform or individually specified. The animation can be set to loop continuously through the frames or stop at the final frame.

## Using Texturizers ##

Texturizers work with the PointRenderer and BillboardRenderer classes. If a texturizer is specified when instantiating one of these renderer objects, it will automatically use it when drawing. Note that texturing points uses point sprites, which are limited to using the entire texture for each particle. Therefore the PointRenderer only works with the SpriteTexturizer, and will ignore custom texture coordinates. For complete texturizer support, use the BillboardRender.

Here is some example code using pyglet textures with a simple SpriteTexturizer and BillboardRenderer:
```
texture = pyglet.image.load('my_texture.png').get_texture()
my_renderer = renderer.BillboardRenderer(texturizer.SpriteTexturizer(texture.id))
```

## Sprite Texturizers ##

Use a sprite texturizer when you want to apply one or more fixed textures to your particles. In their simplest usage, a sprite texturizer simply applies the entire texture to all particles renderered in the group. However, with more advanced arguments you can use a single sprite texturizer to apply different parts of a texture to various particles like a sprite sheet or texture atlas. Although you do not directly control the texture coordinates assigned to specific particles, you can control the frequency of each set of coordinates to apply some coordinates more than others as desired.

```
 |  SpriteTexturizer(texture, coords=(), weights=(), filter=GL_LINEAR, wrap=GL_CLAMP, 
 |                   aspect_adjust_width=False, aspect_adjust_height=False)
 |  
 |  texture -- OpenGL texture name, acquired via glGenTextures. It is up
 |  to the application to load the texture's data before using the texturizer
 |  
 |  coords -- A sequence of texture coordinate sets. Each set consists of coordinates
 |  for the four corners of the quad drawn for a particle (8 floats). Sets may
 |  consist of 4 coordinate pairs (tuples) or simply 8 floats corresponding to the
 |  bottom left, bottom right, top right and top left texture coordinates respectively.
 |  if omitted, coords defaults to a single set of coordinates: (0,0, 1,0, 1,1, 0,1)
 |  
 |  weights -- An optional list of weight values applied to the coordinate sets
 |  specified in coords. Must have the same length as the coords sequence.
 |  If specified, the coordinate set is randomly chosen for each particle in
 |  proportion to its weight. If not specified, the the coordinates are
 |  assigned evenly to the particles
 |  
 |  filter -- The OpenGL filter used to scale the texture when rendering.
 |  One of: GL_NEAREST, GL_LINEAR, GL_LINEAR_MIPMAP_LINEAR, etc.
 |  
 |  wrap -- The OpenGL wrapping parameter for texture application when
 |  rendering. One of: GL_CLAMP, GL_REPEAT, GL_CLAMP_TO_EDGE, etc.
 |  
 |  aspect_adjust_width, aspect_adjust_height -- These two flags
 |  are used to match the aspect ratio of the particle's width and height
 |  to the dimensions of its texture coordinates. This is useful to
 |  match particles to textures of various dimensions without distortion.
 |  If one flag is set, the texturizer adjusts the width or height of the
 |  particle size respectively as appropriate. Only one of these flags
 |  may be set at one time.
```

You also have the option to initialize the sprite texturizer from a sequence of pyglet images using the `from_images()` class method. This combines the input images into a single resident texture atlas and generates the proper texture coordinates to apply each image to particles. If you are not using pyglet, you can still use the default constructor above, but you must construct the texture atlas manually.

```
 |  from_images(cls, images, weights=None, filter=None, wrap=None, 
 |              aspect_adjust_width=False, aspect_adjust_height=False)
 |      Create a SpriteTexturizer from a sequence of Pyglet images. 
 |      
 |      Note all the images must be able to fit into a single OpenGL texture, so
 |      their combined size should typically be less than 1024x1024
```

Here is some example code using this method (also see the [flyby example](http://code.google.com/p/py-lepton/source/browse/trunk/examples/flyby.py)):

```
images = [pyglet.image.load('texture%s.png' % (i+1)) for i in range(4)]
my_renderer=renderer.BillboardRenderer(texturizer.SpriteTexturizer.from_images(images)))
```

## Flip Book Texturizers ##

A flip book texturizer allows you to apply a sequence of texture frames to your particles. This provides a way to animate each particle. The texturizer works by providing it a texture atlas that contains the frame images and a sequence of texture coordinate sets that locate them in the atlas. Each particle's age determines its current current animation frame.

```
     |  FlipBookTexturizer(texture, coords, duration, loop=True, dimension=2, 
     |                     filter=GL_LINEAR, wrap=GL_CLAMP, 
     |                     aspect_adjust_width=False, aspect_adjust_height=False)
     |  
     |  texture -- OpenGL texture name, acquired via glGenTextures. It is up
     |  to the application to load the texture's data before using the texturizer
     |  
     |  coords -- A sequence of texture coordinate sets. Each set is used as one
     |  frame of the animation. Each set consists of coordinates for the four
     |  corners of the quad drawn for a particle (8 or 12 floats). When using 8
     |  floats, the r value of the texture coordinates is set to 0.
     |  
     |  duration -- The time duration of each frame. This may be specified as a
     |  single value, if the duration of all frames are the same, or as a sequence
     |  of individual duration values for each frame.
     |  
     |  loop -- If true (the default), the animation will continuously loop through
     |  the frames from last back to first. If false, the animation will stop on the
     |  last frame.
     |  
     |  dimension -- The number of texture coordinate dimensions. Use 2 for 2D
     |  textures (the default) or 3 for 3D textures.
     |  
     |  filter -- The OpenGL filter used to scale the texture when rendering.
     |  One of: GL_NEAREST, GL_LINEAR, GL_LINEAR_MIPMAP_LINEAR, etc.
     |  
     |  wrap -- The OpenGL wrapping parameter for texture application when
     |  rendering. One of: GL_CLAMP, GL_REPEAT, GL_CLAMP_TO_EDGE, etc.
     |  
     |  aspect_adjust_width, aspect_adjust_height -- These two flags
     |  are used to match the aspect ratio of the particle's width and height
     |  to the dimensions of its texture coordinates. This is useful to
     |  match particles to textures of various dimensions without distortion.
     |  If one flag is set, the texturizer adjusts the width or height of the
     |  particle size respectively as appropriate. Only one of these flags
     |  may be set at one time.
```

Similar to the sprite texturizer above, there is an alternate constructor for creating a flipbook texturizer from a sequence of pyglet images (see also the [logo example](http://code.google.com/p/py-lepton/source/browse/trunk/examples/logo.py)):

```
     |  from_images(cls, images, duration, loop=True, dimension=2, filter=None, 
     |              wrap=None, aspect_adjust_width=False, aspect_adjust_height=False)
     |      Create a FlipBookTexturizer from a sequence of Pyglet images
     |      
     |      Note all the images must be able to fit into a single OpenGL texture, so
     |      their combined size should typically be less than 1024x1024
```

## Generating Point Textures ##

Many types of particle effects can be made by drawing the particles as points that blend smoothly into the background or adjacent particles. Creating these types of textures manually can be tedious and often you will need to tweak the blending to make it look just right.

Lepton supplies a `create_point_texture()` function so that you can programatically generate blended point textures. These textures can then be used directly by texturizers.

```
    create_point_texture(size, feather=0)
        Create and load a circular grayscale image centered in a square texture
        with a width and height of size. The radius of the circle is size / 2. 
        Since size is used as the texture width and height, it should typically
        be a power of two.
        
        Feather determines the softness of the edge of the circle. The default,
        zero, creates a hard edged circle. Larger feather values create softer
        edges for blending. The point at the center of the texture is always
        white.
        
        Return the OpenGL texture name (id) for the resulting texture. This
        value can be passed directy to a texturizer or glBindTexture
```

Using this function with texturizers is quite easy:

```
texture = texturizer.create_point_texture(64, 1)
my_renderer = renderer.PointRenderer(texturizer.SpriteTexturizer(texture))
```

Here are some example particle textures created by `create_point_texture()` at various feather values shown against a black background:

<img src='http://py-lepton.googlecode.com/svn/wiki/images/feather-0.01.png' title='feather=0.01' width='64' height='64'>
<img src='http://py-lepton.googlecode.com/svn/wiki/images/feather-0.5.png' title='feather=0.5' width='64' height='64'>
<img src='http://py-lepton.googlecode.com/svn/wiki/images/feather-1.0.png' title='feather=1.0' width='64' height='64'>

<i>Note:</i> Use of this function requires pyglet.<br>
<br>
<h2>Texturizer Interface</h2>

The texturizer interface consists of three methods invoked by the renderer during <code>draw()</code>:<br>
<br>
<pre><code>     class Texturizer<br>
     |  Sets and restores texture state and defines the particle texture coordinates.<br>
     |  <br>
     |  set_state() -&gt; None<br>
     |      Setup the OpenGL texture state for rendering.<br>
     |      Called by the renderer before particles are drawn.<br>
     |<br>
     |  generate_tex_coords(group) -&gt; FloatArray<br>
     |      Generate texture coordinates for the given particle<br>
     |      group and return them in a FloatArray object.<br>
     |      Called by the renderer when particles are drawn.<br>
     |  <br>
     |  restore_state() -&gt; None<br>
     |      Restore the OpenGL texture state after rendering.<br>
     |      Called by the renderer after particles are drawn.<br>
</code></pre>