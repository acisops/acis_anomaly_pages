.. _howto:

HOWTO: Editing ACIS Anomaly Pages
=================================

This document describes how to edit the ACIS Anomaly Pages.

Clone from GitHub
-----------------

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

How is this anomaly diagnosed?
++++++++++++++++++++++++++++++

What is the response?
+++++++++++++++++++++

Impacts
+++++++

Relevant Procedures
+++++++++++++++++++

Relevant Notes/Memos
++++++++++++++++++++

Here, list memos with links

When you are finished, add your new page to the list of pages in the ``index.rst``
file. So if your file was named ``acis_panic.rst``, then you would add this to the
``index.rst`` file:

.. code-block:: rest

    * :doc:`acis_panic` (Last Updated May 12, 2017)

Make sure you add the date that it was updated, which should be the last change after
the team approves it and before the pull request is merged.

Testing Your Changes Locally
----------------------------

You can have a look at the way your pages are rendered on your local machine. The way
to do this

Deploying to the CXC Web Space
------------------------------

Once the pull request has been merged, log onto the HEAD LAN as ``acisdude``. Go to
the ``~acisdude/python/src/acis_anomaly_pages`` directory, and issue the following two
commands:

* ``git pull`` (make sure this completes without errors before running the next one!)
* ``make cxchtml``

The first command pulls the new changes into the local copy of the repository, and the
next command builds the pages and copies them to the appropriate CXC web space.