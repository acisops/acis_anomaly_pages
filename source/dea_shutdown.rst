.. _dea-shutdown:

DEA-A Anomalous Shutdown
========================

What is it?
-----------

The DEA-A shuts down anomalously, presumably due to a spurious command.

.. note::

   The diagnosis and response to this anomaly assumes that the active
   DEA is DEA-A.

When did it happen before?
--------------------------

The DEA-A has shutdown only once in the mission, in 2005:

* September 15, 2005, 2005:258:23:31:29, obsid 6221

Will it happen again?
---------------------

It appears it could happen again, but one occurrence provides little guidance.

How is this Anomaly Diagnosed?
------------------------------

Upon review of the telemetry data leading to the anomaly, within a major frame (32.2 seconds), one should see:

* 1DEPSA (DEA-A Power Supply On/Off) change from 1 to 0 (On to Off)
* All DEA-A Analog Voltages (1DEP3AVO, 1DEP2AVO, 1DEP1AVO, 1DEP0AVO, 1DEN0AVO, 1DEN1AVO) 
  drop to 0.0 +/- 0.5 V 
* 1DEICACU (DEA-A Input Current) drop to < 0.2 A (this value is noisy, so take an average)
* DEA-A POWER should go to zero
* 1DE28AVO (DEA-A +28V Input Voltage) is expected to have a small uptick, ~0.5 V, consistent with
  the load suddenly dropping to zero

These would be seen to remain in off/powered-down states when reviewing current values on our Real-Time Telemetry pages.
All other hardware telemetry should be nominal. Older data can be examined from the dump files or the 
engineering archive. 

To extract information from the dump data, run ACORN on it as per the instructions in
`"Running ACORN on data dumps in the case of an anomaly (04/06/16)" <http://cxc.cfa.harvard.edu/acis/memos/Dump_Acorn.html>`_. 
Information from the tracelog files written by the ACORN tools can be plotted 
using the Python or command-line interfaces to ACISpy, see below for details.
Alternatively, one may use the
`MAUDE query utility <https://occweb.cfa.harvard.edu/occweb/web/web_dev/smancini/mqb/maude_query_builder.php>`_.


What is the first response?
---------------------------

Our real-time web pages will alert us and the Lead System Engineer will call us. We need to:
 
* Send an email to the ACIS team at the official anomaly email address.  If it is off-hours, call Peter and Bob.
* Send an email to ``sot_red_alert@cfa`` announcing that the ACIS team is aware of the DEA-A shutdown
  and is investigating, and that a telecon will be called when more information is available.
* Contact the GOT Duty Officer to inform that we need the dump data as soon as possible and to
  email or call us when the dump file is available.
* Process the dump data and make sure that there is nothing anomalous in the data *BEFORE*
  the shutdown. We want to know if a new occurrence looks just like
  previous occurrences.
  If yes, it should appear as if in one frame the DEA-A turned off.
* Once analysis of the dump data is complete, convene a telecon at the next reasonable moment
  with the ACIS team and review the diagnosis. The MIT ACIS team (Peter Ford, Bob Goeke, Mark
  Bautz, and Bev LaMarr) should also be included in the discussion, either in the telecon or
  via email. If the diagnosis is consistent with previous DEA-A anomalies, proceed with the
  recovery. If the diagnosis is not consistent with previous DEA-A anomalies, stop and start a
  more involved analysis with the ACIS team.
* As soon as the time of the shutdown is known, inform ``sot_yellow_alert@cfa``.
* Identify whether or not additional comm time is needed and if so ask the OC/LSE to request it.
* Send an email to ``sot_red_alert@cfa`` and call a telecon with the FOT, SOT, and FDs to brief
  them on the diagnosis and the plan to develop CAPs to recover.
* Prepare the CAPs and submit them for review to capreview AT ipa DOT harvard DOT edu, and cc: acisdude.
  It will also be necessary to call the OC/CC to determine which number should be used for the CAPs.
  The CAP should be executed during an interval with no other ACIS commanding.  If that is not possible, the team can discuss running an SCS-107. 
  This main recovery CAP will have the following steps:

  - Power on the DEA side A (|deaa_on|_)
  - Warm boot the BEP and restart housekeeping (|wmboot_hkp|_)
  - Set the focal plane temperature to -121 :math:`^{\circ}\rm{C}` (|fptemp_121|_)

  A CAP to update *txings* values from their defaults to most-recent settings should follow the
  main recovery CAP if possible.  A template for this is in ``acis_docs/CAPs``: ``CAP1622_TXINGB_SETPARAMS``.


* Execute the CAP at the next available comm.
* Write a shift report and distribute to ``sot_shift`` to inform the project that ACIS is restored
  to its default configuration.

    
Impacts
-------

* Until the DEA is powered back on, science operations will be interrupted.
* After DEA is powered back on, the focal plane temperature will be unregulated and possibly uncalibrated. Recovery
  requires a BEP warmboot and setting the focal plane temperature to -121 :math:`^{\circ}\rm{C}`.
