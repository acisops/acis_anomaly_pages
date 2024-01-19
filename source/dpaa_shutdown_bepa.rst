.. _dpaa-shutdown-bepa:

DPA-A Anomalous Shutdown (BEP-A Active)
=======================================

What is it?
-----------

The DPA-A shuts down anomalously, presumably due to a spurious command.

.. note::

    **IMPORTANT:** The diagnosis and response to the anomaly presented in this document assumes that 
    BEP A, powered by DPA side A, is the active BEP
    
    *This anomaly requires human intervention to recover to science operations.*

When did it happen before?
--------------------------

The DPA-A has shut down 4 times over the mission:

* October 26, 2000: 2000:300:15:40, obsid 979
* December 19, 2002: 2002:353:20:26, obsid 60915
* January 12, 2015: 2015:012:00:01, obsid 52186
* December 9, 2016: 2016:344:07:40, obsid 17615

Will it happen again?
---------------------

It appears likely that the anomaly will occur again.

How is this anomaly diagnosed?
------------------------------

Within a major frame (32.2 seconds), one should see:

* 1DPPSA (DPA-A Power Supply On/Off) change from 1 to 0 (On to Off)
* 1DPP0AVO (DPA-A +5V Analog Voltage) drop to 0.0 +/- 0.3 V
* 1DPICACU (DPA-A Input Current) drop to < 0.2 A 
* DPA-A POWER should go to ~zero
* 1DP28AVO (DPA-A +28V Input Voltage) is expected to have a small uptick, ~0.5 V, consistent with
  the load suddenly dropping to zero.  May require more than 1 major frame.
* The software and hardware bilevels will likely not have normal values if BEP side A is active.

All other hardware telemetry should be nominal. The current values for these can be found
on our Real-Time Telemetry pages. Older data can be examined from the dump files or the
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
* Send an email to ``sot_red_alert@cfa`` announcing that the ACIS team is aware of the DPA-A shutdown
  and is investigating, and that a telecon will be called when more information is available.
* Contact the GOT Duty Officer to inform that we need the dump data as soon as possible and to
  email or call us when the dump file is available.
* Process the dump data and make sure that there is nothing anomalous in the data *BEFORE*
  the shutdown. We want to know if a new occurrence looks just like the previous occurrences.
  If yes, it should appear as if in one frame the DPA-A turned off.
* Once analysis of the dump data is complete, convene a telecon at the next reasonable moment
  with the ACIS team and review the diagnosis. The MIT ACIS team (Peter Ford, Bob Goeke, Mark
  Bautz, and Bev LaMarr) should also be included in the discussion, either in the telecon or
  via email. If the diagnosis is consistent with previous DPA-A anomalies, proceed with the
  recovery. If the diagnosis is not consistent with previous DPA-A anomalies, stop and start a
  more involved analysis with the ACIS team.
* As soon as the time of the shutdown is known, inform ``sot_yellow_alert@cfa``.
* Identify whether or not additional comm time is needed and if so ask the OC/LSE to request it.
* Send an email to ``sot_red_alert@cfa`` and call a telecon with the FOT, SOT, and FDs to brief
  them on the diagnosis and the plan to develop CAPs to recover.
* Prepare the CAPs and submit them for review to capreview AT ipa DOT harvard DOT edu, and cc: acisdude.
  It will also be necessary to call the OC/CC to determine which number should be used for the CAPs.
  The main recovery CAP will have the following steps:

  - Power on the DPA side A (|dpaa_on|_)
 
  - Power down the video boards and leave 3 FEPs on by issuing a WSPOW0002A command.
    
  - Reload the patches and restart DEA housekeeping (|stdhoptj|_)
  - Reset the focal plane temperature setpoint to -121 :math:`^\circ{\rm C}` (|fptemp_121|_)

  A CAP to update *txings* values from their defaults to most-recent settings should follow the
  main recovery CAP if possible.  

* Execute the CAPs at the next available comm. Reloading the flight software patches can take
  a half an hour, so ensure that there is enough time in the comm to execute the entire procedure.
* Write a shift report and distribute to ``sot_shift`` to inform the project that ACIS is restored
  to its default configuration.

* Upon warmboot it is essential to update the *txings* parameters to the most recent values as quickly as possible in order
  to prevent undesired radiation triggers.   CAP 1708  was used at the latest ACIS FSW patch: HJK Version 60.  You can
  use that CAP  as a  **template** for this.   But you must use the latest txing parameter set (which may not be the
  values used in 1708). You can find CAP1708_TXINGB_SETPARAMS.docx  in ``acis_docs/CAPs``: ``CAP1708_TXINGB_SETPARAMS``.
  Refer to the most recent txings SAR for the current optimal values for the txings parameters.

.. note::

   As of this writing, the ACIS Flight Software Patch level is standard H, optional J, version = 60. 
   Before preparing the CAPs, check that this is the correct version.

Impacts
-------

* Until the DPA-A is powered back on, science operations will be interrupted.
  
* The warmboot of the BEP will reset the parameters of the TXINGS patch to their defaults. 
  If not updated during initial recovery as above, *txings* settings should be updated as soon as possible via CAP (see CAP 1708) or SAR to prevent undesired radiation shutdowns.


Relevant Procedures
-------------------

