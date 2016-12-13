.. _dpa-shutdown:

DPA-A or DPA-B Anomalous Shutdown
=================================

What is it?
-----------

The DPA-A or DPA-B shuts down anomalously, presumably due to a spurious command.

When did it happen before?
--------------------------

The DPA-A has shut down 4 times over the mission:

* October 26, 2000: 2000:300:15:40, obsid 979
* December 19, 2002: 2002:353:20:26, obsid 60915
* January 12, 2015: 2015:012:00:01, obsid 52186
* December 9, 2016: 2016:344:07:40, obsid 17615

and the DPA-B has shut down once:

* December 13, 2007: 2007:347:17:50, obsid 58072

Will it happen again?
---------------------

It appears likely that the anomaly will occur again if the mission continues.

How is this Anomaly Diagnosed?
------------------------------

Within a major frame (32.2 seconds), one should see, for either DPA-A or DPA-B:

* 1DPPS[AB] (DPA-[AB] Power Supply On/Off) change from 1 to 0
* 1DPP0[AB]V0 (DPA-[AB] +5V Analog Voltage) drop to 0.0 +/- 0.3 V
* 1DPIC[AB]CU (DPA-[AB] Input Current) drop to < 0.2 A (this value is noisy, so take an average)
* DPA-[AB] POWER should go to zero
* 1DP28[AB]VO (DPA-[AB] +28V Input Voltage) is expected to have a small uptick, ~0.5 V, consistent with
  the load suddenly dropping to zero
* If DPA-A shuts down, the software and hardware bilevels will likely not have normal values.

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
* DPA-B shutdowns only require that the DPA-B be powered back on.

Impacts
-------

* Until the DPA is powered back on, science operations will be interrupted.
* In the case of a side A shutdown, the warmboot of the BEP will reset the parameters of the 
  TXINGS patch to their defaults. They can be updated in the weekly load through a SAR.
* After recovery from a DPA-A shutdown, the power status may be in an unusual state (e.g., lower
  than expected input current) due to FEPs being off. This situation should resolve itself with 
  the next observation.


Relevant Procedures
-------------------

.. |dpaa_on| replace:: ``SOP_ACIS_DPAA_ON``
.. _dpaa_on: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAA_ON.pdf

.. |dpab_on| replace:: ``SOP_ACIS_DPAB_ON``
.. _dpab_on: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DPAB_ON.pdf

.. |stdfoptg| replace:: ``SOP_ACIS_SW_STDFOPTG``
.. _stdfoptg: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_SW_STDFOPTG.pdf

.. |fptemp_121| replace:: ``SOT_SI_SET_ACIS_FP_TEMP_TO_M121C``
.. _fptemp_121: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.pdf

SOT Procedures
++++++++++++++

* `Turn On DPA-A <http://cxc.cfa.harvard.edu/acis/cmd_seq/dpaa_on.pdf>`_
* `Turn On DPA-B <http://cxc.cfa.harvard.edu/acis/cmd_seq/dpab_on.pdf>`_
* `Flight Software Standard Patch F, Optional Patch G <http://cxc.cfa.harvard.edu/acis/cmd_seq/sw_stdfoptg.pdf>`_
* `Set Focal Plane Temperature to -121 C <http://cxc.cfa.harvard.edu/acis/cmd_seq/setfp_m121.pdf>`_

FOT Procedures
++++++++++++++

* |dpaa_on|_
* |dpab_on|_
* |stdfoptg|_
* |fptemp_121|_

CAPs
++++

* `CAP 1342 (DPA-A Poweroff Recovery) <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1301_1400/CAP_1342_dpaa_poweroff_recovery/CAP_1342_dpaa_poweroff_recovery.pdf>`_
* `CAP 1407 (DPA-A Poweroff Recovery) <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1401-1500/CAP_1407_dpaa_poweroff_recovery/CAP_1407_dpaa_poweroff_recovery.pdf>`_

Relevant Notes/Memos
--------------------

* `Flight Note 394 <http://cxc.cfa.harvard.edu/acis/memos/FN394.ps>`_
* `Flight Note 417 <http://cxc.cfa.harvard.edu/acis/memos/FN417.ps>`_
