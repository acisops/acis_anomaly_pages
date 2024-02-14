.. _dpab-shutdown-bepa:

DPA-B Anomalous Shutdown (BEP-A Active)
=======================================

What is it?
-----------

The DPA-B shuts down anomalously, presumably due to a spurious command.

.. note::

    **IMPORTANT:** The diagnosis and response to the anomaly presented in this document assumes that
    BEP A, powered by DPA side A, is the active BEP.

    *This anomaly requires human intervention to recover to science operations.*


When did it happen before?
--------------------------

The DPA-B has shut down once:  

* December 13, 2007: 2007:347:17:50, obsid 58072

Will it happen again?
---------------------

It appears likely that the anomaly will occur again.

How is this anomaly diagnosed?
------------------------------

Within a major frame (32.2 seconds), one should see:

* 1DPPSB (DPA-B Power Supply On/Off) change from 1 to 0 (On to Off)
* 1DPP0BVO (DPA-B +5V Analog Voltage) drop to 0.0 +/- 0.3 V
* 1DPICBCU (DPA-B Input Current) drop to < 0.2 A
* DPA-B POWER should go to zero
* 1DP28BVO (DPA-B +28V Input Voltage) is expected to have a small uptick, ~0.5 V, consistent with
  the load suddenly dropping to zero. May require more than 1 major frame.
* If 1STAT2ST = 0, the DPA-B shutdown has caused a watchdog reboot of the BEP in use. This will
  happen if the DPA-B shuts down during an observation which is using
  B-side FEPs. If a watchdog reboot occurs, the software will revert to Version 11.

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
  another ACIS Ops team member should call Peter, Bob, and Jim Francis.

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
  them on the diagnosis and the plan to develop CAPs to recover.


* Prepare the CAPs and submit them for review to capreview AT ipa DOT harvard DOT edu, and cc: acisdude.
  It will also be necessary to call the OC/CC to determine which number should be used for the CAPs.

  The steps in the main recovery CAP will depend on whether or not the active BEP has executed a watchdog reboot.
  This may happen if the shutdown occurs during an observation that utilizes the side B FEPs
  (DPA side B powers FEPs 3-5), or if a subsequent observation requests them.  Observations using three or more
  CCDs require DPA Side-B FEPs and will result in a bus error/watchdog reset (Side-A's FEP 0 is reserved for six
  CCD observations). Observations using only one or two CCDs need only DPA Side-A FEPs and won't be watchdog
  reset upon loss of the Side-B power. (As of this writing there are two exceptions, the Next-In-Line and the 2-CCD
  ECS SI modes which use FEPs 1 & 4 and would cause a watchdog reboot.)
  

     If BEP A - the active BEP - has not executed a watchdog reboot, the steps should be:

     - Turn on DPA side B (|dpab_on|_, this can be executed even if a science run is currently in
       progress, see note below).


     Otherwise if, in addition to this anomaly,  the BEP A has executed a watchdog reboot, the steps should be:

     - Stop the science run and power down the FEPs and video boards (|standby|_)
     - Turn on DPA side B (|dpab_on|_)
     - Warm-boot the BEP and start a DEA housekeeping run (|warmboot|_).
     - Set 3 FEPs on by issuing a WSPOW0002A command (1AWSPOW0002A_206.CLD).


The current standard, safe configuration for ACIS is 3 FEPs (1, 3, & 5) powered on and not clocking.
The last step in the recovery in the case of a watchdog reboot listed above issues a WSPOW0002A
command to put ACIS in the 3 FEPs powered on and not clocking configuration.

  A CAP to update *txings* values from their defaults to the most-recent settings should follow the
  main recovery CAP if possible.   CAP 1708  was used at the latest ACIS FSW patch: HJK Version 60.  You can
  use that CAP  as a  **template** for this.   But you must use the latest txing parameter set (which may not be the
  values used in 1708). You can find CAP1708_TXINGB_SETPARAMS.docx  in ``acis_docs/CAPs``: ``CAP1708_TXINGB_SETPARAMS``.
  Refer to the most recent txings SAR for the current optimal values for the txings parameters.

 
* Execute the recovery CAPs at the next available comm. 
  
* Write a shift report and distribute to ``sot_shift`` to inform the project that ACIS is restored
  to its default configuration.

.. note::

   At this point in the mission (Jan 2017), during observations, it is standard practice to power off unused FEPs to
   reduce power consumption and keep the electronics temperatures lower. For this reason, it is
   believed that it is safe to power on the DPA side B during a science run that does not use
   these FEPs. If this practice is changed later in the mission, this procedure may have to be
   revisited.


Impacts
-------

* Until the DPA-B is powered back on, science operations which require the use of the side B FEPs
  will be affected.
* If it is necessary to warm boot the BEP, this will reset the parameters of the TXINGS patch 
  to their defaults. 
  If not updated during initial recovery as above, *txings* settings should be updated as soon as possible via CAP (see CAP 1708) or SAR to prevent undesired radiation shutdown.

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

.. |cap1708_pdf| replace:: PDF
.. _cap1708_pdf: http://cxc.cfa.harvard.edu/acis/CAPs/CAP1708_TXINGB_SETPARAMS.pdf

.. |cap1708_doc| replace:: DOC
.. _cap1708_doc: http://cxc.cfa.harvard.edu/acis/CAPs/CAP1708_TXINGB_SETPARAMS.docx


SOT Procedures
++++++++++++++

* `Turn On DPA-B <http://cxc.cfa.harvard.edu/acis/cmd_seq/dpab_on.pdf>`_
* `Put ACIS Into Thermal Standby Mode <http://cxc.cfa.harvard.edu/acis/cmd_seq/standby.pdf>`_
* `Warm Boot the Active ACIS BEP and Start DEA Housekeeping Run
  <http://cxc.cfa.harvard.edu/acis/cmd_seq/warmboot_hkp.pdf>`_

  
FOT Procedures
++++++++++++++

* ``SOP_ACIS_DPAB_ON`` (|dpab_on_pdf|_) (|dpab_on_doc|_)
* ``SOP_61021_STANDBY`` (|standby_pdf|_) (|standby_doc|_)
* ``SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING`` (|warmboot_pdf|_) (|warmboot_doc|_)

CLD Scripts
+++++++++++


   1AWSPOW00002A_206.cld (OBC-A side)
    - Located at: /data/acis/acis_docs/command_load/1AWSPOW0002A_206.cld and 1AWSPOW0002A_206.txt

CAPs
++++

* CAP 1708 (Update TXINGS Parameter Values) (|cap1708_pdf|_) (|cap1708_doc|_)
* CAP 1055 (Commanding to Turn On DPA Side B and Warm Boot BEP Side A) (|cap1055_pdf|_) (|cap1055_doc|_)
  
.. |mptl| replace:: ``multiplot_tracelog`` Command-line Script
.. _mptl: http://cxc.cfa.harvard.edu/acis/acispy_cmd/#multiplot-archive


Relevant Notes/Memos
+++++++++++++++++++

* `Flight Note 417 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note417_DPA_Turn_Off_Anomaly.pdf>`_

This flight note covered the December 2002 DPA-A Shutdown and was used to close this issue out as well.


Relevant ACISpy Links
---------------------

* `Reading MSID Data from Tracelog File <http://cxc.cfa.harvard.edu/acis/acispy/loading_data.html#reading-msid-data-from-a-tracelog-file>`_
* `Plotting Data in Python <http://cxc.cfa.harvard.edu/acis/acispy/Plotting_Data.html>`_
* |mptl|_
