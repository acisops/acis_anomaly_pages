.. _stuck_status_bit:

ACIS Science Run Termination Failure Anomaly
============================================

What is it?
-----------

SCS107 command fails to terminate a science run during bias creation.

When did it happen before?
--------------------------

Twice:

* March 3, 2016: 2016:063:17:11, obsid 17719
* September 11, 2017:256:07:53, obsid 19931

Will it happen again?
---------------------

It appears likely that the anomaly could occur again.

How is this Anomaly Diagnosed?
------------------------------

* During a safe-mode transition, the 1STAT1ST ACIS status bit fails to set as 
  expected, from Science Active (0 or RED) to Science Idle (1 or GREEN). The 
  ACIS status bits are available on both the ACIS PMON web pages and the 
  hardware web pages.

* This is indicates that the science process failed to terminate and exit when 
  SCS-107 was run.

* All other indicators (ACIS voltages, input currents, power, temperatures, and
  SIM position) should indicate that the equipment has shut down as normal for 
  an SCS-107 run, so the instrument is safe.

What is the first response?
---------------------------

The anomaly is most likely to be noted by ACIS ops during the first contact 
after a safe mode transition.  It does not produce any automated alerts.

We need to:
 
* Send an e-mail to the ACIS team (including acisdude, Peter Ford, Bob Goeke,
  Mark Bautz, and Bev LaMarr) to alert them to the existence of the anomaly.
* Send a notification to the Flight Directors including the time of the anomaly
  and the Obsid when it occurred.  Paul Viens?  telecon briefing?
* Prepare a CAP and submit it for review to capreview AT ipa DOT harvard DOT edu,
  and cc: acisdude. It will also be necessary to call the OC/CC to determine 
  which number should be used for the CAP.

  This CAP will have the following steps:

  - Confirm telemetry format is 1 or 2 and the most recent version of flight SW 
    is running
  - Warmboot the BEP and restart DEA housekeeping (|wmboot_hk|_)

  There is a template CAP in acis_docs: CAPXXXX_WMBOOT_HK

* Execute the CAP at the next available comm.
* Write a shift report and distribute to ``sot_shift`` to inform the project 
  that ACIS is restored to its default configuration.

Impacts
-------

* The anomaly occurs during safe mode transition, so no science is lost.
* The warmboot of the BEP will reset the parameters of the TXINGS patch to their
  defaults.  They can be updated in the weekly load through a SAR.
* If a science run is started before a warmboot can be performed, testing on the 
  ACIS Engineering Unit has shown that the initiation of the run will clear the 
  problem, but not without generating an error message due to "clobbering" the 
  hung process.

.. note::

    As of this writing, the latest ACIS Flight Software Patch is F, Optional 
    Patch G. An update has been developed to the ``buscrash`` patch which would
    prevent this anomaly, but has not been deemed critical enough to drive a 
    flight software update at this time.

Relevant Procedures
-------------------

.. |wmboot_hk| replace:: ``SOP_ACIS_WARMBOOT_DEAHOUSKEEPING``
.. _wmboot_hk: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.pdf

.. |wmboot_hk_pdf| replace:: PDF
.. _wmboot_hk_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.pdf

.. |wmboot_hk_doc| replace:: DOC
.. _wmboot_hk_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.doc

.. |wmboot_hk_ssc| replace:: ``I_1_WARMBOOT.ssc``
.. _wmboot_hk_ssc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/products/ssc/I_1_WARMBOOT.ssc

.. |deahk_load_cld| replace:: ``IA_WD000_137.CLD``
.. _deahk_load_cld: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/archive/cld/1A_WD000_137.CLD

.. |deahk_start_cld| replace:: ``IA_XD000_185.CLD``
.. _deahk_start_cld: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/archive/cld/1A_XD000_185.CLD

SOT Procedures
++++++++++++++

* `Warm Boot the Active ACIS BEP and Start DEA HK Run <http://cxc.cfa.harvard.edu/acis/cmd_seq/warmboot_hkp.pdf>`_

FOT Procedures
++++++++++++++

* ``SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING`` (|wmboot_hk_pdf|_) (|wmboot_hk_doc|_)

FOT Scripts
+++++++++++

* |wmboot_hk_ssc|_

CLD Scripts
+++++++++++

* |deahk_load_cld|_
* |deahk_start_cld|_

CAPs
++++

.. |cap1381_pdf| replace:: PDF
.. _cap1381_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1301_1400/CAP_1381_wmboot_deahk/CAP_1381_wmboot_deahk.pdf

.. |cap1381_doc| replace:: DOC
.. _cap1381_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1301_1400/CAP_1381_wmboot_deahk/CAP_1381_wmboot_deahk.doc

.. |cap1433_pdf| replace:: PDF
.. _cap1433_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1401-1500/CAP_1433__wmboot_deahk/CAP1433__wmboot_deahk.pdf

.. |cap1433_doc| replace:: DOC
.. _cap1433_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1401-1500/CAP_1433__wmboot_deahk/CAP1433__wmboot_deahk.doc

* CAP 1433 (ACIS BEP Warmboot and DEA housekeeping restart) (|cap1433_pdf|_) (|cap1433_doc|_)
* CAP 1381 (ACIS BEP Warmboot and DEA housekeeping restart) (|cap1381_pdf|_) (|cap1381_doc|_)

Relevant Notes/Memos
--------------------

* `ACIS-MIT Software Problem Report 151 <http://acis.mit.edu/axaf/spr/prob0151.html>`_
* `Flight Note 574 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note574_Sci_Run_Termination_Failure_Closeout.pdf>`_
