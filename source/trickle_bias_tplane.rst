.. _trickle-bias-tplane:

Trickle Bias / T-Plane Latchup Anomaly
======================================

What is it?
-----------

Science event telemetry begins before bias map has been telemetered,
overloading direct memory transfers from one or more FEPs, resulting
in locked values for the FEP Threshold Plane.

When did it happen before?
--------------------------

Three times:

* June 27, 2000: obsid 371
* October 29, 2001: obsid 3403
* November 4, 2001: obsid 2010

Will it happen again?
---------------------

Most likely not. The ``untricklebias`` patch, installed in Flight Software
Version B-opt-C in 2003, calls all ``biasThief`` methods from within the
science thread, preventing simultaneous telemetry of bias and event
packets.

How is this Anomaly Diagnosed?
------------------------------

We are speaking of two symptoms, interleaved bias and event packets,
and frozen threshold crossing values. The most immediately obvious 
symptom is saturated telemetry, with far too many events from the 
affected FEP or FEPs. On an affected FEP, the threshold crossing 
counts will not change from one frame to the next. The symptoms 
disappear with the next science run, and may become apparent only 
when SSR data are examined.

What is the first response?
---------------------------

In 2001, the response was to recycle FEP power. This is now 
automatically built into the start of each SI mode command sequence, 
and is unnecessary. The subsequent science run in the load will 
execute normally, so no corrective action is necessary.

If we see T-Plane latchup on coming into comm, it may be worth trying
to salvage the remainder of the science run. Check whether a CLD
exists for a ``WSPOWXXXXX`` packet to command the required set of DEA boards
and FEPs. If so, and significant time remains for the obsid, execute
stop science, ``WSFEPALLDN``, the ``WSPOWXXXXX`` command, and start science.

A special case: if the following science run is a no-bias version of
the same SI mode, it will not recycle the FEP power. Send a stop
science and a ``WSFEPALLDN`` command to ACIS.

Afterward, the team may examine the telemetry stream at leisure to see 
what may have triggered the latchup. If there was no interleaving of
bias and events, look for any other simultaneous high demand on DMA
transfers out of the affected FEPs.

Impacts
-------

Once the T-Plane latches, science will be lost from the latched FEPs
for the rest of the science run, and some science is likely to be
lost from remaining FEPs due to telemetry saturation.

Relevant Procedures
-------------------

.. |stop_sci| replace:: ``1A_AA000_191.CLD``, ``AA00000000`` (stop science command)
.. _stop_sci: http://acis.mit.edu/cgi-bin/get-cld?cld=1A_AA000_191.CLD

.. |feps_down| replace:: ``1A_WS003_165.CLD``, ``WSFEPALLDN`` (command to power down all FEPs)
.. _feps_down: http://acis.mit.edu/cgi-bin/get-cld?cld=1A_WS003_165.CLD


Command Files
+++++++++++++

* |stop_sci|_
* |feps_down|_

Currently there are 2 6-chip and 3 5-chip power commands in CLDs. The
5-chip commands all power an additional, unneeded FEP.

Relevant Notes/Memos
--------------------

* `ACIS Software Problem Report M00062901: SPR 133: Event Packets interleaved with Bias Packets, causing FEP T-Plane Lock-Up. <http://acis.mit.edu/axaf/spr/prob0133.html>`_
* `MIT ECO # 36-1028 untricklebias: Patch to run bias thief methods from science task <http://acis.mit.edu/axaf/eco/eco-1038.pdf>`_


