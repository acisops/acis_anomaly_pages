.. _dea-seq-reset:

DEA Sequencer Reset During a Science Observation
================================================

What is it?
-----------

The DEA sequencer crashes, resulting in loss of science data from all
video boards for the rest of the science run.

When did it happen before?
--------------------------

The DEA Sequencer has reset three times during the mission; most recently in 2013:

* June 22, 2004: 2004:174:20:49, obsid 5008
* February 13, 2009: 2009:044:00:14, obsid 10275
* July 30, 2013: 2013:211:05:42, obsid 15474

Will it happen again?
---------------------

It appears likely that the anomaly will occur again.

How is this Anomaly Diagnosed?
------------------------------

* The science run will be terminated, sending a ``scienceReport`` packet that will contain
  non-zero ``fepErrorCodes`` and a non-zero ``terminationCode``.
* Within a major frame (32.2 seconds), there will be a slight drop in DPA Input Current A and/or B,
  (1DPICACU and/or 1DPICBCU) ~0.2-0.3 A. The drop in two input currents depends on which side is
  running which FEPs.
* Within a major frame, the 1STAT1ST bilevel will toggle off (science idle).
* DEA Housekeeping will show sharp drops in the temperatures of FEP 0 and/or FEP 1, if they are
  running.

What is the first response?
---------------------------

Most likely we will be notified by CXCDS Ops that data ceased prematurely for an
observation. We need to:
 
* Process the dump data and get access to the CXC products
* Send an e-mail to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz, 
  and Bev LaMarr)
* Convene a telecon at the next reasonable moment. 
* Examine data from the next observation because the setup for the next
  observation should clear the problem.

Impacts
-------

* The last portion of the science run will be lost. The following science run should be unaffected.

Relevant Notes/Memos
--------------------

Access locked:

* `ACIS-MIT SPR 136 <http://acis.mit.edu/axaf/spr/prob0136.html>`_
* `ACIS-MIT SPR 143 <http://acis.mit.edu/axaf/spr/prob0143.html>`_
* `ACIS-MIT SPR 149 <http://acis.mit.edu/axaf/spr/prob0149.html>`_

Open access, full memo:

* `ACIS-MIT Software Problem Report 136 <ftp://acis.mit.edu/pub/SPR136-1.0.pdf>`_
