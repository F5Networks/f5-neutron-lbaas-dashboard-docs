F5 Dashboard for OpenStack Neutron LBaaS
========================================

.. sidebar:: **OpenStack version:**

   |dashboard-long| supports the OpenStack Mitaka release.

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

|dashboard-long| (``f5-neutron-lbaas-dashboard``) is an OpenStack `Horizon dashboard plugin <https://docs.openstack.org/security-guide/networking/architecture.html>`_.
It works in conjunction with the `F5 Driver for OpenStack Neutron LBaaS <https://clouddocs.f5.com/products/openstack/lbaasv2-driver/mitaka/>`_ and `F5 Agent for OpenStack Neutron LBaaS <https://clouddocs.f5.com/products/openstack/agent/latest/>`_ to manage F5 BIG-IP `Local Traffic Manager <https://f5.com/products/big-ip/local-traffic-manager-ltm>`_ (LTM) services via the OpenStack Neutron API.

.. seealso::

   For more information about how |dashboard-long| interacts with the Neutron API and BIG-IP devices, see `Configure SSL Offloading On F5 Neutron LBaaS Dashboard`_.

Downloads
---------

|rpm-download|

Installation
------------

Follow the instructions for your distribution to install |dashboard-long| on your Horizon controller.

.. note::

   This document assumes that you already have the RDO Mitaka release installed as your OpenStack control plane, with Horizon and Barbican enabled.

RPM
```

#. Download and install the |dashboard|.

   .. parsed-literal::

      # curl -L -O |f5_dashboard_rpm_url|
      # rpm -ivh |f5_dashboard_rpm_package|

#. Verify that the install script successfully enabled |dashboard-long|. If the dashboard is not enabled, manually copy it to your Horizon 'enabled' directory.

   .. code-block:: bash
      :emphasize-lines: 2,3,4

      # ls /usr/share/openstack-dashboard/openstack_dashboard/local/enabled/_1491_project*
      /usr/share/openstack-dashboard/openstack_dashboard/local/enabled/_1491_project_ng_loadbalancersv2_panel.py
      /usr/share/openstack-dashboard/openstack_dashboard/local/enabled/_1491_project_ng_loadbalancersv2_panel.pyc
      /usr/share/openstack-dashboard/openstack_dashboard/local/enabled/_1491_project_ng_loadbalancersv2_panel.pyo

   If the files aren't present, copy them into the Horizon "enabled" directory.

   .. code-block:: bash

      # cp /usr/lib/python2.7/site-packages/f5_neutron_lbaas_dashboard/enabled/_1491_project* /usr/share/openstack-dashboard/openstack_dashboard/local/enabled/

#. Configure the Horizon virtual host.

   .. code-block:: bash

      # sed -i '/  WSGIDaemonProcess/a\  WSGIApplicationGroup %{GLOBAL}'   /etc/httpd/conf.d/15-horizon_vhost.conf

#. Restart Horizon.

   .. code-block:: bash

      # systemctl restart httpd.service

Usage 
-----

#. Open the Horizon GUI in your web browser.

#. Go to :menuselection:`Project --> Network --> F5 Load Balancers` to access the F5 LBaaS dashboard. 

#. `Configure SSL Offloading On F5 Neutron LBaaS Dashboard`_.

#. `Configure Enhanced Service Definition on F5 Neutron LBaaS Dashboard`_.

.. caution::

   A `known issue <https://bugs.launchpad.net/neutron/+bug/1720313>`_ of OpenStack Neutron LBaaS may occur, when a user attempts to update a listener with TERMINATED_HTTPS protocol. OpenStack community delivers a patch, `https://review.openstack.org/#/c/518455 <https://review.openstack.org/#/c/518455/>`_, to fix this issue for Queens and higher release version. If you encounter this issue, please contact with your OpenStack verndor to backport the patch to your OpenStack distribution.

Guides
------

See the `F5 Integrations for OpenStack Neutron LBaaS`_ documentation for more information about managing BIG-IP devices from OpenStack.

.. _F5 Integrations for OpenStack Neutron LBaaS: http://clouddocs.f5.com/cloud/openstack/v1/lbaas/
.. _Configure SSL Offloading On F5 Neutron LBaaS Dashboard: https://clouddocs.f5.com/cloud/openstack/v1/lbaas/ssl-offloading-configuration.html
.. _Configure Enhanced Service Definition on F5 Neutron LBaaS Dashboard: https://clouddocs.f5.com/cloud/openstack/v1/lbaas/enhanced-service-definitions.html

.. |Build Status| image:: https://travis-ci.org/F5Networks/neutron-lbaas-dashboard.svg?branch=stable%2Fmitaka
   :target: https://travis-ci.org/F5Networks/neutron-lbaas-dashboard
   :alt: Travis-CI Build Status
