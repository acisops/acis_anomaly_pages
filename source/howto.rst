.. _howto:

HOWTO: Editing ACIS Anomaly Pages
=================================

This document describes how to edit the ACIS Anomaly Pages. 

Clone from GitHub
-----------------

Version control for ACIS Anomaly Pages is handled via git and GitHub. For more
information on how to use them, see the 
`ACIS Operations git Memo <http://cxc.cfa.harvard.edu/acis/memos/git_notes.html>`_.

You need to clone the ``acis_anomaly_pages`` repository from GitHub:

http://github.com/acisops/acis_anomaly_pages

Create a new git branch and make your changes there, then submit a pull request.

reStructuredText
----------------

ACIS Anomaly Pages uses the `Sphinx <http://www.sphinx-doc.org/>`_ documentation
generation software and `reStructuredText <http://docutils.sourceforge.net/rst.html>`_
for rendering HTML documents. For a primer on how to use reStructuredText in Sphinx,
visit http://www.sphinx-doc.org/en/stable/rest.html, where you can learn the markup
used to make the different documents. Thankfully, what you need to edit these docs is
typically a very simple subset of reStructuredText directives. It's also useful to use
the already existing pages for guidance.

Editing an Existing Page
------------------------

Simply pick a page and edit it within your new git branch using the reStructuredText
directives. When you are finished and the changes have been approved by the ACIS team,
make sure that you edit the "last updated" text on the ``index.rst`` page for that
document.

Adding a New Page
-----------------

To add a new page, create a new ``.rst`` file in the ``source`` directory of the repo.
Edit it as you would for an existing page. New pages should have this basic structure
of section headings (with context-specific modifications):

What is it?
+++++++++++

Here is where you write a brief one or two-sentence description of what the anomaly is.

When did it happen before?
++++++++++++++++++++++++++

Make a list here of the times that it has occurred before in the mission, including
obsids. For example, from the :doc:`dpaa_shutdown` page:

The DPA-A has shut down 4 times over the mission:

* October 26, 2000: 2000:300:15:40, obsid 979
* December 19, 2002: 2002:353:20:26, obsid 60915
* January 12, 2015: 2015:012:00:01, obsid 52186
* December 9, 2016: 2016:344:07:40, obsid 17615

Will it happen again?
+++++++++++++++++++++

This section should explain whether or not this particular anomaly is expected to happen
again. In most cases, the answer will be "yes", unless this anomaly has fixed as a result
of a software patch.

How is this anomaly diagnosed?
++++++++++++++++++++++++++++++

This section consists of a set of information which can be used to make sure that the
current anomalous behavior is consistent with previous occurrences of the anomaly and
distinguish it from other possible anomalies. This typically is a list of MSIDs and other
telemetry to check using the ACIS hardware, PMON, and other *Chandra* status pages.

What is the response?
+++++++++++++++++++++

This should be a bulleted list of steps that will need to be taken in the response. This
will include things like:

* Who to notify and when: the ACIS team, the MIT ACIS team, flight directors, etc.
* Whether or not additional comm time will be required to resolve the anomaly.
* If the dump data from the most recent comm needs to be investigated.
* If a CAP should be prepared, and what it should contain.

Impacts
+++++++

This section should describe what the expected impacts of the anomaly are, such as if
all ACIS observations are halted until the anomaly is resolved, or if the anomaly only
affected one observation.

Relevant Procedures
+++++++++++++++++++

In this section, relevant FOT and SOT procedures which may be used in the resolution of
the anomaly should be listed and linked to.

Relevant Notes/Memos
++++++++++++++++++++

Here, list and link to memos and notes providing further information about the anomaly.

Finishing Up
------------

When you are finished, add your new page to the list of pages in the ``index.rst``
file. So if your file was named ``acis_panic.rst``, then you would add this to the
``index.rst`` file:

.. code-block:: rest

    * :doc:`acis_panic` (Last Updated May 12, 2017)

Make sure you add the date that it was updated, which should be the last change after
the team approves it and before the pull request is merged.

Testing Your Changes Locally
----------------------------

You can have a look at the way your pages are rendered on your local machine. For this
to work, you need to have the following packages installed in your local python installation:

* ``sphinx``
* ``sphinx-bootstrap-theme``

You can install these using ``pip`` if you don't have them already:

.. code-block:: bash

    pip install sphinx sphinx-bootstrap-theme

Or, if you don't have write permissions to the Python stack (like on the HEAD LAN),
use the ``--user`` flag:

.. code-block:: bash

    pip install --user sphinx sphinx-bootstrap-theme

Once you have these packages installed, go to the top-level directory of the repo and type
``make html``. The pages will be made inside the directory ``build/html``, and you can read
them with your chosen web browser.

If you want to put the pages up for the ACIS team to see them, copy the contents of ``build/html``
to the ACIS temporary web space at http://cxc.cfa.harvard.edu/acis/tmp/ and give the directory a
name like ``hi_lo_edits`` or something else that is descriptive.

ACIS Team Review
----------------

Once you have checked everything out, submit a pull request to the ``master`` branch of
http://github.com/acisops/acis_anomaly_pages for ACIS team review. In the back and forth
of review you may have to make further changes and add them to the branch / pull request.

Once the review is over and the team has approved the changes, the pull request can be
merged.

Deploying to the CXC Web Space
------------------------------

Once the pull request has been merged, log onto the HEAD LAN as ``acisdude``. Issue
the following commands:

* ``cd ~/python/src/acis_anomaly_pages`` (to go to the directory with the source)
* ``source ~/python/bin/activate.csh`` (to load the Python stack with the appropriate commands)
* ``git pull`` (make sure this completes without errors before running the next one!)
* ``make deploy``

The third command pulls the new changes into the local copy of the repository, and the
final command builds the pages and copies them to the appropriate CXC web space.
