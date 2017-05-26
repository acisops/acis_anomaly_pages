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
  (1DPICACU and/or 1DPICBCU) ~0.2-0.3 A. The drop in two input currents depends on which FEPs
  were active (DPA Side A runs FEPs 0, 1, and 2; DPA side B runs FEPs 3, 4, and 5.)
* Within a major frame, the 1STAT1ST bilevel will toggle off (science idle).
* DEA Housekeeping will show sharp drops in the temperatures of FEP 0 and/or FEP 1, if they are
  running.

If only some of the FEPs were halted and the science run continues, this might be a
case of :ref:`fep_reset`.

If we are in comm when the anomaly occurs, most of the above symptoms will be visible
on PMON displays and the real-time pages. A red ``FEPREC_RESET`` message will occur
in the left Software Housekeeping column, and then scroll up and off the page. There
may also be red ``SMRABORT`` messages, and/or a ``FEP_IO_ERROR`` message.

1. When it's available, obtain the relevant dump data file from the ``/dsops/critical/GOT/input/``
directory on the Ops LAN.

2. Run ACORN on the file as per the instructions in
   `"Running ACORN on data dumps in the case of an anomaly (04/06/16)" <http://cxc.cfa.harvard.edu/acis/memos/Dump_Acorn.html>`_
This provides history of input currents 1DPIC[AB]CU and the status bit 1STAT1ST mentioned above.

3. Run the MIT decom on the data dump as per the instructions in
   `"Memo on Running MIT tools (04/26/16)" <http://cxc.cfa.harvard.edu/acis/memos/Dump_Psci.html>`_
This will provide history of the FEP temperatures mentioned above.

4. MIT personnel can look at the error code, if any, in the science report packet at the
end of the run, and verify the error code mentioned above.


What is the first response?
---------------------------

Most likely we will be notified by CXCDS Ops that data ceased prematurely for an
observation. We need to:
 
* Send an e-mail to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz,
  and Bev LaMarr) to alert them to the existence of the anomaly.
* If we notice the anomaly before CXCDS OPS informs us, we should notify them as well.
* Send a notification fo the Flight Directors including the time of the anomaly and the Obsid
when it occurred.
* Examine data from the next observation because the setup for the next
  observation should clear the problem. This can be done from the realtime
  SW pages.
* Process the dump data and get access to the CXC products to verify that this
  anomaly looks identical or similar to previous occurrences.
* Convene a telecon with the ACIs engineering team at the next reasonable moment
  to review the data and diagnosis.

Impacts
-------

* The last portion of the science run will be lost. The following science run should be unaffected.
* Examine data from the following science run to be sure it is unaffected.

Relevant Notes/Memos
--------------------

Access locked (very terse problem reports):

* `ACIS-MIT SPR 136 <http://acis.mit.edu/axaf/spr/prob0136.html>`_
* `ACIS-MIT SPR 143 <http://acis.mit.edu/axaf/spr/prob0143.html>`_
* `ACIS-MIT SPR 149 <http://acis.mit.edu/axaf/spr/prob0149.html>`_

Open access, full memo:

* `ACIS-MIT Software Problem Report 136 <ftp://acis.mit.edu/pub/SPR136-1.0.pdf>`_
