.. _fep-reset:

FEP Anomalous Reset
===================

What is it?
-----------

One or more (but not all) FEPs can reset during an observation, resulting in 
loss of science data from the affected FEPs for the rest of the science run.

The unaffected FEPs will continue to operate normally. This is different than 
the DEA Sequencer Reset During a Science Observation anomaly which shuts down 
all FEPs.

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

To be certain:

1. Obtain the relevant dump data file from the directory ``/dsops/critical/GOT/input/``,
   which is located on the Ops LAN.

2. Run ACORN on the file as per the instructions in 
   `"Running ACORN on data dumps in the case of an anomaly (04/06/16)" <http://cxc.cfa.harvard.edu/acis/memos/Dump_Acorn.html>`_

3. Run the MIT decom on the data dump as per the instructions in 
   `"Memo on Running MIT tools (04/26/16)" <http://cxc.cfa.harvard.edu/acis/memos/Dump_Psci.html>`_

4. Look for the line that gives the tiem of the last exposure packet
   from the offending FEPs. It will look like this:

   ``136:19:57:18 - Last exposure packets from FEPs 3 and 4, exposureNumber=11324``

5. Look for when the ``FEPREC_RESET`` occurred. The line will look like
   this:

   ``136:19:57:59 - SwHousekeeping FEPREC_RESET reported, count=2, value=4``

   If the time between those two events is less thatn 449 seconds, then the reset 
   was not due to a watchdog timer-induced reset.

6. Look at the following MSIDs:

   * DPA5VHKB
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

Most likely we will be notified by CXCDS Ops that data from one or more of
the CCDs stopped during an observation. We need to:
 
* Send an email to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz,
  and Bev LaMarr)
* Process the dump data and get access to the CXC products
* Convene a telecon at the next reasonable moment.

Impacts
-------

Usually the power down prior tot he next observation clears the anomaly. If 
the target is not on one of the halted FEPs, then it is likely that the science 
objectives of the observation will be met.

We should examine data from the next observation because power-cycling the FEPs 
should clear the condition. But if the next observation uses the same configuration, 
the FEPs will not be power cycled and the anomaly will persist.

Relevant Notes/Memos
--------------------

* Obsid 15232: `ftp://acis.mit.edu/pub/acis-obsid-15232-anom.pdf <ftp://acis.mit.edu/pub/acis-obsid-15232-anom.pdf>`_.
* Obsid 7647: `3-FEP reset anomaly (PS) (7/11/2007) <http://cxc.cfa.harvard.edu/acis/memos/OCCcm08039_closeout.ps>`_
