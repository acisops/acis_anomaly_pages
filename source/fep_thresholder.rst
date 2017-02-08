.. _hi-lo-anomaly:

FEP Thresholder Anomaly
=======================

What is it?
-----------

One chip (S1) started dropping every second frame part way through the observation. A couple pixels seemed to be stuck on, returning the same 3x3 island of pulse heights in every frame.

When did it happen before?
--------------------------

Once:

* December 16, 2016: obsid 18278

Will it happen again?
---------------------

Unknown. This is the only instance in flight so far.

How is this Anomaly Diagnosed?
------------------------------

Both of the following symptoms may be noticed:

* Data System Operations reports the anomaly to us at V&V time.
* Exposure numbers on the impacted chip will be red on the PMON page, indicating dropped frames, but that can happen for a number of reasons (ratty comm, telemetry saturation, this anomaly). The telemetry will probably not saturate for this anomaly, and reported event rates will look normal for the affected chip. In addition, only odd or even frame numbers will be reported for that chip.


What is the first response?
---------------------------

If this anomaly is noticed during an observation, and there is time either during
the current comm or a following one (still on the same obsid),
we have `a SOP <http://cxc.cfa.harvard.edu/acis/cmd_seq/dea_fep_diags.pdf>`_ 
written to intervene if the observation with the anomaly is still in progress to dump diagnostic data.

* Send an e-mail to sot_red_alert and convene a telecon giving the plan, and the need for quick action to get the SOP done before the end of the science run.
* Prepare a CAP and submit it for review to cap_review@ipa.cfa.harvard.edu
* The steps below should also be followed, as appropriate.

If not, we need to: 

* Send an email to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz, and Bev LaMarr)
* Process the dump data and get access to the CXC products
* Notify sot_yellow_alert and/or brief the 9am telecon
* Convene a telecon at the next reasonable moment. This would include the ACIS team as above. Approval by MIT via e-mail is a possible alternative.
* Examine data from the next observation, because the setup for the next observation should 
  clear the problem.

.. |sop_diagnostics| replace:: ``SOP_ACIS_DEA_FEP_DIAGNOSTICS``
.. _sop_diagnostics: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DEA_FEP_DIAGNOSTICS.pdf

Impacts
-------

* The last portion of the science run for that particular CCD/FEP combination will be 
  compromised.
  The following science run should be unaffected.

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

