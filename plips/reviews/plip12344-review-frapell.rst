============================================
Review of PLIP12344 / plone.app.contenttypes
============================================

Reviewer: Franco Pellegrini, frapell, frapell@gmail.com

Plattform: Ubuntu 12.10, Python 2.7.3, Firefox 19.0.2, Chromium 25.0.1364.160

plone.app.contenttypes git rev: a14cbe31c5e38e6c33744be66f477630e987d619

Steps
=====

- Used plone.app.contenttypes buildout from:
  ``git@github.com:plone/plone.app.contenttypes.git``

- Ran ``test-plone-4.3.x.cfg``

- Ran ``./bin/test``. All Tests pass.


Notes
=====

- Creating a new site from scratch choosing "Dexterity" as base content-types
  framework worked flawlessly
  
  
Feature parity with AT (compared to vanilla 4.2.4)
==================================================

- File:
    - AT has a "Location" field under "Categorization" that DX lacks.
    - AT has "Allow comments" that DX lacks.
    - AT generates its title from filename, DX doesn't.
    - When adding a content as related, the DX version doesn't show it in the 
      view template.
    - When clicking the "View" tab you don't go to the "/view" view (bug?).
    - DX version allows to setup content rules, where AT doesn't.

- Folder:
    - AT has a "Location" field under "Categorization" that DX lacks.
    - AT has an "allow comments" that DX lacks.
    - DX version allows to choose content as related, and AT doesn't.
    - However, the view template doesn't show the related content.
    - AT version has an extra view template: "Thumbnail view"
    - AT Version provides possibility to restrict which contents are addable.
      DX version needs to enable a behavior for that, should it be enabled by
      default?

- Image:
    - AT has a "Location" field under "Categorization" that DX lacks.
    - AT has "Allow comments" that DX lacks.
    - AT generates its title from filename, DX doesn't.
    - When adding a content as related, the DX version doesn't show it in the 
      view template.
    - When clicking the "View" tab you don't go to the "/view" view (bug?).
    - DX version allows to change the default view and AT doesn't. When choosing 
      "View Image Fullscreen", you can no longer see the green edit bar.
    - AT provides a "Transform" tab that allows basic image manipulation.
      DX doesn't.
    - DX version allows to setup content rules, where AT doesn't.

- Link:
    - AT has a "Change note" that the DX lacks.
    - DX allows to use variables like "navigation_root_url" and "portal_url"
      that get later expanded to create the full url, however, the "view"
      template shows the link with the variable (ie. ${navigation_root_url}/something)
      even thought, when you hover the link, you see the proper url with the
      variable expanded.
    - DX version allows to setup content rules, where AT doesn't.


- News Item:
    - AT has a "Change note" that the DX lacks.
    - AT has an "allow comments" that DX lacks.
    - DX version allows to setup content rules, where AT doesn't.
    - When adding a content as related, the DX version doesn't show it in the 
      view template.


- Document:
    - AT has a "Location" field under "Categorization" that DX lacks.
    - AT has a "Change note" that the DX lacks.
    - AT has "Allow comments", "Presentation mode", "Table of contents", 
      that DX lacks.
    - When adding a content as related, the DX version doesn't show it in the 
      view template.
    - DX version allows to setup content rules, where AT doesn't.


- The labels widget in the AT content types displays existing labels that are
  selectable through a checkbox, existing labels, and possibility to add new ones.
  DX widget, is just a textarea.

- The "allow comments" checkbox gets fixed when enabling the proper behavior.
  Should this behavior be enabled by default?
  
- Going to the "Types" tool and changing the "versioning policy" appears to have
  no effect (Unless p.a.versioningbehavior is installed and configured)
  
- The "Change note" (Versioning), gets solved by adding "plone.app.versioningbehavior"
  and enabling the behavior. Should this be included and enabled by default?
  
- Had to add "collective.dexteritydiff", in order to be able to see diffs,
  however, it doesn't show any diff with different versions.
  
  
Code Reviewing
==============

I don't have much to add to Johannes' review. To me, the code looks impressively 
clean, short, and very readable. I do have a question and a personal opinion:

    - Why is that upgrade step needed? ("Update plone.app.contenttypes fti")
    - We need to increase the amount of comments in the code and docstrings.
      Specially in the interfaces.py file

About your question on plone.app.collection, https://dev.plone.org/ticket/12344#comment:19,
+1 from me to merge the DX version into plone.app.contenttypes. It makes sense 
to me to have all default content types coded in just one package.
Also, it will make migration easier, since you only need to remove the package
with the AT implementation when done.

Also, there's a small list of PEP8/PyFlakes warns and errors: 
http://pastie.org/private/mp2oolnw7m2gcbiynmy2g


Migration
=========

I wasn't able to obtain a successful migration from AT to DX, trying from
different approaches.

When using the released egg in an existing 4.2 site with existing AT content:

    - Migration didn’t work: http://pastie.org/private/5bmlznfxdgymfq6qwp8iw
    - Using Products.contentmigration 2.1.3 as suggested by the documentation 
      didn’t work either, when having a member’s folder, i got this: 
      http://pastie.org/private/jhlyuupz0xjpnlvsbhgbua
    - Removing the Member’s folder and running migration didn’t work either:
      http://pastie.org/private/xcrbecyetlb2jojep8lbg
    - After removing the whole “/Members” folder, migration again failed:
      http://pastie.org/private/k5rcnxi8sfckrpjaxcqma

Existing AT content was a folder, couple of docs, a news item, a file, an image 
and a Link.

Moving the Data.fs from the instance I used to compare feature parity to this
development buildout, running migration and then trying to install the package
resulted in error: http://pastie.org/private/3699tgm5caywklqc33b7q

Trying to create a Plone Site from scratch choosing "Archetypes" as default 
framework resulted in an error: http://pastie.org/private/vqegqcfgjeewrafg9y27pq


Summary
=======

As I mentioned, I think the quality of this implementation is outstanding.
There are a couple of minor things mentioned on the feature parity section
that needs to be fixed, and a couple of others that might need to be fixed (if
we decide we want DX version working *exactly* as AT one).

I do, however, think that we definitely need a clean migration path from AT.

So, from me it's a +0.75 on including this into core. The remaining 0.25 when we 
have a working migration path.
