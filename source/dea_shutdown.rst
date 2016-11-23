.. _dea-shutdown:

DEA-A Anomalous Shutdown
========================

What is it?
-----------

The DEA-A shuts down anomalously, presumably due to a spurious command.

When did it happen before?
--------------------------

The DEA-A has shutdown only once in the mission, in 2005:

* September 15, 2005, 2005:258:23:31:29, obsid 6221

Will it happen again?
---------------------

It appears it could happen again, but one occurrence in 16 years provides little guidance.

How is this Anomaly Diagnosed?
------------------------------

Within a major frame (32.2 seconds), one should see:

* 1DEPSA (DEA-A Power Supply On/Off) change from 1 to 0
* All DEA-A Analog Voltages (1DEP3AVO, 1DEP2AVO, 1DEP1AVO, 1DEP0AVO, 1DEN0AVO, 1DEN1AVO) 
  go to 0.0 +/- 0.5 V 
* 1DEICACU (DEA-A Input Current) drop to < 0.2 A (this value is noisy, so take an average)
* DEA-A POWER should go to zero
* 1DE28AVO (DEA-A +28V Input Voltage) is expected to have a small uptick, ~0.5 V, consistent with
  the load suddenly dropping to zero

All other hardware telemetry should be nominal. The current values for these can be found 
on our Real-Time Telemetry pages.  Older data can be examined from the dump files or the 
engineering archive.


What is the first response?
---------------------------

Our real-time web pages will alert us and the Lead System Engineer will call us. We need to:
 
* Process the dump data and make sure that there is nothing anomalous in the data *BEFORE* 
  the shutdown. We want to know if a new occurrence looks just like the previous occurrences. 
  If yes, it should appear as if in one frame the DEA-A turned off. 
* Send an email to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz, and Bev LaMarr),
* Convene a telecon at the next reasonable moment. A DEA-A recovery requires that the BEP be 
  warmbooted after the DEA-A is back on (see `the SOP <http://cxc.cfa.harvard.edu/acis/cmd_seq/deaa_on.pdf>`_ 
  and `Flight Note 572 <http://cxc.cfa.harvard.edu/acis/memos/Flight_Note572_DEA_Shutdown_Closeout_merged.pdf>`_
  for details).

Impacts
-------

* Until the DEA is powered back on, science operations will be interrupted.
* After DEA is powered back on, the focal plane temperature will be unregulated and possibly uncalibrated. Recovery
  will require a BEP warmboot and setting the focal plane temperature to -121 :math:`^{\circ}\rm{C}`.
* The warmboot of the BEP will reset the parameters of the TXINGS patch to their defaults. They can be updated in the
  weekly load through a SAR.

Relevant Procedures
-------------------

SOT Procedures
++++++++++++++

* `Turn On DEA-A <http://cxc.cfa.harvard.edu/acis/cmd_seq/deaa_on.pdf>`_
* `Warm Boot the Active ACIS BEP and Start DEA HK Run <http://cxc.cfa.harvard.edu/acis/cmd_seq/warmboot_hkp.pdf>`_
* `Set Focal Plane Temperature to -119.7 C <http://cxc.cfa.harvard.edu/acis/cmd_seq/setfp_m121.pdf>`_

FOT Procedures
++++++++++++++

.. |deaa_on| replace:: ``SOP_61036_DEAA_ON``
.. _deaa_on: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_61036_DEAA_ON.pdf

.. |wmboot_hkp| replace:: ``SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING``
.. _wmboot_hkp: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_WARMBOOT_DEAHOUSEKEEPING.pdf

.. |fptemp_121| replace:: ``SOT_SI_SET_ACIS_FP_TEMP_TO_M121C``
.. _fptemp_121: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_SI_SET_ACIS_FP_TEMP_TO_M121C.pdf

* |deaa_on|_
* |wmboot_hkp|_
* |fptemp_121|_

Relevant Notes/Memos
--------------------

* `Flight Note 572 <http://cxc.cfa.harvard.edu/acis/memos/Flight_Note572_DEA_Shutdown_Closeout_merged.pdf>`_
* `ACIS - DEA ADC Reset (Dorothy Gordon) <http://cxc.cfa.harvard.edu/acis/memos/gordon_dea_20051118.pdf>`_