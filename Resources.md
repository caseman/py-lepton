### Other Particle Systems ###

http://particlesystems.org
> An open source particle engine and API written in C++. A strong source of ideas and inspiration. Lepton [Domain](Domain.md)s in particular are inspired by this project.

[Spark Particle Engine](http://spark.developpez.com/)
> Open-source C++ particle engine with OpenGL and SFML renderers

[Flint Particle System](http://flintparticles.org/)
> Open source Actionscript particle system. It's approach to a modular api is similar to Lepton's. Has some very cool features.

[Java Open Particle System](http://jops.softmed.org/)
> An open source particle engine for Java. Has a nice GUI for creating particle effect.s

http://www.wondertouch.com/
> A very nice looking commercial particle engine. Nice examples to draw inspiration from.

http://www.fxpression.com/
> Open source particle plug-in for Ogre3D

[VEE -- Visual Effects Engine](http://www.tml.tkk.fi/~tilmonen/vee/home.html)
> "2nd order" Open source particle effects engine with Python support

### Papers and Pages ###

[Hugo Elias' Models page](http://freespace.virgin.net/hugo.elias/models/m_main.htm)
> Basic, practical examples of particles and simulating various real-life materials such as strings and cloth.

[Building an advanced particle system](http://www.gamasutra.com/features/20000623/vanderburg_01.htm)
> Gamasutra article with some ideas, not sure how "advanced" they are, but they are good basics.

[More accurate volumetric particle renderering](http://www.gamasutra.com/view/feature/3680/a_more_accurate_volumetric_.php)
> Talks about solving boundary artifacts when volume filling particles (e.g., smoke) contact solid surfaces in 3D scenes. Something to consider for future Lepton functionality.

[GPU-based particle system](http://ati.amd.com/developer/techreports/2004/GH2004/Kipfer-UberFlow%20A%20GPU-Based%20Particle%20Engine(GH04)-Slides.pdf)
> Particle system implemented using shaders. Contains some valuable ideas and datastructures for any particle system, however.

[Particle effects presentation](http://2ld.de/gdc2007/EverythingAboutParticleEffectsSlides.pdf)

[Building a million-particle system](http://www.gamasutra.com/features/20040728/latta_01.shtml)
> Some ideas on scaling up particle effects, mostly involving pushing the work off to the GPU.

[A Particle System for Interactive Visualization of 3D Flows](http://wwwcg.in.tum.de/Research/data/Publications/tvcg05.pdf)
> Can simulate a flow involving millions of particles at interactive rates on the GPU

[Optimizing the rendering of a particle system](http://realtimecollisiondetection.net/blog/?p=91)
> Strategies for speeding up particle drawing

### Coding Resources ###

[Fast random-number generators](http://www.cse.yorku.ca/~oz/marsaglia-rng.html)
[Ziggurat method for generating normally distributed random numbers](http://www.jstatsoft.org/v05/i08/supp/1)
> The basis for the fast random number generators used by lepton.


[Multi-threading particle systems](http://cowboyprogramming.com/2007/01/05/multithreading-particle-sytems/)
> Leveraging parallel processing capabilities of modern PCs for better particle system performance.

[Optimizing OpenGL throughput on Mac OS X](http://developer.apple.com/graphicsimaging/opengl/optimizingdata.html)

[Matrix and Quaternion FAQ](http://www.flipcode.com/documents/matrfaq.html)
> Put this in your pipe and smoke it.

[Using unittest effectively](http://palladion.com/home/tseaver/obzervationz/2008/unit_testing_notes-20080724)
> A nice overview of some non-obvious tips for writing effective unittests, by my old friend and former colleague, Tres Seaver.