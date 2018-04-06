.. _dpaa-shutdown:

ACIS Door Mechanism side B Anomalous Enable
===========================================

What is it?
-----------

The ACIS Door Mechanism side B was enabled anomalously, presumably due to a spurious command.

When did it happen before?
--------------------------

The ACIS Door Mechanism side B has been enabled anomalously only once during the mission:

* March 20, 2018: 2018:079:23:42:10, obsid 20135

This page also lists symptoms and responses for a side A anomaly, which has not happened in flight.

Will it happen again?
---------------------

It appears likely that this anomaly, or similar ones, will occur again if the mission continues.

How is this anomaly diagnosed?
------------------------------

Within a major frame (32.2 seconds), one should see:

* 1MDBUBON (Door Enable side B) change from 0 to 1 (Disabled to Enabled). Side A: 1MDBUAON.
* 1MCDRBCL (Door close drive side B) is OFF. Side A: 1MCDRACL.
* Temperatures 1MAHOBT and 1MAHCBT will show credible values (-2 C in this case), due
  to the thermistors being powered up when the mechanism is enabled. Side A: 1MAHOAT and 1MAHCAT.
* 1DACTAT (the Door Angle Potentiometer) should be unchanged at 70 +/- 5 degrees,
  indicating the door has not moved.
* ACIS should still be recieving photons if a science run is in progress.
* 1MECLBCL (ACIS door closed indicator) will read NCLOS (not closed); normally this
  is unpowered and reads CLOS (closed). Note this item does not appear on the ACIS
  Real Time telemetry pages. The OC has access to this item on GRETA pages. Side A: 1MECLACL.

All other hardware telemetry should be nominal. The current values for these (except 1MECLBCL)
can be found on our Real-Time Telemetry pages.  Older data can be examined from the dump files
or the engineering archive.

What is the response?
---------------------

Our real-time web pages will alert us and the Lead System Engineer will call us. We need to:

* Send an email to the ACIS team at the official anomaly email address. If it is off-hours, call
  Peter and Bob.
* Send an email to ``sot_red_alert@head`` announcing that the ACIS team is aware of the enabling
  of the door mechanism, and calling a telecon to discuss disabling it. Note that other
  mechanisms (valves) can be sorted out later and are not in any sense urgent.
* Ask the Lead Systems Engineer and the Flight Director to send the command 1MCMDBDS to
  disable the door mechanism, side B. They may wish to test the command link by first
  sending a no-op command to the SI RCTU (CNOOPSI command). Use ``CAP 1438`` as a guide.
  The side A command would be 1MCMDADS.
* Contact the GOT Duty Officer to inform that we need the dump data as soon as possible and to
  email or call us when the dump file is available.
* Process the dump data and make sure that there is nothing anomalous in the data *BEFORE*
  the shutdown. We want to know if a new occurrence looks just like the previous occurrences.
  If yes, it should appear as if in one frame the door mechanism was enabled.
* Once analysis of the dump data is complete, convene a telecon at the next reasonable moment
  with the ACIS team and review the diagnosis. The MIT ACIS team (Peter Ford, Bob Goeke, Mark
  Bautz, and Bev LaMarr) should also be included in the discussion, either in the telecon or
  via email. If the diagnosis is consistent with previous anomalies, (TBR... proceed with the
  recovery). If the diagnosis is not consistent with previous spurious command anomalies,
  stop and start a more involved analysis with the ACIS team.

...possibly useful things left over from the DEA-A poweroff page I cribbed from... TBR

* Identify whether or not additional comm time is needed and if so ask the OC/LSE to request it.
* Send an email to ``sot_red_alert@head`` and call a telecon with the FOT, SOT, and FDs to brief
  them on the diagnosis and the plan to develop a CAP to recover.
* Prepare a CAP and submit it for review to capreview AT ipa DOT harvard DOT edu, and cc: acisdude.
  It will also be necessary to call the OC/CC to determine which number should be used for the CAP.
  This CAP will have the following steps:

  - Send a NOP command, CNOOPSI, and monitor command count increment.
  - Send command 1MCMDBDS to disable side B of the door mechanism. (Side A: 1MCMDADS).
  - Verify telemetry has returned to normal.

