.. _radmon_flag:

Anomalous Radiation Monitor Flag Assertion
===========================================

(This anomaly page is a work in progress.)

What is it?
-----------

The ACIS Radiation Monitor Flag was asserted anomalously, presumably due to a spurious command.  This suspends any active science runs and powers off the video boards.

When did it happen before?
--------------------------

This anomaly has not previously occurred.

Will it happen again?
---------------------

There have been no spurious hardware serial digital commands in ACIS to date.  (Peter should verify this statement!)

How is this anomaly diagnosed?
------------------------------

If the radmon flag is asserted during real-time contact with ACIS,

* In software housekeeping,

  * SWSTAT_SCI_INHIBIT_ON = 74
  * SWSTAT_DEACCD_POWEROFF = 81

* If a science run was in progress, terminationCode = SMTERM_RADMON in the scienceReport

While science is inhibited:

* Bilevels are (science idle or science active, PGF & JF need to check on EU)
* a dump of sysConfig would show all video boards down and FEPs in the correct configuration from the command load
* DEA current consistent with video boards off (Need to add value)
* No event data, even if the command load implies there should be
* If a start science command is issued, command echo result = 13 (CMDRESULT_INHIBITED)
* If a stop science command is issued, command echo result = 13 (CMDRESULT_INHIBITED)

What is the response?
---------------------

(will this show up in a standard SOH by OC/CC?  Don't think so but
need to check)
We need to:

* Send an email to the ACIS team at the official anomaly email address, ``acis-anomaly -at- googlegroups -dot- com``.
  If it is off-hours, another ACIS Ops team member should call Peter
  Ford, Jim Francis, and Bob Goeke.
* Send an email to ``sot_red_alert@cfa`` announcing that the ACIS
  radiation monitor flag has been enabled and that science data will
  not be collected until it is disabled.
* Recommend to the Lead Systems Engineer and the Flight Director to
  send the command 1RMONIRM (v=0) to
  de-assert to Radiation Monitor Flag. 

* Contact the GOT Duty Officer to inform that we need the dump data as soon as possible and to
  email or call us when the dump file is available.
* Process the dump data and make sure that there is nothing anomalous in the data *BEFORE*
  the anomaly. Confirm in software housekeeping that the science run
  was inhibited by the radiation monitor flag.
 

Impacts
-------

* While the Radiation Monitor Flag is asserted, ACIS will not take science data.
* When the Radiation Monitor Flag is de-asserted, ACIS will re-start the suspended science run, or start a new science run, depending on the ACIS commands that may have been received while the monitor was asserted.

Relevant Procedures
-------------------

There are no relevant procedures, as this anomaly has not yet occurred.

SOT Procedures
++++++++++++++

*

FOT Procedures
++++++++++++++

*

FOT Scripts
+++++++++++

* 

CLD Scripts
+++++++++++

* 

ACIS Commands
+++++++++++++

* ``1RMONIRM(0)`` De-assert Radiation Monitor Flag

CAPs
++++

*

Relevant Notes/Memos
--------------------

* `ACIS Flight Software User's Guide, Section 3.9, Radiation Monitor Triggers <https://acis.mit.edu/acis/swuserB/#_Toc38976407>`_ Discussion of the ACIS Radmon Flag.


A note on other similar potential anomalies
-------------------------------------------

Spurious high level pulse commands to the PSMC have caused anomalies, but there have been no spurious serial digital commands as of yet.

Note that the radiation monitor flag discussed here is entirely different from the TXINGS patch, which uses ACIS data to detect high radiation environment and then sets the ACIS bilevels to a particular pattern for the OBC to act upon.  The ACIS radiation monitor flag is not used in current operation at all.
