## Requirements
POX requires Python 2.7.  In practice, it also mostly runs with Python 2.6, but nobody is presently trying to push or support this.

POX officially supports Windows, Mac OS, and Linux (though it has been used on other systems as well).  A lot of the development happens on Mac OS, so it almost always works on Mac OS.  Occasionally things will break for the other OSes; the time it takes to fix such problems is largely a function of how quickly problems are reported.  In general, problems are noticed on Linux fairly quickly (especially for big problems) and noticed on Windows rather slowly.  If you notice something not working or that seems strange, please submit an issue on the github tracker or send a message to the pox-dev mailing list so that it can be fixed!

POX can be used with the "standard" Python interpreter (CPython), but also supports PyPy (see below).

Getting the Code
The best way to work with POX is as a git repository.  You can also grab it as a tarball or zipball, but source control is generally a good thing.

POX is hosted on github.  If you intend to make modifications to POX itself, you might consider making your own github fork of it from the POX repository page.  If you just want to grab it quickly to run or play around with, you can simply create a local clone:

$ git clone http://github.com/noxrepo/pox
$ cd pox
Selecting a Branch / Version
The POX repository has multiple branches.  Specifically, it has at least some release branches and at least one active branch.  The default branch (what you get if you just do the above commands) will be the most recent release branch.  Release branches may get minor updates, but are no longer being actively developed.  On the other hand, active branches are being actively developed.  In theory, release branches should be somewhat more stable (by which we mean that if you have something working, we aren't going to break it).  On the other hand, active branches will contain improvements (bug fixes, new features, etc.).  Whether you should base your work on one or the other depends on your needs.  One thing that may factor into your decision is that you'll probably get better support on the mailing list if you're using an active branch (lots of answers start with "upgrade to the active branch").

As of this writing, the active branch is named betta.  To use the betta branch, you simply check it out after cloning the repository:

~$ git clone http://github.com/noxrepo/pox
~$ cd pox
~/pox$ git checkout betta
PyPy Support
While it's not as heavily tested as the normal Python interpreter, it's a goal of POX to run well on the PyPy Python runtime.  There are two advantages of this.  First, PyPy is generally quite a bit faster than CPython.  Secondly, it's very easily portable – you can easily package up POX and PyPy in a single tarball and have them ready to run.

You can, of course, download, install, and invoke PyPy in the usual way.  On Mac OS and Linux, however, POX also supports a really simple method: Download the latest PyPy tarball for your OS, and decompress it into a folder named "pypy" alongside pox.py.  Then just run pox.py as usual (./pox.py), and it should use PyPy instead of CPython.

