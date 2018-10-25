.. _sni-limitation:

Configure an SNI TLS Load Balancer Using the F5 Dashboard for Neutron LBaaS
===========================================================================

Background
----------

When you create a new load balancer on the |dashboard-long| that uses *TERMINATED_HTTPS* and a new certificate(s), the F5 Agent creates a new virtual server and SSL profile on your BIG-IP device. You can also create a new load balancer using an existing certificate/BIG-IP SSL profile.

The relation between certificates added to the |dashboard-short| and BIG-IP SSL profiles is one-to-one. Each certificate you add using the dashboard creates a corresponding SSL profile on the BIG-IP device. You can use certificates with more than one load balancer, just as you can associate a BIG-IP SSL profile with more than one virtual server.


SNI TLS Load Balancer
---------------------

A Neutron LBaaS load balancer that uses multiple certificates is an "SNI TLS Load Balancer". The corresponding BIG-IP virtual server also has multiple associated SSL profiles. One of the associated SSL profiles will be the "Default SSL Profile for SNI".

.. caution::

   Setting more than one SSL profile as the "Default SSL Profile for SNI" will cause virtual server creation to fail.

Because the OpenStack dashboard has no visibility into the state of the BIG-IP device, you need to check the BIG-IP system settings to find out which SSL profile is the default for SNI.

The recommended way is **NOT to share any certificate of an SNI TLS load balancer with any other load balancer**. If you hope to share one certificate between SNI TLS load balancers, you need to create a dedicated certificate object in Barbican for each SNI TLS load balancer.
