.. _trickle-bias-tplane:

Trickle Bias / T-Plane Latchup Anomaly
======================================

What is it?
-----------

Science event telemetry begins before bias map has been fully telemetered. Bias
maps may be truncated. The anomaly was first seen when conflicts in direct
memory transfers from science and bias-thief tasks triggered a bug in the ACTEL
firmware, which led to all values in the threshold bit plane(s) of the relevant
FEP(s) being locked.

When did it happen before?
--------------------------

Thirteen times:

* June 26, 2000: 2 FEPs latched
* January 21, 2001: interleaving of bias and events, no latchup
* March 14, 2001: interleaving, no latchup, one short bias
* October 29, 2001: 2 FEPs latched
* November 4, 2001: 1 FEP latched
* July 9, 2005: 2 FEPs latched
* May 9, 2006: interleaving only
* April 19, 2008: interleaving only
* March 25, 2011: interleaving only
* March 28, 2012: interleaving only
* June 22, 2012: interleaving only
* February 17, 2013: interleaving, bias maps truncated or lost
* April 1, 2013: interleaving only
* July 24, 2013: interleaving, 4 bias maps truncated or lost

Will it happen again?
---------------------

Most likely not. The ``untricklebias`` patch, installed in Flight Software Version
B-opt-C in 2003, calls all ``biasThief`` methods from within the science thread,
preventing latchup. The installation of ``buscrash``, and later of an updated
``buscrash2`` patch in Flight Software version F-opt-G in 2013, eliminated the
trickle-bias failure altogether. F-opt-G also retired the ``untricklebias`` patch.
This history is best described in `MIT ECO-1047 <http://acis.mit.edu/axaf/eco/eco-1047.pdf>`_.


How is this Anomaly Diagnosed?
------------------------------

We are speaking of two symptoms, interleaved bias and event packets, and frozen
threshold crossing values:

* If bias packets appearing after the first event packets is the only symptom,
  and bias maps are truncated, this will be apparent when SSR data are processed.
  Likely the CXC DS Ops will be the first to notice it.
* If bias packets appear after the first event packets, but biases are complete,
  the situation is benign. It will have no effect on CXC DS Ops data processing,
  and no action (or even diagnosis) is necessary.
* If the T-plane has latched, the most immediately obvious symptom is saturated
  telemetry on one or more FEPs, with far too many events, and skipped frames on
  those FEPs. On an affected FEP, the threshold crossing counts in pmon will not
  change from one frame to the next. The symptoms disappear after the FEP power 
  cycle in the next science run,, and may become apparent only when SSR data are 
  examined. 

If the next science run has started before the next comm, a latched T-plane
anomaly will not be apparent until SSR data are processed.

What is the first response?
---------------------------

* In 2001, the response was to recycle FEP power. This is now automatically built
  into the start of each SI mode command sequence in the ACIS tables with bias.
  The subsequent science run in the load will execute normally, so no corrective
  action is necessary.

* A special case: if the following science run is a no-bias version of the same
  SI mode, it will not recycle the FEP power. Send a stop science and a
  ``WSFEPALLDN`` command to ACIS. The following run will now take a bias.

* If we see T-plane latchup on coming into comm, it may be worth trying to salvage
  the remainder of the science run. Check whether a CLD exists for a ``WSPOWXXXXX``
  packet to command the required set of DEA boards and FEPs. If so, and significant
  time remains for the obsid, execute stop science, ``WSFEPALLDN``, the ``WSPOWXXXXX``
  command, and start science. Because the FEPs have been recycled, the bias will be 
  recomputed, so not all the subsequent science time will be recovered.

* Afterward, the team may examine the telemetry stream at leisure to see what may have
  triggered the latchup. If there was no interleaving of bias and events, ask the MIT
  ACIS team to look for any other simultaneous high demand on DMA transfers out of the
  affected FEPs.

* If there is no latchup, but some bias maps are truncated or missing, advise the CXC
  DS Ops, who will replace the bias maps with equivalent recent ones.

