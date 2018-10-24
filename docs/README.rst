F5 Dashboard for OpenStack Neutron LBaaS
========================================

.. sidebar:: **OpenStack version:**

   The |dashboard-long| supports OpenStack Mitaka release.

|Build Status|

.. raw:: html

    <script async defer src="https://f5-openstack-slack.herokuapp.com/slackin.js"></script>

.. toctree::
   :hidden:
   :caption: Contents
   :maxdepth: 1
   :glob:

   RELEASE-NOTES
   p12-limitation
   sni-limitation

Version |version|
-----------------

:ref:`Release Notes`

The |dashboard-long| (``f5-neutron-lbaas-dashboard``) is an OpenStack `Horizon dashboard plugin <https://docs.openstack.org/security-guide/networking/architecture.html>`_.
It works in conjunction with the `F5 Driver for OpenStack Neutron LBaaS <https://clouddocs.f5.com/products/openstack/lbaasv2-driver/latest/>`_ and `F5 Agent for OpenStack Neutron LBaaS <https://clouddocs.f5.com/products/openstack/agent/latest/>`_ to manage F5 BIG-IP `Local Traffic Manager <https://f5.com/products/big-ip/local-traffic-manager-ltm>`_ (LTM) services via the OpenStack Neutron API.

.. seealso::

   For more information about how the |dashboard-short| interacts with the Neutron API and BIG-IP devices, see `Configure SSL Offloading On F5 Neutron LBaaS Dashboard`_.

Downloads
---------

|rpm-download|

Installation
------------

Follow the instructions for your distribution to install the |dashboard-long| on your Horizon controller.

**Note**: This document assumes that you already have RDO Mitaka release installed as your OpenStack control plane with Horizon and Barbican enabled.

RPM
```

#. Download and Install |dashboard|.

   .. parsed-literal::

      curl -L -O |f5_dashboard_rpm_url|
      rpm -ivh |f5_dashboard_rpm_package|

#. Make sure the install script has enabled |dashboard-short|. If not, please manually copy it to your Horizon 'enabled' directory.

   .. parsed-literal::

      ls /usr/share/openstack-dashboard/openstack_dashboard/local/enabled/_1491_project*

      # If those files are not there, then copy them.
      cp /usr/lib/python2.7/site-packages/f5_neutron_lbaas_dashboard/enabled/_1491_project* /usr/share/openstack-dashboard/openstack_dashboard/local/enabled/

#. Configure Horizon virtual host.

   .. parsed-literal::

      sed -i '/  WSGIDaemonProcess/a\  WSGIApplicationGroup %{GLOBAL}'   /etc/httpd/conf.d/15-horizon_vhost.conf

#. Restart Horizon.


   .. parsed-literal::

      systemctl restart httpd.service

Next Steps
``````````

Use web browser to access Horizon GUI. The location of |dashboard-short| is 'Project'->'Network'->'F5 Load Balancers'.


Guides
------

See the `Configure SSL Offloading On F5 Neutron LBaaS Dashboard`_ documentation for more information.

Limitations
-----------

PKCS12 certificate bundle
`````````````````````````

The current version of |dashboard-short| can not support to import PKCS12 certificate bundle. If you need to use PKCS12 certificate bundle in your load balancer, please see :ref:`Configure terminated HTTPS load balancer with PKCS12 certificate bundle <p12-limitation>`

Server Name Indication (SNI)
````````````````````````````

If you need to create terminated HTTPS load balancer with SNI enabled, please see :ref:`Configure SNI load balancer <sni-limitation>`

.. |dashboard| replace:: f5-neutron-lbaas-dashboard
.. |dashboard-short| replace:: F5 Dashboard
.. |dashboard-long| replace:: F5 Dashboard for OpenStack Neutron LBaaS

.. _Configure SSL Offloading On F5 Neutron LBaaS Dashboard: https://clouddocs.f5.com/cloud/openstack/v1/lbaas/

.. |Build Status| image:: https://travis-ci.org/F5Networks/neutron-lbaas-dashboard.svg?branch=stable%2Fmitaka
   :target: https://travis-ci.org/F5Networks/neutron-lbaas-dashboard
   :alt: Travis-CI Build Status
