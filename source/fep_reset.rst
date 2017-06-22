.. _fep-reset:

FEP Anomalous Reset
===================

What is it?
-----------

One or more (but not all) FEPs can reset during an observation, resulting in 
loss of science data from the affected FEP(s) for the rest of the science run. 
The unaffected FEP(s) will continue to operate normally. This is different than 
the :doc:`../dea_seq_reset` anomaly which shuts down all FEPs. 

When did it happen before?
--------------------------

FEP resets have occurred three times during the mission; most recently in 2013:

* April 6, 2007, 2007:096:19:24:38.27, obsid: 7647
* March 10, 2008, 2008:070:15:31:51.08, obsid: 7783
* May 16, 2013, 2013:136:19:57:18, obsid: 15232

Will it happen again?
---------------------

It appears likely it could happen again. Side B of the DPA seems particularly
susceptible to power transients.

How is this Anomaly Diagnosed?
------------------------------

If all of the FEPs were halted, this could be an instance of the 
:doc:`../dea_seq_reset` anomaly. However, if some FEPs were halted 
but others continued, then the resets were most likely caused by a 
momentary "glitch" in the DPA-B +5V supply which halted the CPUs 
that were currently powered by that supply.

If some FEPs are halted and their ``deltaOverclock`` values are 
large and negative for the first three output nodes and large and 
positive for the fourth output node, this may be an instance of the
:doc:`../hi_lo_anomaly`.

If this is a suspected FEP Reset anomaly, some analysis is necessary 
to be certain.

If you are watching PMON during a Comm and the event occurs while you
are watching then:

1. Before the anomaly, the ``pmon`` display should look normal.

   If you happened to have comm right when the reset happened, you
   would see ``FEPREC_RESET`` in red in the bottom left column that 
   would eventually scroll off as more housekeeping packets came in. 
   The CCD/FEP statistics in the middle section would stop accumulating
   for the FEP(s) that had reset.

   If you miss the FEP reset itself, but get comm afterwards, ``pmon``
   does not complain, but you would notice that fewer CCD/FEPs are
   accumulating statistics in the center section than you would
   expect. The header part of the middle section, with the CCD/FEP
   assignments, would still show the FEPs that had reset, but the 
   display would be static or blank depending on whether pmon had 
   seen any data from the current observation previous to the anomaly. 
   So there is an indication that something is amiss, but it is not 
   in red flashing letters.

   If you saw the science report at the end of the run, you would see
   that the FEPs that had reset would have many fewer exposures/events/etc.

   If you missed the FEP reset itself but suspect one occurred because
   PMON reports that a FEP or FEPs stopped collecting, OR if DS Ops
   has alerted us to the fact that one or more FEPs stopped
   collecting, then you can proceed to step 2.

2. Look at the following DEA Housekeeping and MSID values:

   DEA Housekeeping: DPA5VHKB

   Telemetry MSIDs: 1DPICBCU, 1DPP0BVO, 1DPICACU

   Typically, DPA5VHKB bounces around +/- a volt. However, if you see
   it steadies up right around the time of the FEP halt, this indicates
   that all DPA-B boards were in a reset state. Check to see if the DPA-B
   current (1DPICBCU) dropped at around the same time while the DPA-B 
   voltage (1DPP0BVO) and the DPA-A current and voltage remained steady. 
   The above behavior confirms that the FEPs actually reset.

To look at these values:


3. When available, obtain the relevant dump data file from the directory:

   ``/dsops/critical/GOT/input/`` 
   
   which is located on the Ops LAN, and extract the relevant MSIDs.

4. There are a total of 8 boards in the DPA, powered independently:

   DPA-A +5V to BEP-A, FEP0, FEP1, and FEP2

   DPA-B +5V to BEP-B, FEP3, FEP4, and FEP5

   Previous occurrences of this anomaly were on the DPA-B side,
   affecting only the side B FEPs. In that case, one should see:

   - No change in behavior of DPA 5V Analog A (1DPP0AVO) and DPA Input 
     Current A (1DPICACU) within one major frame (32.2 seconds)
   - DPA 5V Analog B (1DPP0BVO) becomes steady (also seen in DEA
     Housekeeping, DPA5VHKB)
   - DPA Input Current B (1DPICBCU) drops

.. raw:: html
   <br>

5. The timeline of events in the anomaly can be reconstructed by
   examining the packet data in the MIT psci data files. Peter Ford,
   Joan Quigley, Royce Buehler, or Catherine Grant can do that. They
   should find the time of the last event packets from the affected FEPs
   and the time that the FEP resets were reported in software
   housekeeping.

   If the time between these two events is less than 449 seconds, then
   the reset was not due to a watchdog timer reset. See 
   `Peter Ford's Obsid 15232 memo <ftp://acis.mit.edu/pub/acis-obsid-15232-anom.pdf>`_ 
   for more information. 


What is the first response?
---------------------------

If you happen to observe the incident on PMON, send a warning email to
CXCDS Ops (send to ``operators@cfa`` or ``ascdsops@head``). Then do the 
analysis above when the data is available. If that analysis confirms a 
FEP reset, then send an email to the Flight Directors alerting them of 
the incident.

Most likely, we will be notified by CXCDS Ops that data collection on one or more of
the CCDs stopped during an observation. We need to:

* Send an e-mail to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz,
  and Bev LaMarr) to alert them to the existence of the anomaly.

* Examine data from the next observation, because in most cases the setup for 
  the next observation should clear the problem (though see the note below in 
  :ref:`fep_reset_impacts`). This can be done from the realtime SW pages.

* Process the dump data and get access to the CXC products to verify that this
  anomaly looks identical or similar to previous occurrences.

* Convene a telecon with the ACIS engineering team at the next reasonable moment 
  to review the data and diagnosis.

.. _fep_reset_impacts:

Impacts
-------

* If the target is not on one of the halted FEPs, then it is likely that
  the science objectives of the observation will still be met.  

* We should examine data from the next observation because power-cycling the FEPs 
  via the execution of the ``WSPOW00000`` command should clear the condition. 
  However, any run immediately following which executes ``WSVIDALLDN`` instead 
  (such as an event histogram or no-bias run) may be affected, since in this 
  case the anomaly is likely to persist.

Relevant Notes/Memos
--------------------

* Obsid 15232: `ACIS OBSID 15232 Anomaly (5/17/2013) <ftp://acis.mit.edu/pub/acis-obsid-15232-anom.pdf>`_
* Obsid 7647: `3-FEP reset anomaly (7/11/2007) <http://cxc.cfa.harvard.edu/acis/memos/OCCcm08039_closeout.pdf>`_
