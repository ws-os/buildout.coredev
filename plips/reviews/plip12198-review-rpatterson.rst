Ship Chamelon (2.x)/five.pt with Plone by default
=================================================

PLIP ticket: https://dev.plone.org/ticket/12198

Review by Ross Patterson (me@rpatterson.net, zenwryly@irc.freenode.net)

The PLIP was reviewed on Ubuntu 12.04 using python 2.7.3 and Chromium 29.


Review steps
------------

- Since there's no PLIP buildout config, set up the buildout by adding
  `five.pt`_ to the eggs in my own ``local.cfg``::

  $ bin/buildout -c local.cfg -N

- Ran all the tests::

  $ bin/alltersts

- Added `five.pt`_ to the eggs for a client project with a bunch of
  add-INS and tested all the add-on templates I could think of.


Notes and observations
----------------------

- I ran into no issues whatsoever.

- Didn't notice any perceivable performance improvements.


Conclusion
----------

This new version of Chameleon definitely addresses the largest
consideration, being more fragile in it's template parsing.  I saw no
issues with any templates whatsoever.

I still have a concern about the performance improvements that have been
claimed throughout the life of the Chameleon project.  The only
benchmarks I've read about have been artificial and I've been
suggesting all along that someone do some benchmarks that are closer
to real-world.  At the very least, I would like to see some benchmarks
showing the difference Chameleon makes on OOTB Plone templates.  Until
we have that, I'd suggest we be careful about bandying about these
"20%" claims.  My apologies if I've missed any benchmarks that have
been done on closer to real-world template usage.

That said, I'll defer to those who seem very confident it makes a
significant performance difference and call myself a +1 for merging.
