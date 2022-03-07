.. _bep-buscrash:

BEP Bus Crash Anomaly
=====================

What is it?
-----------


A ``FATAL_INTR_FEP_BUS_ERROR`` on ACIS causes a watchdog reboot to ACIS v11 flight software.  This is thought to be caused by an SEU in the BEP or FEP memory.  ACIS will continue to perform observations, but the calibration will be unreliable.


When did it happen before?
--------------------------

Two instances are believed to be cases of this class of BEP bus-crash anomaly, most recently in Jan. 2022:

* October 05, 2008, 2008:279:01:47:59, obid: 9209
* January 16, 2022, 2022:016:00:05:23, obsid: 25741


Will it happen again?
---------------------

There is nothing to prevent another similar SEU (which is the leading candidate explanation), so it can recur. 

How is this Anomaly Diagnosed?
------------------------------

PMON will show that the software will has reset to v11, with the watchdogFlag set to 1.  (Note that the warmbootFlag will likely be set to unity, which does not indicate that a warmboot occurred, but rather than a commanded boot would apply patches.)  A fatalMessage packet written into telemetry by the active BEP reports the BEP CPU's BadVaddr register.


DPA power and current (``1DPICACU``, ``1DPICBCU``)
will drop at the time of reset, on either or both sides.  But at the next science observation will resume usual and expected behavior.  DEA power and current (``1DEICACU``) should be unaffected.  DPA voltages may exhibit a very minor drop, but this change would be miniscule compared to the impact on currents.  


To diagnose the event:

1. When available, obtain the relevant dump data file from the directory:

   ``/dsops/critical/GOT/input/`` 
   
   which is located on the Ops LAN, and extract the relevant MSIDs using
   ACORN, as per the instructions in
   `"Running ACORN on data dumps in the case of an anomaly (04/06/16)" <http://cxc.cfa.harvard.edu/acis/memos/Dump_Acorn.html>`_.
   The MSID data in the tracelog can then be examined using ACISpy, see
   the links below for details.


2. The timeline of events in the anomaly can be reconstructed by
   examining the packet data in the MIT psci data files. Peter Ford,
   Joan Quigley, Royce Buehler, or Catherine Grant can do that. They
   should confirm the fatalMessage packet.

   

What is the first response?
---------------------------


* Call a red-alert telecon, and plan to run SCS-107 to stop the science load running and assess the overall state of ACIS.
  
* Be sure that the full ACIS team (including Ken Gage, Peter Ford, Bob Goeke, Mark Bautz,
  Jim Francis, and Bev LaMarr) is alerted to the anomaly.  

* Process the dump data and get access to the CXC products to verify that this
  anomaly looks identical or similar to previous occurrences.
  
* Convene a telecon with the ACIS engineering team at the next reasonable moment  to review the data and diagnosis.
  
* Prepare a CAP to dump patches and to warmboot upon successful patch verification.  (CAP 1597, by Ken Gage, should be the template; see below.)

.. _fep_reset_impacts:


Impacts
-------

* All science data after the interrupt likely need to be re-observed (having been either interrupted by the reboot, or collected in v11).

* Peter Ford has proposed potential patches to either report additional details of the event or to avoids some possible pathways for this to occur.  None is presently active.



Relevant Notes/Memos
--------------------


* ObsID 25741: `BEP Bus Error in ObsID 25741 <https://acisweb.mit.edu/pub/buserr-25741-v1.1.pdf>`_

* ObsID 9209: `ACIS OBSID 9209 Anomaly Closeout (9/8/2009) <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/flightnotes/controlled/Flight_Note498_ACIS_OBSID_9209_Anomaly.pdf>`_

* CAP 1597: `Restart ACIS <https://occweb.cfa.harvard.edu/occweb/FOT/configuration/CAPs/1501-1600/CAP_1597_Restart_ACIS/CAP_1597_Restart_ACIS.pdf>`_
  
.. |mptl| replace:: ``multiplot_tracelog`` Command-line Script
.. _mptl: http://cxc.cfa.harvard.edu/acis/acispy/command_line.html#multiplot-tracelog

Relevant ACISpy Links
---------------------

* `Reading MSID Data from Tracelog File <http://cxc.cfa.harvard.edu/acis/acispy/loading_data.html#reading-msid-data-from-a-tracelog-file>`_
* `Plotting Data in Python <http://cxc.cfa.harvard.edu/acis/acispy/plotting_data.html>`_
* |mptl|_
