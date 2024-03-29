.. _hi-lo-anomaly:

Hi/Lo Pixel Anomaly
===================

What is it?
-----------

Event data stops being reported for one or more CCD/FEP combinations and the delta 
overclock values are peculiar, large and negative for the first three output nodes 
and large and positive for the fourth output node.

When did it happen before?
--------------------------

Twice:

* March 7, 2011: obsid 12934
* October 31, 2013: obsid 16496

Will it happen again?
---------------------

It appears likely it could happen again.

How is this Anomaly Diagnosed?
------------------------------

Both of the following symptoms will be noticed:

* One or more FEPs will stop returning event data.
* The ``deltaOverclock`` values reported from these FEPs are large and negative 
  for the first three output nodes and large and positive for the fourth output 
  node. If one sees this, this is a clear indication of the Hi/Lo Pixel Anomaly 
  and not a :doc:`../fep_reset`.

Both of these symptoms can be observed from one of the PMON pages. We should also
receive red alert emails and text messages from PMON which say "Repeated consecutive 
overclock values of 0 or 4095" and/or "Repeated consecutive delta-overclocks outside 
expected range".

What is the first response?
---------------------------

If you happen to observe the incident on PMON, send a warning email to
CXCDS Ops (send to ``operators@cfa`` or ``ascdsops@head``). Importantly, 
we have a `SOP <http://cxc.cfa.harvard.edu/acis/cmd_seq/dea_fep_diags.pdf>`_ 
written to intervene if the observation with the anomaly is still in 
progress to dump diagnostic data. If the analysis confirms the anomaly,
then send an email to the Flight Directors alerting them of the incident.

Otherwise, the most likely scenario is that we will be notified by CXCDS Ops that 
data collection on one or more of the CCDs stopped during an observation. We need to:

* Send an e-mail to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz,
  and Bev LaMarr) to alert them to the existence of the anomaly.

* Examine data from the next observation, because in most cases the setup for 
  the next observation should clear the problem (though see the note below in 
  :ref:`hi_lo_impacts`). This can be done from the realtime SW pages.

* Process the dump data and get access to the CXC products to verify that this
  anomaly looks identical or similar to previous occurrences. In particular, 
  if the anomaly did occur, the dump data will reveal that for the affected FEPs
  the sum of the ``initialOverclock`` and ``deltaOverclock`` values will be 
  consistent with zero for the first three output nodes and will be close to
  a value of 4095 for the last output node. 

* Convene a telecon with the ACIS engineering team at the next reasonable moment 
  to review the data and diagnosis.

.. |sop_diagnostics| replace:: ``SOP_ACIS_DEA_FEP_DIAGNOSTICS``
.. _sop_diagnostics: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_ACIS_DEA_FEP_DIAGNOSTICS.pdf

.. _hi_lo_impacts:

Impacts
-------

* The remaining portion of the science run for the particular CCD/FEP 
  combinations exhibiting the anomaly will be lost. 

* In most situations, the following science run should be unaffected, 
  as a power-cycle of the affected board via execution of the ``WSPOW00000``
  command should clear the problem. However, any run immediately following 
  which executes ``WSVIDALLDN`` instead (such as an event histogram or 
  no-bias run) may be affected, since in this case the anomaly may persist.

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

* `Hi/Lo Pixel Anomaly Closeout Report <http://cxc.cfa.harvard.edu/acis/memos/OCCcm09291_DDTS_Closeout.txt>`_
* `The ACIS Hi-Lo Pixel Anomaly <https://acisweb.mit.edu/pub/hi-lo-pixel-anomaly-v1.4.pdf>`_
