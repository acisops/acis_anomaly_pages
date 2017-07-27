.. _hi-lo-anomaly:

FEP Thresholder Anomaly
=======================

What is it?
-----------

One CCD starts dropping every second frame part way through the
observation.  In addition in OBSID 18278, two events were reported
multiple times on consecutive (undropped) frames and no events were
reported from rows 1016-1022.  The ACIS flight SW team believes that
this behavior can be explained by a corruption in the radiation-tolerant
memory of the FEP pixel thresholder. The one instance of this anomaly
affected the S1 CCD, but if the explanation is truly a corruption in
the FEP pixel thresholder memory, any FEP/CCD could be affected in a
future anomaly.  If the anomaly were to happen again the behavior of
dropping every other frame should be observed, but the events that are
reported multiple times and the region of the CCD with no event reported
would be different depending on the exact nature of the memory corruption.

When did it happen before?
--------------------------

Once:

* December 16, 2016: obsid 18278

Will it happen again?
---------------------

Unknown. This is the only instance in flight so far.

How is this Anomaly Diagnosed?
------------------------------

Most likely the anomaly will occur out of COM and we will be notified
by V&V that one of the CCDs in an observation is missing every other
exposure after some point in the observation.

These symptoms may be noticed:

*  Every other frame will be missing for one CCD after some point in the observation.  The data before this point should contain all frames.  If the anomaly occurs in realtime COM, the exposure numbers on the impacted CCD will be red on the PMON page, indicating dropped frames. But this can happen for a number of reasons (ratty COM, telemetry saturation, and this anomaly).  The telemetry will probably not saturate for this anomaly, and reported event rates may look normal for the affected CCD.  In addition, only odd or even frame numbers will be reported for that CCD.

* After the anomaly occurs, there may be a region of the CCD for which events had been reported but for which no additional events are reported

* After the anomaly occurs, there may be events that are reported on consecutive (undropped) frames.  These events will have the same chipx,chipy values but the pulse heights will change slowly from frame to frame as the delta overclock values change.  The raw pulse heights for the event and the bias values are not changing, but the overclock values are changing from frame to frame.  This can lead to an event being reported at the same position on consecutive frames until the overclock values change such that the event grade changes to a grade that is not accepted.

What is the first response?
---------------------------

We need to: 

* Send an email to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz, and Bev LaMarr)

* Process the dump data and get access to the CXC products

* Notify sot_yellow_alert and/or brief the 9am telecon

* Convene a telecon at the next reasonable moment. This would include the ACIS team as above. Agreement by MIT via e-mail is a possible alternative.

* Examine data from the next observation, because the setup for the next observation will clear the problem.

.. |sop_diagnostics| replace:: ``SOP_ACIS_DEA_FEP_DIAGNOSTICS``
.. _sop_diagnostics: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DEA_FEP_DIAGNOSTICS.pdf

In the somewhat unlikely event that this anomaly is
noticed during an observation, and there is time either
during the current comm or a following one (still on
the same obsid), we have
`an SOP (DEA FEP Diagnostics SOP) <http://cxc.cfa.harvard.edu/acis/cmd_seq/dea_fep_diags.pdf>`_
written to intervene if the observation with the anomaly is still in
progress to dump diagnostic data.

This procedure suspends the observation while the diagnostics are run, and then resumes
all chip/FEP combinations except the problematic one. If the target is not on the
affected chip, it may be advantageous to the user to skip the diagnostic procedure
and finish the observation uninterrupted.

* Send an e-mail to sot_red_alert and convene a telecon giving the plan, and the need for quick action to get the SOP done before the end of the science run.

* Prepare a CAP and submit it for review to cap_review@ipa.cfa.harvard.edu

* The steps below should also be followed, as appropriate.

Impacts
-------

* The last portion of the science run for that particular CCD/FEP combination will be compromised. The following science run should be unaffected.

Relevant Procedures
-------------------

SOT Procedures
++++++++++++++

* `DEA and FEP Diagnostics Procedure <http://cxc.cfa.harvard.edu/acis/cmd_seq/dea_fep_diags.pdf>`_

FOT Procedures
++++++++++++++

* |sop_diagnostics|_

Relevant Notes/Memos
--------------------

* `ACIS FEP Anomaly during OBSID 18278 <ftp://acis.mit.edu/pub/acis-18278-anom-v1.2.pdf>`_

* `Flight Note 585 <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note585_ACIS_S1_Dropped_Frames_Closeout.pdf>`_


