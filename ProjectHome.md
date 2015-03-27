## Lepton: A high-performance, pluggable particle engine and API for Python ##

**Lepton is designed to make complex and beautiful particle effects possible, and even easy from Python programs.**

Lepton provides the following core features:

  * Native-code core for high-performance particle dynamics and rendering
  * Pluggable particle controllers for specifying particle behavior
  * Two pluggable OpenGL renderers, and two pygame renderers
  * Spacial domains, used to control particle emission and behavior
  * Easy to use and powerful texture support, including animation
  * Modular architecture that lets you easily configure and customize the engine

The code includes several examples of how you can use the engine (using pyglet and pygame). Note the engine itself does not depend on any other 3rd-party Python libraries and simply requires the application to setup an OpenGL context in order to render particles.

### Learn More ###

  * **[Overview](Overview.md)** -- The 10,000 foot view of Lepton
  * **[Code Example](http://code.google.com/p/py-lepton/source/browse/trunk/examples/splode.py)** -- A complete 3D explosion example program using Lepton and Pyglet
  * **[Change Log](http://code.google.com/p/py-lepton/source/browse/trunk/CHANGES.txt)** -- Changes in the latest release
  * **RoadMap** -- Where Lepton is headed
  * **[Latest Checkins](http://code.google.com/p/py-lepton/source/list)** -- What's cooking in the subversion repo


### Applications ###

Lepton was used for our pyweek entry Fishing Frenzy:
  * **[Fishing Frenzy Pyweek Page](http://www.pyweek.org/e/CampInvisible/)**
  * **[Browse the Source Code](http://code.google.com/p/caseman/source/browse/#svn/trunk/fishing_frenzy)**

If you are working on an application using lepton, let us know!