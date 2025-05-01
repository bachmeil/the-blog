---
title: Matlab startup problem on Ubuntu 22.04
layout: post
---
On starting a new Matlab installation on Ubuntu, I got the message

```
Gtk-Message: 15:19:26.632: Failed to load module "canberra-gtk-module"
X11Util.Display: Shutdown (JVM shutdown: true, open (no close attempt): 1/1, reusable (open, marked uncloseable): 0, pending (open in creation order): 1)
X11Util: Open X11 Display Connections: 1
X11Util: Open[0]: NamedX11Display[:0, 0x72e178000fb0, refCount 1, unCloseable false]
```

I got something like this inside Matlab

```
com.jogamp.opengl.GLException: X11GLXDrawableFactory - Could not initialize shared resources for X11GraphicsDevice[type .x11, connection :0, unitID 0, handle 0x0, owner false, ResourceToolkitLock[obj 0x413b9df1, isOwner false, <30fe5aac, af2eb55>[count 0, qsz 0, owner <NULL>]]]
  at jogamp.opengl.x11.glx.X11GLXDrawableFactory$SharedResourceImplementation.createSharedResource(X11GLXDrawableFactory.java:326)
  at jogamp.opengl.SharedResourceRunner.run(SharedResourceRunner.java:297)
  at java.lang.Thread.run(Unknown Source)
Caused by: com.jogamp.opengl.GLException: glXGetConfig(0x1) failed: error code Unknown error code 6
  at jogamp.opengl.x11.glx.X11GLXGraphicsConfiguration.glXGetConfig(X11GLXGraphicsConfiguration.java:570)
  at jogamp.opengl.x11.glx.X11GLXGraphicsConfiguration.XVisualInfo2GLCapabilities(X11GLXGraphicsConfiguration.java:500)
  at jogamp.opengl.x11.glx.X11GLXGraphicsConfigurationFactory.chooseGraphicsConfigurationXVisual(X11GLXGraphicsConfigurationFactory.java:434)
  at jogamp.opengl.x11.glx.X11GLXGraphicsConfigurationFactory.chooseGraphicsConfigurationStatic(X11GLXGraphicsConfigurationFactory.java:240)
  at jogamp.opengl.x11.glx.X11GLXDrawableFactory.createMutableSurfaceImpl(X11GLXDrawableFactory.java:524)
  at jogamp.opengl.x11.glx.X11GLXDrawableFactory.createDummySurfaceImpl(X11GLXDrawableFactory.java:535)
  at jogamp.opengl.x11.glx.X11GLXDrawableFactory$SharedResourceImplementation.createSharedResource(X11GLXDrawableFactory.java:283)
  ... 2 more
  ```

  The solution was to start Matlab with this option:

  ```
  matlab -softwareopengl
  ```

  It says OpenGL startup options will be removed, but I will probably use the same version of Matlab for five years, so I'll be okay.