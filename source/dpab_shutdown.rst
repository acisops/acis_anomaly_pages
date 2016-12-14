.. _dpab-shutdown:

DPA-B Anomalous Shutdown
========================

What is it?
-----------

The DPA-B shuts down anomalously, presumably due to a spurious command.

When did it happen before?
--------------------------

The DPA-B has shut down once:  

* December 13, 2007: 2007:347:17:50, obsid 58072

Will it happen again?
---------------------

It appears likely that the anomaly will occur again if the mission continues.

How is this Anomaly Diagnosed?
------------------------------

Within a major frame (32.2 seconds), one should see:

* 1DPPSB (DPA-B Power Supply On/Off) change from 1 to 0
* 1DPP0BV0 (DPA-B +5V Analog Voltage) drop to 0.0 +/- 0.3 V
* 1DPICBCU (DPA-B Input Current) drop to < 0.2 A (this value is noisy, so take an average)
* DPA-B POWER should go to zero
* 1DP28BVO (DPA-B +28V Input Voltage) is expected to have a small uptick, ~0.5 V, consistent with
  the load suddenly dropping to zero

All other hardware telemetry should be nominal. The current values for these can be found
on our Real-Time Telemetry pages.  Older data can be examined from the dump files or the
engineering archive.

What is the first response?
---------------------------

Our real-time web pages will alert us and the Lead System Engineer will call us. We need to:

* Send an email to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz, and Bev LaMarr)
* Send an email to ``sot_yellow_alert`` explaining the status of ACIS and the planned response.
* Call the Flight Directors:   

  - Call the lead Flight Director, Tom Aldcroft. Leave a message if he does not answer.
  - If Tom does not answer, call Scott Wolk. Leave a message if he does not answer.

* Process the dump data and make sure that there is nothing anomalous in the data *BEFORE*
  the shutdown. We want to know if a new occurrence looks just like the previous occurrences.
  If yes, it should appear as if in one frame the DPA-A (or DPA-B) turned off.
* Convene a telecon at the next reasonable moment.
* As soon as the time of the DPA-[AB] shutdown is known, inform ``sot_yellow_alert``. 
* Identify whether or not additional comm time is needed and if so ask the Lead Systems 
  Engineer to request it.
* DPA-A shutdowns require reloading the patches, restarting DEA housekeeping, and resetting 
  the focal plane temperature. 
* DPA-B shutdowns require that the DPA-B be powered back on, science operations may need
  to be stopped, and the FEPs and video boards maybe need to be powered down.

Impacts
-------

* Until the DPA-A is powered back on, science operations will be interrupted, but if DPA-B 
  needs to be powered back on science operations may need to be temporarily stopped.
* In the case of a side A or B shutdown, the warmboot of the BEP will reset the parameters of the 
  TXINGS patch to their defaults. They can be updated in the weekly load through a SAR.
* After recovery from a DPA-A shutdown, the power status may be in an unusual state (e.g., lower
  than expected input current) due to FEPs being off. This situation should resolve itself with 
  the next observation.


Relevant Procedures
-------------------

.. |dpab_on| replace:: ``SOP_ACIS_DPAB_ON``
.. _dpab_on: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAB_ON.pdf

.. |stdfoptg| replace:: ``SOP_ACIS_SW_STDFOPTG``
.. _stdfoptg: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_SW_STDFOPTG.pdf

.. |fptemp_121| replace:: ``SOT_SI_SET_ACIS_FP_TEMP_TO_M121C``
.. _fptemp_121: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.pdf

SOT Procedures
++++++++++++++

* `Turn On DPA-B <http://cxc.cfa.harvard.edu/acis/cmd_seq/dpab_on.pdf>`_
* `Flight Software Standard Patch F, Optional Patch G <http://cxc.cfa.harvard.edu/acis/cmd_seq/sw_stdfoptg.pdf>`_
* `Set Focal Plane Temperature to -121 C <http://cxc.cfa.harvard.edu/acis/cmd_seq/setfp_m121.pdf>`_

FOT Procedures
++++++++++++++

* |dpab_on|_
* |stdfoptg|_
* |fptemp_121|_

CAPs
++++

* `CAP 1055 (Commanding to Turn On DPA Side B and Warm Boot BEP Side A) <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1001_1100/CAP_1055_Turn_on_DPA_B/CAP_1055_CMDing_Turn_On_DPA_B_warmboot_BEP_A_sign.pdf>`_

Relevant Notes/Memos
--------------------

* `Flight Note 394 <http://cxc.cfa.harvard.edu/acis/memos/FN394.ps>`_
* `Flight Note 417 <http://cxc.cfa.harvard.edu/acis/memos/FN417.ps>`_
