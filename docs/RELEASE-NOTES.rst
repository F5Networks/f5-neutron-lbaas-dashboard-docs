.. index:: Release Notes

.. _Release Notes:

Release Notes for F5 Dashboard for OpenStack LBaaS
==================================================

|version| (Mitaka)
------------------

This version of the F5 Dashboard for OpenStack LBaaS supports OpenStack Mitaka release.


Added Functionality
```````````````````

* Advanced Load Balancer
  - Configure Enhanced Service Definition(ESD) for LoadBalancer's listener.


Bug Fixes
`````````

None.

9.0.0 (Mitaka)
--------------

This version of the F5 Dashboard for OpenStack LBaaS supports the OpenStack Mitaka release.

Added Functionality
```````````````````

* SSL Offloading:

  - Configure terminated HTTPS load balancer.
  - Create Barbican SSL certificate container.

Bug Fixes
`````````

None.

Limitations
```````````

- :ref:`PKCS12 certificate bundle limitation <p12-limitation>`

  |dashboard-long| does not support PKCS12 certificate bundle import. If you need to use a PKCS12 certificate bundle in your load balancer, see :ref:`Configure a Terminated HTTPS Load Balancer with a PKCS12 Certificate Bundle <p12-limitation>`.

- :ref:`Server Name Indication (SNI) limitation <sni-limitation>`

  If you need to create a load balancer that uses TERMINATED_HTTPS protocol with SNI enabled, see :ref:`Configure a SNI Load Balancer <sni-limitation>`.