* The warmboot of the BEP will reset the parameters of the TXINGS patch to their defaults.
  If not updated during initial recovery as above, *txings* settings should be updated as soon as possible via CAP (see CAP 1622) or SAR to prevent undesired radiation shutdown.


Relevant Procedures
-------------------

.. |deaa_on| replace:: ``SOP_ACIS_DEAA_ON``
.. _deaa_on: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DEAA_ON.pdf

.. |deaa_on_pdf| replace:: PDF
.. _deaa_on_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DEAA_ON.pdf

.. |deaa_on_doc| replace:: DOC
.. _deaa_on_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DEAA_ON.docx

.. |wmboot_hkp| replace:: ``SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING``
.. _wmboot_hkp: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.pdf 

.. |wmboot_hkp_pdf| replace:: PDF
.. _wmboot_hkp_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.pdf 

.. |wmboot_hkp_doc| replace:: DOC
.. _wmboot_hkp_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.doc
		 
.. |fptemp_121| replace:: ``SOT_SI_SET_ACIS_FP_TEMP_TO_M121C``
.. _fptemp_121: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.pdf

.. |fptemp_121_pdf| replace:: PDF
.. _fptemp_121_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.pdf

.. |fptemp_121_doc| replace:: DOC
.. _fptemp_121_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.doc

.. |deaonssc| replace:: ``I_1_DEAA_ON.ssc``
.. _deaonssc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/products/ssc/I_1_DEAA_ON.ssc  
 
.. |warmbootssc| replace:: ``I_1_WARMBOOT.ssc``
.. _warmbootssc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/products/ssc/I_1_WARMBOOT.ssc  
 
SOT Procedures
++++++++++++++

* `Turn On DEA-A <http://cxc.cfa.harvard.edu/acis/cmd_seq/deaa_on.pdf>`_
* `Warm Boot the Active ACIS BEP and Start DEA HK Run <http://cxc.cfa.harvard.edu/acis/cmd_seq/warmboot_hkp.pdf>`_
* `Set Focal Plane Temperature to -121 C <http://cxc.cfa.harvard.edu/acis/cmd_seq/setfp_m121.pdf>`_

FOT Procedures
++++++++++++++

* ``SOP_ACIS_DEAA_ON`` (|deaa_on_pdf|_) (|deaa_on_doc|_)
* ``SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING`` (|wmboot_hkp_pdf|_) (|wmboot_hkp_doc|_)
* ``SOT_SI_SET_ACIS_FP_TEMP_TO_M121C`` (|fptemp_121_pdf|_) (|fptemp_121_doc|_)

FOT Scripts
+++++++++++

* |deaonssc|_
* |warmbootssc|_

CAPs
+++++++++++

.. note::

   The response to the first occurence of the DEA-A anomaly did not
   include all the steps above and included additional testing of S0 and
   commanding of a CTI run, neither of which is necessary for an *identical* repeat occurrence.
   Response to future DEA anomalies should follow the above
   steps.

.. |cap980_pdf| replace:: PDF
.. _cap980_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/0901_1000/CAP_0980_DEA_A_Recovery/CAP_980_2005_259_sign.pdf

.. |cap981_pdf| replace:: PDF
.. _cap981_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/0901_1000/CAP_0981_ACIS_DEA_Warm_Boot/CAP_981_2005_289_sign.pdf

* CAP 980 (DEA-A Recovery) (|cap980_pdf|_)
* CAP 981 (ACIS DEA Warm Boot) (|cap981_pdf|_)
* CAP 1622 (Update TXINGS Parameter Values) (``acis_docs/CAPs/CAP1622_TXINGB_SETPARAMS.pdf``) (``acis_docs/CAPs/CAP1622_TXINGB_SETPARAMS.docx``)
  
Relevant Notes/Memos
--------------------

* `Flight Note 572
  <http://cxc.cfa.harvard.edu/acis/memos/Flight_Note572_DEA_Shutdown_Closeout_merged.pdf>`_
  (includes SOT memo "ACIS DEA-A Off anomaly" by Edgar & Germain)
* `ACIS - DEA ADC Reset (Dorothy Gordon) <http://cxc.cfa.harvard.edu/acis/memos/gordon_dea_20051118.pdf>`_

.. |mptl| replace:: ``multiplot_tracelog`` Command-line Script
.. _mptl: https://cxc.cfa.harvard.edu/acis/acispy_cmd/#multiplot-tracelog

Relevant ACISpy Links
---------------------

* `Reading MSID Data from Tracelog File <https://cxc.cfa.harvard.edu/acis/acispy/loading_data.html#reading-msid-data-from-a-tracelog-file>`_
* `Plotting Data in Python <https://cxc.cfa.harvard.edu/acis/acispy/Plotting_Data.html>`_
* |mptl|_
