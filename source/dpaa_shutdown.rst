.. _dpaa-shutdown:

DPA-A Anomalous Shutdown
========================

What is it?
-----------

The DPA-A shuts down anomalously, presumably due to a spurious command.

When did it happen before?
--------------------------

The DPA-A has shut down 4 times over the mission:

* October 26, 2000: 2000:300:15:40, obsid 979
* December 19, 2002: 2002:353:20:26, obsid 60915
* January 12, 2015: 2015:012:00:01, obsid 52186
* December 9, 2016: 2016:344:07:40, obsid 17615

Will it happen again?
---------------------

It appears likely that the anomaly will occur again if the mission continues.

How is this Anomaly Diagnosed?
------------------------------

Within a major frame (32.2 seconds), one should see:

* 1DPPSA (DPA-A Power Supply On/Off) change from 1 to 0 (On to Off)
* 1DPP0AV0 (DPA-A +5V Analog Voltage) drop to 0.0 +/- 0.3 V
* 1DPICACU (DPA-A Input Current) drop to < 0.2 A (this value is noisy, so take an average)
* DPA-A POWER should go to zero
* 1DP28AVO (DPA-A +28V Input Voltage) is expected to have a small uptick, ~0.5 V, consistent with
  the load suddenly dropping to zero
* The software and hardware bilevels will likely not have normal values.

All other hardware telemetry should be nominal. The current values for these can be found
on our Real-Time Telemetry pages.  Older data can be examined from the dump files or the
engineering archive.

What is the first response?
---------------------------

Our real-time web pages will alert us and the Lead System Engineer will call us. We need to:

* Send an email to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz, and Bev LaMarr)
* Send an email to ``sot_yellow_alert`` explaining the status of ACIS and that the ACIS team is
  investigating.
* Call the Flight Directors, Tom Aldcroft and Scott Wolk. Leave messages if they do not answer.
* Contact the GOT Duty Officer to inform that we need the dump data as soon as possible and to
  email or call when the dump file is available.
* Process the dump data and make sure that there is nothing anomalous in the data *BEFORE*
  the shutdown. We want to know if a new occurrence looks just like the previous occurrences.
  If yes, it should appear as if in one frame the DPA-A turned off.
* Once analysis of the dump data is complete, convene a telecon at the next reasonable moment
  with the ACIS team and Peter Ford, Bob Goeke, Mark Bautz, and Bev LaMarr, and review the
  diagnosis. If the diagnosis is consistent with previous DPA-A anomalies, proceed with the
  recovery. If the diagnosis is not consistentwith previous DPA-A anomalies, stop and start a
  more involved analysis with the ACIS team.
* As soon as the time of the shutdown is known, inform ``sot_yellow_alert``. 
* Identify whether or not additional comm time is needed and if so ask the OC/LSE to request it.
* Call a telecon with the FOT, SOT, and FDs to brief them on the diagnosis and the plan to develop
  a CAP to recover.
* Prepare a CAP and submit it for review. This CAP will have the following steps:

  - Power on the DPA side A (|dpaa_on|_)
  - If *Chandra* is heading into the radiation belts, it may be necessary to also issue a ``WSVIDALLDN``
    command to power off the video boards (|wsvidalldn|_)
  - Reload the patches and restart DEA housekeeping (|stdfoptg|_)
  - Reset the focal plane temperature to -121 :math:`^\circ{\rm C}` (|fptemp_121|_)

* Execute the CAP at the next available comm. Reloading the flight software patches can take
  a half an hour, so ensure that there is enough time in the comm to execute the entire procedure.
* Write a shift report and distribute to ``sot_shift`` to inform the project that ACIS is restored
  to its default configuration.

Impacts
-------

* Until the DPA-A is powered back on, science operations will be interrupted.
* The warmboot of the BEP will reset the parameters of the TXINGS patch to their defaults. 
  They can be updated in the weekly load through a SAR.
* After recovery from a DPA-A shutdown, the power status may be in an unusual state (e.g., lower
  than expected input current) due to FEPs being off. This situation should resolve itself with 
  the next observation.

Relevant Procedures
-------------------

.. |dpaa_on| replace:: ``SOP_ACIS_DPAA_ON``
.. _dpaa_on: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAA_ON.pdf

.. |stdfoptg| replace:: ``SOP_ACIS_SW_STDFOPTG``
.. _stdfoptg: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_SW_STDFOPTG.pdf

.. |fptemp_121| replace:: ``SOT_SI_SET_ACIS_FP_TEMP_TO_M121C``
.. _fptemp_121: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.pdf

.. |wsvidalldn| replace:: ``1A_WS007_164.CLD``
.. _wsvidalldn: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/archive/cld/1A_WS007_164.CLD

.. |stdfoptgssc| replace:: ``I_ACIS_SW_STDFOPTG.ssc``
.. _stdfoptgssc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/products/ssc/I_ACIS_SW_STDFOPTG.ssc

SOT Procedures
++++++++++++++

* `Turn On DPA-A <http://cxc.cfa.harvard.edu/acis/cmd_seq/dpaa_on.pdf>`_
* `Flight Software Standard Patch F, Optional Patch G <http://cxc.cfa.harvard.edu/acis/cmd_seq/sw_stdfoptg.pdf>`_
* `Set Focal Plane Temperature to -121 C <http://cxc.cfa.harvard.edu/acis/cmd_seq/setfp_m121.pdf>`_

FOT Procedures
++++++++++++++

* |dpaa_on|_
* |stdfoptg|_
* |fptemp_121|_

FOT Scripts
+++++++++++

* |stdfoptgssc|_

CLD Scripts
+++++++++++

* |wsvidalldn|_

CAPs
++++

* `CAP 1407 (DPA-A Poweroff Recovery) <http://cxc.cfa.harvard.edu/acis/CAPs/CAP1407_dpaa_poweroff_recovery.pdf>`_
* `CAP 1342 (DPA-A Poweroff Recovery) <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1301_1400/CAP_1342_dpaa_poweroff_recovery/CAP_1342_dpaa_poweroff_recovery.pdf>`_
* `CAP 818 (DPA-A Side Recovery from Enabled/Powered Off State) <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/0801_0900/CAP_0818_DPA-A%20Power%20Off%20Recovery/CAP_818_2002_354_not_signed.pdf>`_

Relevant Notes/Memos
--------------------

* `Flight Note 394 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note394_DPA_Turn_Off_Anomaly.pdf>`_
* `Flight Note 417 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note417_DPA_Turn_Off_Anomaly.pdf>`_
* `Flight Note 563 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note563_DPA-A_Turn_Off_Anomaly_Report.pdf>`_
