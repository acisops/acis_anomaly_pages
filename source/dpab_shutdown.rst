.. _dpab-shutdown:

DPA-B Anomalous Shutdown
========================

What is it?
-----------

The DPA-B shuts down anomalously, presumably due to a spurious command.

.. note::

    The diagnosis and response to this anomaly presented in this document assumes that the
    active BEP is powered by DPA side A.

When did it happen before?
--------------------------

The DPA-B has shut down once:  

* December 13, 2007: 2007:347:17:50, obsid 58072

Will it happen again?
---------------------

It appears likely that the anomaly will occur again if the mission continues.

How is this anomaly diagnosed?
------------------------------

Within a major frame (32.2 seconds), one should see:

* 1DPPSB (DPA-B Power Supply On/Off) change from 1 to 0 (On to Off)
* 1DPP0BVO (DPA-B +5V Analog Voltage) drop to 0.0 +/- 0.3 V
* 1DPICBCU (DPA-B Input Current) drop to < 0.2 A (this value is noisy, so take an average)
* DPA-B POWER should go to zero
* 1DP28BVO (DPA-B +28V Input Voltage) is expected to have a small uptick, ~0.5 V, consistent with
  the load suddenly dropping to zero
* If 1STAT2ST = 0, the DPA-B shutdown has caused a watchdog reboot of the BEP in use. This will
  happen if the DPA-B shuts down during an observation which is using B-side FEPs.

All other hardware telemetry should be nominal. The current values for these can be found
on our Real-Time Telemetry pages.  Older data can be examined from the dump files or the
engineering archive.

To extract information from the dump data, run ACORN on it as per the instructions in
`"Running ACORN on data dumps in the case of an anomaly (04/06/16)" <http://cxc.cfa.harvard.edu/acis/memos/Dump_Acorn.html>`_. 
Information from the tracelog files written by the ACORN tools can be plotted 
using the Python or command-line interfaces to ACISpy, see below for details.

What is the response?
---------------------

Our real-time web pages will alert us and the Lead System Engineer will call us. We need to:

* Send an email to the ACIS team at the official anomaly email address,
  ``acis-anomaly -at- googlegroups -dot- com``. If it is off-hours, 
  another ACIS Ops team member should call Peter, Bob, Kari, and Jim Francis.
* Send an email to ``sot_red_alert@cfa`` announcing that the ACIS team is aware of the DPA-B shutdown
  and is investigating, and that a telecon will be called when more information is available.
* Contact the GOT Duty Officer to inform that we need the dump data as soon as possible and to
  email or call us when the dump file is available.
* Process the dump data and make sure that there is nothing anomalous in the data *BEFORE*
  the shutdown. We want to know if a new occurrence looks just like the single previous 
  occurrence. If yes, it should appear as if in one frame the DPA-B turned off.
* Once analysis of the dump data is complete, convene a telecon at the next reasonable moment
  with the ACIS team and review the diagnosis. The MIT ACIS team (Peter Ford, Bob Goeke, Mark
  Bautz, and Bev LaMarr) should also be included in the discussion, either in the telecon or
  via email. If the diagnosis is consistent with previous DPA-B anomalies, proceed with the
  recovery. If the diagnosis is not consistent with previous DPA-B anomalies, stop and start a
  more involved analysis with the ACIS team.
* As soon as the time of the DPA-B shutdown is known, inform ``sot_yellow_alert@cfa``.
* Identify whether or not additional comm time is needed and if so ask the OC/LSE to request it.
* Send an email to ``sot_red_alert@cfa`` and call a telecon with the FOT, SOT, and FDs to brief
  them on the diagnosis and the plan to develop a CAP to recover.
* Prepare a CAP and submit it for review to capreview AT ipa DOT harvard DOT edu, and cc: acisdude.
  It will also be necessary to call the OC/CC to determine which number should be used for the CAP.
  The steps in the CAP will depend on whether or not the active BEP has executed a watchdog reboot.
  This may happen if the shutdown occurs during an observation that utilitizes the side B FEPs
  (side B powers FEPs 3-5), or if a subsequent observation requests them. Note that this implies
  that a watchdog reboot of the BEP will be avoided only if it occurs during an observation using
  only 1 or 2 CCDs, and until an observation occurs using 3 or more CCDs.

  1. If the BEP has not executed a watchdog reboot, the steps should be:

     - Turn on DPA side B (|dpab_on|_, this can be executed even if a science run is currently in
       progress, see note below).

  2. If the BEP has executed a watchdog reboot, the steps should be:

     - Stop the science run and power down the FEPs and video boards (|standby|_)
     - Turn on DPA side B (|dpab_on|_)
     - Warm-boot the BEP and start a DEA housekeeping run (|warmboot|_).