Impacts
-------

* Once the T-plane latches, science will be lost from the latched FEPs for the rest
  of the science run, and some science is likely to be lost from remaining FEPs due
  to telemetry saturation.

Relevant Procedures
-------------------

.. |stop_sci| replace:: ``1A_AA000_191.CLD``, ``AA00000000`` (stop science command)
.. _stop_sci: http://acis.mit.edu/cgi-bin/get-cld?cld=1A_AA000_191.CLD

.. |feps_down| replace:: ``1A_WS003_165.CLD``, ``WSFEPALLDN`` (command to power down all FEPs)
.. _feps_down: http://acis.mit.edu/cgi-bin/get-cld?cld=1A_WS003_165.CLD

.. |te_start| replace:: ``1A_XT000_190.CLD``, ``XTZ0000005`` (start TE science)
.. _te_start: http://acis.mit.edu/cgi-bin/get-cld?cld=1A_XT000_190.CLD

.. |cc_start| replace:: ``1A_XCZ000_190.CLD``, ``XCZ0000005`` (start CC science)
.. _cc_start: http://acis.mit.edu/cgi-bin/get-cld?cld=1A_XCZ000_190.CLD

Command Files
+++++++++++++

* |stop_sci|_
* |feps_down|_
* |te_start|_
* |cc_start|_ 

Currently there are 3 6-chip, 4 5-chip, and 1 4-chip power commands in CLDs. 
These commands power on all 6 FEPs, whether or not they are in use.

6-chip power commands:

==============  ======================  ============
Power Command   CLD File                Chips
==============  ======================  ============
``WSPOW0CF3F``	``1A_WS028_140.CLD``	  I0-I3, S2-S3
``WSPOW3F03F``	``1A_WS00F_233.CLD``	  S0-S5
``WSPOW18F3F``	``1A_WSPOW1_233.CLD``	  I0-I3, S3-S4
==============  ======================  ============

5-chip power commands:

==============  ======================  ============
Power Command   CLD File                Chips
==============  ======================  ============
``WSPOW08F3F``	``1A_WS03E_140.CLD``	  I0-I3, S3
``WSPOW04F3F``	``1A_WSP043F_140.CLD``  I0-I3, S2
``WSPOW1CC3F``	``1A_WS040_233.CLD``	  I2-I3, S2-S4
``WSPOW3E03F``	``1A_WS03F_233.CLD``	  S1-S5
==============  ======================  ============

4-chip power commands:

==============  ======================  ============
Power Command   CLD File                Chips
==============  ======================  ============
``WSPOW003F``	  ``1A_WS045_140.CLD``	  I0-I3
==============  ======================  ============

Relevant Notes/Memos
--------------------

* `ACIS Software Problem Report M00062901: SPR 133: Event Packets interleaved with Bias Packets, causing FEP T-Plane Lock-Up. <http://acis.mit.edu/axaf/spr/prob0133.html>`_
* `ACIS Software Problem Report M13021701: SPR 148: Premature end of biasThief output <http://acis.mit.edu/axaf/spr/prob0148.html>`_
* `ACIS Software Problem Report M13080801: SPR 150: Missing exposures after trickleBias anomaly in 53531 <http://acis.mit.edu/axaf/spr/prob0150.html>`_
* `MIT ECO 36-1028 untricklebias: Patch to run bias thief methods from science task <http://acis.mit.edu/axaf/eco/eco-1028.pdf>`_
* `MIT ECO 36-1034 Flight S/W patch to prevent bus crash on FEP power-down <http://acis.mit.edu/axaf/eco/eco-1034.pdf>`_
* `MIT ECO 36-1038 Flight S/W patch to prevent bus crash on FEP power-down <http://acis.mit.edu/axaf/eco/eco-1038.pdf>`_
* `MIT ECO 36-1047 buscrash2 patch to prevent Trickle-Bias anomalies and BEP crashes <http://acis.mit.edu/axaf/eco/eco-1047.pdf>`_