.. |dpaa_on| replace:: ``SOP_ACIS_DPAA_ON``
.. _dpaa_on: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAA_ON.pdf

.. |dpaa_on_pdf| replace:: PDF
.. _dpaa_on_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAA_ON.pdf

.. |dpaa_on_doc| replace:: DOC
.. _dpaa_on_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAA_ON.doc

.. |stdhoptj| replace:: ``SOP_ACIS_SW_STDHOPTJ``
.. _stdhoptj: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_SW_STDHOPTJ.pdf

.. |stdhoptj_pdf| replace:: PDF
.. _stdhoptj_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_SW_STDHOPTJ.pdf

.. |stdhoptj_doc| replace:: DOC
.. _stdhoptj_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_SW_STDHOPTJ.doc



.. |fptemp_121| replace:: ``SOT_SI_SET_ACIS_FP_TEMP_TO_M121C``
.. _fptemp_121: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.pdf

.. |fptemp_121_pdf| replace:: PDF
.. _fptemp_121_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.pdf

.. |fptemp_121_doc| replace:: DOC
.. _fptemp_121_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.pdf

.. |stdhoptjssc| replace:: ``I_ACIS_SW_STDHOPTJ.ssc``
.. _stdhoptjssc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/products/ssc/I_ACIS_SW_STDHOPTJ.ssc



SOT Procedures
++++++++++++++

* `Turn On DPA-A <http://cxc.cfa.harvard.edu/acis/cmd_seq/dpaa_on.pdf>`_
* `Flight Software Standard Patch H, Optional Patch J <http://cxc.cfa.harvard.edu/acis/cmd_seq/sw_stdhoptj.pdf>`_
* `Set Focal Plane Temperature to -121 C <http://cxc.cfa.harvard.edu/acis/cmd_seq/setfp_m121.pdf>`_

FOT Procedures
++++++++++++++

* ``SOP_ACIS_DPAA_ON`` (|dpaa_on_pdf|_) (|dpaa_on_doc|_)
* ``SOP_ACIS_SW_STDHOPTJ`` (|stdhoptj_pdf|_) (|stdhoptj_doc|_)
* ``SOT_SI_SET_ACIS_FP_TEMP_TO_M121C`` (|fptemp_121_pdf|_) (|fptemp_121_doc|_)

FOT Scripts
+++++++++++

* |stdhoptjssc|_

CLD Files
+++++++++++

* 1AWSPOW0002A_206.cld
    - Located at: /data/acis/acis_docs/command_load/1AWSPOW0002A_206.cld and 1AWSPOW0002A_206.txt



  
CAPs
++++

.. |cap818_pdf| replace:: PDF
.. _cap818_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/0801_0900/CAP_0818_DPA-A%20Power%20Off%20Recovery/CAP_818_2002_354_not_signed.pdf

.. |cap1342_pdf| replace:: PDF
.. _cap1342_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1301_1400/CAP_1342_dpaa_poweroff_recovery/CAP_1342_dpaa_poweroff_recovery.pdf

.. |cap1342_doc| replace:: DOC
.. _cap1342_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1301_1400/CAP_1342_dpaa_poweroff_recovery/CAP_1342_dpaa_poweroff_recovery.doc

.. |cap1407_pdf| replace:: PDF
.. _cap1407_pdf: http://cxc.cfa.harvard.edu/acis/CAPs/CAP1407_dpaa_poweroff_recovery.pdf

.. |cap1407_doc| replace:: DOC
.. _cap1407_doc: http://cxc.cfa.harvard.edu/acis/CAPs/CAP1407_dpaa_poweroff_recovery.doc

.. |cap1708_pdf| replace:: PDF
.. _cap1708_pdf: http://cxc.cfa.harvard.edu/acis/CAPs/CAP1708_TXINGB_SETPARAMS.pdf

.. |cap1708_doc| replace:: DOC
.. _cap1708_doc: http://cxc.cfa.harvard.edu/acis/CAPs/CAP1708_TXINGB_SETPARAMS.docx

* CAP 1708 (Update TXINGS Parameter Values) (|cap1708_pdf|_) (|cap1708_doc|_)
* CAP 1407 (DPA-A Poweroff Recovery) (|cap1407_pdf|_) (|cap1407_doc|_)
* CAP 1342 (DPA-A Poweroff Recovery) (|cap1342_pdf|_) (|cap1342_doc|_)
* CAP 818 (DPA-A Side Recovery from Enabled/Powered Off State) (|cap818_pdf|_)

Relevant Notes/Memos
--------------------

* `Flight Note 394 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note394_DPA_Turn_Off_Anomaly.pdf>`_
* `Flight Note 417 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note417_DPA_Turn_Off_Anomaly.pdf>`_
* `Flight Note 563 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note563_DPA-A_Turn_Off_Anomaly_Report.pdf>`_

.. |mptl| replace:: ``multiplot_tracelog`` Command-line Script
.. _mptl: http://cxc.cfa.harvard.edu/acis/acispy_cmd/#multiplot-tracelog

Relevant ACISpy Links
---------------------

* `Reading MSID Data from Tracelog File <http://cxc.cfa.harvard.edu/acis/acispy/loading_data.html#reading-msid-data-from-a-tracelog-file>`_
* `Plotting Data in Python <http://cxc.cfa.harvard.edu/acis/acispy/Plotting_Data.html>`_
* |mptl|_
