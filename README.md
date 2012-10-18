# Frycook

Frycook is a quick and dirty alternative to Chef.  When a giant kitchen full of equipment and a full staff headed by a master chef are *WAY* more than you need, a frycook will do the trick nicely.  It'as also nice when you'd rather do things in Python than have to learn Ruby or some other proprietary DSL.

Frycook is a thin layer on top of fabric, cuisine, and mako.  Frycook consists of the frycook package you install via pip and a set of recipes and metadata files you generate to describe your environment and how to build it.  I'll call this set of recipes and metadata a _globule_ in this document.  There is a sample globule in the sample directory that handles a base server setup and web domain setup for a simple web server, based on Ubuntu 12.04.

The ideas embodied in frycook originally came from managing several hundred servers for a social gaming company, and then were augmented by time spent poring over the chef documentation.

The best way to learn frycook is to look through the code in the frycook module and in the sample globule that's included with this repo.  I've also generate API documentation from the somewhat extensive docstrings I've included in the source files.  You can acces those *here*.

# Story

When I build a server, I prefer to write a program to build the server instead of just logging in to the server and configuring things by hand.  This way, when the server eventually crashes (or I need to build a second copy to scale things up), I have a program that can quickly build a new copy of that server for me.  Later, when I need to change the configuration on the server, I just update the setup program and run that again.  That way my setup program is always up to date.  

The first way I did this was by writing bash scripts that made liberal use of ssh to connect to the server and run commands on it.  This works pretty good.  Eventually, though, you end up with a huge pile of bash code that gets unwieldy pretty quickly.  Combine that with the fact that I find bash to be a little opaque and I finally went looking for a better solution.  I checked out all of the popular solutions (Chef, Puppet, etc...) and didn't find one that felt like a good fit for me.  They all have lots of complex infrastructure to setup and require learning things like Ruby or proprietary DSLs, none of which caught my fancy.

I spend most of my time writing code in Python, so I decided that a Python solution would be most palatable.  This inevitably led to fabric, which is the go-to solution for handling ssh, and cuisine, which adds a bunch of neat commands on top of fabric to handle lots of common administrative tasks on remote servers.  Still, though, these weren't enough.  I wanted to add in the ability to track *all* the metadata for my servers in one central place (but not in a database server), and to be able to generate configuration files from templates (I like Mako for this).

Finally, I decided to bite the bullet and roll my own system.  I figured it would be fun and give me a chance to get up close and personal with fabric and cuisine and learn a bit about how they work internally.  Also, this way I would have complete control of my build system and understand it intimately.  Sometimes I'm kind of a control freak that way.

Although I didn't choose to use chef or puppet, I really liked some of their concepts and I decided to make them part of the foundation for my system.  Chef has the nice concept of recipes for individual subsystems and cookbooks for building multiple subsystems into complete systems.  Chef also places a lot of value on idempotency, making sure that you can run the same setup script on a server multiple times and not cause yourself problems in the process.

What I ended up with was a package you install with pip and a directory structure for storing the metadata, config files, recipes, and cookbooks for my environment.  I couldn't think of a better name, so I ended up calling this directory structure and its contents a globule (frycooks deal with lots of grease).  The package you install has the base recipe and cookbook classes and a program to process everything (called, appropriately, frycooker).
