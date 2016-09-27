.. _dpa-shutdown:

DPA-A or DPA-B Anomalous Shutdown
=================================

What is it?
-----------

The DPA-A or DPA-B shuts down anomalously, presumably due to a spurious command.

When did it happen before?
--------------------------

The DPA-A has shut down 4 times over the mission and the DPA-B has shut down once. The last DPA-A shutdown was in 2015.

Will it happen again?
---------------------

It appears likely that the anomaly will occur again if the mission continues.

What is the first response?
---------------------------

Our real-time web pages will alert us and the Lead System Engineer will call us. We need to process the dump data and
make sure that there is nothing anomalous in the data *BEFORE* the shutdown. We want to know if a new occurrence looks
just like the previous occurrences. If yes, it should appear as if in one frame the DPA-A (or DPA-B) turned off. We need
to process the dump data, send an email to the ACIS team (including Peter Ford, Bob Goeke, Mark Bautz, and Bev LaMarr),
and convene a telecon at the next reasonable moment. DPA-A shutdowns require reloading the patches, restarting DEA
housekeeping, and resetting the focal plane temperature. DPA-B shutdowns only require that the DPA-B be powered back on.

Impacts
-------

* Until the DPA is powered back on, science operations will be interrupted.

Relevant Procedures
-------------------

.. |dpaa_on| replace:: ``SOP_61038_DPAA_ON``
.. _dpaa_on: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_61038_DPAA_ON.pdf

.. |dpab_on| replace:: ``SOP_61037_DPAB_ON``
.. _dpab_on: http://occweb.cfa.harvard.edu/occweb/FOT/configuration/procedures/SOP/SOP_61037_DPAB_ON.pdf

SOT Procedures
++++++++++++++

* `Turn On DPA-A <http://cxc.cfa.harvard.edu/acis/cmd_seq/dpaa_on.ps>`_
* `Turn On DPA-B <http://cxc.cfa.harvard.edu/acis/cmd_seq/dpab_on.ps>`_
* `Load DEA Housekeeping Parameter Block and Start DEA Housekeeping Run <http://cxc.cfa.harvard.edu/acis/cmd_seq/dea_hkp.ps>`_
* `Set Focal Plane Temperature to -120 C <http://cxc.cfa.harvard.edu/acis/cmd_seq/setfp_m120.ps>`_

FOT Procedures
++++++++++++++

* |dpaa_on|_
* |dpab_on|_
* Load DEA Housekeeping Parameter Block and Start DEA Housekeeping Run ?
* Set Focal Plane Temperature to -120 C ?

Relevant Notes/Memos
--------------------

* `Flight Note 394 <http://cxc.cfa.harvard.edu/acis/memos/FN394.ps>`_
* `Flight Note 417 <http://cxc.cfa.harvard.edu/acis/memos/FN417.ps>`_
