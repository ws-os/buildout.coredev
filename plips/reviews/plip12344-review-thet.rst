============================================
Review of PLIP12344 / plone.app.contenttypes
============================================

Reviewer: Johannes Raggam, thet, johannes@raggam.co.at

Plattform: Ubuntu 12.10, Python 2.7.3, Firefox 19.0.2, Chromium 24.0.1312.56  

plone.app.contenttypes git rev: eed52a2f8327b065f6aaafa1553e141d9bfe4d28 

Steps
=====
- Used plone.app.contenttypes buildout from:
  git@github.com:plone/plone.app.contenttypes.git
- Ran test-plone-4.3.x.cfg
- Ran ``./bin/test``. All Tests pass.


Code Reviewing Remarks
======================

- There seems a lot more than 200 test cases, which is good. I don't know about
  the test coverage yet.

- For a more cleaned up code base I suggest to move the XML Schema definitions
  in a explicit directory called "schemata".

- Instead of implementing the event type yourself, you should use
  plone.app.event. It's going to be merged into core very soon and the packages
  are already used in production (http://pypi.python.org/pypi/plone.app.event).
  There are some things, which should be backported to plone.app.event -
  especially the changes to the "Event" folder/collection. Since I'm
  maintaining plone.app.event, let's keep in contact regarding this.

- I like the totally simple (when compared to collective.contentleadimage)
  leadimage behavior implementation. But at first sight I expected more
  plone.app.contenttype specific behaviors in the behavior subpackage. Also,
  the leadimage functionality seemed a bit misplaced in plone.app.contenttypes.
  But then I realized that it's needed for the NewsItem. So it's ok for me.

- There are folder_full_view and folder_full_view_item views. I have created a
  new implementation which is going to be merged in a Plone core package soon
  (when I find the time for it). It fixes the default implementation's
  inability to render Browser-based views properly and should not have any
  troubles with dexterity based contents. I suggest to remove your
  folder_full_view views and use (and fix if necessary) mine. You can find it
  here until it's merged into plone.app.content or the like:
  https://github.com/collective/collective.fullview

- Is it really necessary to have all contenttype views reimplemented in
  plone.app.contenttypes? (Thinking out loud:) If yes, we should consider
  making them generic for DX and AT, make them BrowserView based and put them
  into plone.app.content. Maybe by using the IAccessor concept introduced and
  used in plone.app.event/plone.event as IEventAccessor. This might go beyound
  the scope of this PLIP and touches the responsibility of other PLIPs (13260,
  13283). But we should discuss this, since there is some more space for
  simplifications and improvements.

- I reviewed the related changes in Products.ATContentTypes and
  Products.CMFPlone. Great to see the reduction of interwoven dependencies
  (Archetypes)! I totally appreciate this.

- I could not find any signs of an upgrade path for AT based Collections to DX
  based ones (searching in plone.app.collection and plone.app.contenttypes).
  That should be fixed too.

- When installing the Plone site and choosing the Dexterity Framework, is
  Archetypes still included as dependency and it's GS installation profile just
  hidden or is it not included? If it's still in, the GenericSetup profile
  should be shown, so that integrators can install it, if they need to (e.g.
  for Addon-Products, which still need Archetypes).


TTW Remarks
===========

- Creating an event leads into an error when redirected to the event view::

    URL: /home/thet-data/data/dev/plone/plone.app.contenttypes/plone.app.contenttypes/plone/app/contenttypes/browser/event.pt
    Line 121, Column 32
    Expression: <PathExpr standard:u'portal/icon_export_vcal.png'>

- Creating an Image and uploading works, image scales are created, using a
  custom Title works as well as autogenerating the title from the image name.
  Only the scale names are cryptic (UUID?). I'm not sure if I should like this.
  Back then, when I came to Plone, having meaningful image names was a pro
  criterium for Plone compared to other CMS (e.g. Typo3, which stores all
  images under cryptic UID names).

- Events and NewsItem are shown in the Events and News folders as expected. All
  those, even the "Welcome to Plone" page, seem to be DX based types. Perfect.

- The folder_summary_view on Plone site root doesn't show NewsItem images,
  while the "Summary View" in the News folder does. Looks like if on Plone site
  root the new view isn't used.



Summary
=======

This package is at good quality and the issues mentioned above are just minor
ones which can be addressed when merging. After resolving (or discussing) them,
I'm +1 for merging it into core.


Review TODOs
============

- Check test coverage and plausibility of tests.
- Check Migrations.