* Execute the CAP at the next available comm.
* Write a shift report and distribute to ``sot_shift`` to inform the project that ACIS is restored
  to its default configuration.

.. note::

   At this point in the mission (Jan 2017), it is standard practice to power off unused FEPs to
   reduce power consumption and keep the electronics temperatures lower. For this reason, it is
   believed that it is safe to power on the DPA side B during a science run that does not use
   these FEPs. If this practice is changed later in the mission, this procedure may have to be
   revisited.

Impacts
-------

* Until the DPA-B is powered back on, science operations which require the use of the side B FEPs
  will be affected.
* If it is necessary to warm boot the BEP, this will reset the parameters of the TXINGS patch 
  to their defaults. They can be updated in the weekly load through a SAR.

Relevant Procedures
-------------------

.. |dpab_on| replace:: ``SOP_ACIS_DPAB_ON``
.. _dpab_on: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAB_ON.pdf

.. |dpab_on_pdf| replace:: PDF
.. _dpab_on_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAB_ON.pdf

.. |dpab_on_doc| replace:: DOC
.. _dpab_on_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAB_ON.doc

.. |standby| replace:: ``SOP_61021_STANDBY``
.. _standby: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_61021_STANDBY.pdf

.. |standby_pdf| replace:: PDF
.. _standby_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_61021_STANDBY.pdf

.. |standby_doc| replace:: DOC
.. _standby_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_61021_STANDBY.doc

.. |warmboot| replace:: ``SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING``
.. _warmboot: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.pdf

.. |warmboot_pdf| replace:: PDF
.. _warmboot_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.pdf

.. |warmboot_doc| replace:: DOC
.. _warmboot_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.doc

.. |cap1055_pdf| replace:: PDF
.. _cap1055_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1001_1100/CAP_1055_Turn_on_DPA_B/CAP_1055_CMDing_Turn_On_DPA_B_warmboot_BEP_A_sign.pdf

.. |cap1055_doc| replace:: DOC
.. _cap1055_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1001_1100/CAP_1055_Turn_on_DPA_B/CAP_1055_Turn_on_DPA-B.doc

SOT Procedures
++++++++++++++

* `Turn On DPA-B <http://cxc.cfa.harvard.edu/acis/cmd_seq/dpab_on.pdf>`_
* `Put ACIS Into Thermal Standby Mode <http://cxc.cfa.harvard.edu/acis/cmd_seq/standby.pdf>`_
* `Warm Boot the Active ACIS BEP and Start DEA Housekeeping Run <http://cxc.cfa.harvard.edu/acis/cmd_seq/warmboot_hkp.pdf>`_

FOT Procedures
++++++++++++++

* ``SOP_ACIS_DPAB_ON`` (|dpab_on_pdf|_) (|dpab_on_doc|_)
* ``SOP_61021_STANDBY`` (|standby_pdf|_) (|standby_doc|_)
* ``SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING`` (|warmboot_pdf|_) (|warmboot_doc|_)

CAPs
++++

* CAP 1055 (Commanding to Turn On DPA Side B and Warm Boot BEP Side A) (|cap1055_pdf|_) (|cap1055_doc|_)

.. |mptl| replace:: ``multiplot_tracelog`` Command-line Script
.. _mptl: http://cxc.cfa.harvard.edu/acis/acispy/command_line.html#multiplot-tracelog

Relevant ACISpy Links
---------------------

* `Reading MSID Data from Tracelog File <http://cxc.cfa.harvard.edu/acis/acispy/loading_data.html#reading-msid-data-from-a-tracelog-file>`_
* `Plotting Data in Python <http://cxc.cfa.harvard.edu/acis/acispy/plotting_data.html>`_
* |mptl|_
