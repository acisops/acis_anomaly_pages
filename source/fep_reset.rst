.. _fep-reset:

FEP Anomalous Reset
===================

What is it?
-----------
One or more (but not all) FEPs can reset during an observation,
resulting in loss of science data from the affected FEP(s) for the
rest of the science run.

The unaffected FEP(s) will continue to operate normally. This is 
different than the DEA Sequencer Reset During a Science Observation 
anomaly which shuts down all FEPs.

When did it happen before?
--------------------------

FEP resets have occurred three times during the mission; most recently in 2013:

* April 6, 2007, 2007:096:19:24, obsid: 7647
* March 10, 2008, 2008:070:15:31, obsid: 7783
* May 16, 2013, 2013:136:19:57, obsid: 15232

Will it happen again?
---------------------

It appears likely it could happen again. Side B of the DPA seems particularly
susceptible to power transients.

How is this Anomaly Diagnosed?
------------------------------

If all of the FEPs were halted, this could be an instance of :ref:`dea-seq-reset`.
However, if some FEPs were halted but others continued, then the resets were most
likely caused by a momentary "glitch" in the DPA-B +5V supply which halted the 
CPUs that were currently powered by that supply.  

Most likely the first we hear of the problem will be from DS Ops after
they've processed the data. They will report that one FEP has stopped
collecting events. In that case start with Step 3 below to verify.

However, if you happen to be in Comm, and you are checking PMON, you may notice
that one of the FEP's have halted - not collecting events - while the 
others are still operating. 

The following two information lines may still exist on the PMON
page - if they do then:

1. Look for the line that gives the time of the last exposure packet
   from the offending FEPs. It will look like this:

   ``136:19:57:18 - Last exposure packets from FEPs 3 and 4, exposureNumber=11324``

2. Look for when the ``FEPREC_RESET`` occurred. The line will look like
   this:

   ``136:19:57:59 - SwHousekeeping FEPREC_RESET reported, count=2, value=4``


If the time between the last exposure packet and the FEPREC_RESET
less thatn 449 seconds, then the reset was not due to a watchdog
timer-induced reset.


If DS Ops has alerted us to the problem, or if you are looking at PMON
while in Comm and the the data lines have scrolled off the display, then:

3. When it's available, obtain the relevant dump data file from the ``/dsops/critical/GOT/input/``,
   directory which is located on the Ops LAN.

4. Run ACORN on the file as per the instructions in 
   `"Running ACORN on data dumps in the case of an anomaly (04/06/16)" <http://cxc.cfa.harvard.edu/acis/memos/Dump_Acorn.html>`_

5. Run the MIT decom on the data dump as per the instructions in 
   `"Memo on Running MIT tools (04/26/16)" <http://cxc.cfa.harvard.edu/acis/memos/Dump_Psci.html>`_

6. Look at the following data values:

  DEA Houskeeping:
   * DPA5VHKB

  Telemetry MSIDs:
   * 1DPICBCU
   * 1DPP0BVO
   * 1DPICACU
   
   Typically, DPA5VHKB bounces around +/- a volt.  If, however you see
   it steadies up right around the time of the FEP halt, this indicates
   that all DPA-B boards were in a reset state.

   Check to see if the DPA-B current (1DPICBCU) dropped at around the
   same time  while the DPA-B voltage (1DPP0BVO) and the DPA-A current 
   and voltage remained steady.

The above behavior confirms that the FEPs actually reset.

What is the first response?
---------------------------

If you discovered the event during a Comm, then carry out the analysis
above and send an email to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz,
and Bev LaMarr). When you are certain that this is an example of a FEP
Reset, then send a warning to DS Ops, and to the Flight Directors
specifying the Obsid and time where it occurred.

Most likely we will be notified by CXCDS Ops that data from one or more of
the CCDs stopped during an observation. We need to:
 
* Send an email to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz,
  and Bev LaMarr)
* Process the dump data and get access to the CXC products
* Convene a telecon at the next reasonable moment.


Impacts
-------

If the target is not on one of the halted FEPs, then it is likely that the science 
objectives of the observation will be met.

Usually the power down prior to the next observation clears the anomaly. 

We should examine data from the next observation because power-cycling the FEPs 
should clear the condition. But if the next observation uses the same configuration, 
the FEPs will not be power cycled and the anomaly will persist.

Relevant Notes/Memos
--------------------

* Obsid 15232: `ftp://acis.mit.edu/pub/acis-obsid-15232-anom.pdf <ftp://acis.mit.edu/pub/acis-obsid-15232-anom.pdf>`_.
* Obsid 7647: `3-FEP reset anomaly (PS) (7/11/2007) <http://cxc.cfa.harvard.edu/acis/memos/OCCcm08039_closeout.ps>`_