* Execute the CAP at the next available comm. Reloading the flight software patches can take
  a half an hour, so ensure that there is enough time in the comm to execute the entire procedure.
* Write a shift report and distribute to ``sot_shift`` to inform the project that ACIS is restored
  to its default configuration.


Impacts
-------

* While the door mechanism is enabled, any spurious command to move the door will result
in undesired hardware action.

Relevant Procedures
-------------------


SOT Procedures
++++++++++++++

* `Open ACIS Camera Housing Door <http://cxc.cfa.harvard.edu/acis/cmd_seq/open_door.ps>`_ and
* `Close ACIS Camera Housing Door <http://cxc.cfa.harvard.edu/acis/cmd_seq/close_door.ps>`_ contain useful discussion of the door mechanism.

FOT Procedures
++++++++++++++

* SAP 61032 (Open ACIS door) and
* SAP 51010 (Close ACIS door) contain useful details on the door mechanism.

FOT Scripts
+++++++++++

* 

CLD Scripts
+++++++++++

* 

ACIS Commands
+++++++++++++

* ``1MCMDBDS`` Disable ACIS Door side B
* ``1MCMDADS`` Disable ACIS Door side A

CAPs
++++

.. |cap1438_pdf| replace:: PDF
.. _cap1438_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1301_1400/CAP_1438_dpaa_poweroff_recovery/CAP_1438_dpaa_poweroff_recovery.pdf

.. |cap1438_doc| replace:: DOC
.. _cap1438_doc: https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1301_1400/CAP_1438_dpaa_poweroff_recovery/CAP_1438_dpaa_poweroff_recovery.doc

.. |temp_cap1438_pdf| replace:: TEMP PDF
.. _temp_cap1438_pdf: https://occweb.cfa.harvard.edu/occweb/FOT/operations/caps_in_process/CAP_1438_ACIS_Mechanism_Disable.pdf

.. |temp_cap1438_doc| replace:: TEMP DOC
.. _temp_cap1438_doc: https://occweb.cfa.harvard.edu/occweb/FOT/operations/caps_in_process/CAP_1438_ACIS_Mechanism_Disable.doc

* CAP 1438 (ACIS Mechanism Disable) (|temp_cap1438_pdf|_) (|temp_cap1438_doc|_) (link TBR: these are in the caps in process area)

Relevant Notes/Memos
--------------------

* `Flight Note 394 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note394_DPA_Turn_Off_Anomaly.pdf>`_ for SEU-induced spurious PSMC commands.
* Flight note written about this specific incident TBD

A note on other similar potential anomalies
-------------------------------------------

Note that the hardware for communicating pulse commands to the PSMC is the same for
a large number of systems, and presumably all of them are subject to SEUs which could
be interpreted by the hardware as spurious commanding. In nearly all cases, this
situation is benign. For example, commanding the PSMC to the existing state is a NOP.
Commanding something to turn on which is disabled is likewise a NOP. Disabling
a system that's active is a NOP (TBR... is this true?).

There are, however, a few cases to note. Enabling a system that is normally off and
disabled leaves us one spurious command away from activating a system inadertantly. 

In the cases of the door mechanisms (side A or B) or the DEA side B, we should take
immediate action to send a disable command, as activating the corresponding power
supply would have negative consequences. The command to disable DEA side B is 1DEPSBDS.
The ACIS hardware commands are documented `here <http://acis.mit.edu/acis/ipcl/ipcl-cmds-08jun98.html>`_
(among other places).
In the cases of the vent valve mechanisms, either the small or large vent valve,
either side A or side B, this can be done at our leisure, since even if these
valves were to close at this point in the mission, there would be no immediate
consequences.

See other anomaly pages for responses to spurious box turn-off commands (DPA-A, DPA-B, DEA-A).