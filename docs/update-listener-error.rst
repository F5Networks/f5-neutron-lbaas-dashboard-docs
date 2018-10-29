.. _update-listener-error:
  
Update the listener of a Terminated HTTPS Load Balancer
=======================================================

A `known issue <https://bugs.launchpad.net/neutron/+bug/1720313>`_ of OpenStack Neutron LBaaS may occur when a user attempts to update the listener of an existing terminated HTTPS load balancer. OpenStack community delivers a patch, `https://review.openstack.org/#/c/518455 <https://review.openstack.org/#/c/518455/>`_, to fix this issue for Queens and higher release version. If you encounter this issue, please contact with your OpenStack verndor to backport the patch to your OpenStack distribution.
