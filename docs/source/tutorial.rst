Intro
=====

In this tutorial we'll use the sample globule to create a simple web
server using Nginx and running on Ubuntu 12.04 server.  It assumes you
already have a functioning Ubuntu 12.04 server with nothing installed
other than the basic operating system load.

Initial Server Build
====================

The first thing you'll want to do is to install the python requirements
on the machine you'll be running frycooker.py on.  They're contained in
the ``requirements.txt`` file in the setup directory of the sample globule.
I generally use a virtualenv for things like this, but it will work just
fine if installed globally as well::

    $ cd sample/setup
    $ pip install -r requirements.txt

When starting with a new server, you'll just want to apply all the
cookbooks and recipes that it needs at once. Since those are defined in
the server's environment under its components section, frycook knows
everything it needs to do to load the new server.  You just have to tell
it to apply the components to the server and it will do its thing::

    $ cd sample/setup
    $ ./runner.sh -a -p -O dev

Several things are worth noting here.  First, we're running frycooker.py
using the ``runner.sh`` wrapper script.  It sets the PYTHONPATH environment
variable, then invokes frycooker.py.

The second thing to note is the set of command-line arguments we used.
The ``-a`` argument tells frycooker.py to apply the components defined
in the environment to the server.  The ``-p`` argument tells
frycooker.py to update the package manager on the server we're loading
before applying the recipes and cookbooks (in this case, it's the
equivalent of running ``sudo apt-get update``).  The ``-O`` argument
tells frycooker.py that the cookbooks and recipes are allowed to be as
disruptive as necessary when running on the target server.  Finally, the
``dev`` argument tells frycooker.py to run against the computer named
dev in the environment.

The last noteworthy thing is that frycooker.py will expect to be able to
resolve the ``dev`` name to an ip address to ssh to and apply the
recipes and cookbooks.  Therefore you'll want to either have a hosts
entry for it or have added it to your dns server configuration before
running this.

Continued Maintenance
=====================

After you have been running your new server for a while, you'll probably
want to make changes to it.  You'll do this by updating your recipes as
necessary, then running them against the server.  If you've just made a
small change to a single recipe, you'll want to apply it to the server
without re-running all of the cookboks and recipes that make up the
server.  For the sake of this tutorial, let's assume you have changed
the example_com recipe and want to apply it to the server. Here's how
you'll do that::

    $ cd sample/setup
    $ ./runner.sh -r example_com dev

Note that here we've used the ``-r`` command-line parameter to specify a
single recipe to run.  We've also specified the ``dev`` server to run it
against.  We always have to specify a server when running frycooker.py.

If you wanted to apply a whole cookbok instead of a single recipe, you'd
do it like this::

    $ cd sample/setup
    $ ./runner.sh -c web dev

In this case we're applying the web cookbook, as specified with the ``-c``
argument.

You could also, of course, just re-apply everything to the server if
you've made a bunch of changes::

    $ cd sample/setup
    $ ./runner.sh -a -p dev

Noe that I didn't use the ``-O`` parameter here so that it would be nice
to the server and not disrupt users any more than necessary.

That outlines the major functions you'll perform with frycook.  This
tutorial didn't go into the details of creating recipes.  That's an
exercise left up to the reader.
