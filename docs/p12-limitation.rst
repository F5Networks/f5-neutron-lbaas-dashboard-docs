.. _p12-limitation:

Configure a Terminated HTTPS Load Balancer with a PKCS12 Certificate Bundle
===========================================================================

|dashboard-long| supports SSL offloading using PEM certificates. When you use |dashboard-long| to create or update the certificate for a listener, you need to provide the following information:

- ``server certificate``,
- ``intermediate certificate``,
- ``private key`` ,
- ``passphrase``.

|dashboard-long| does not support SSL offloading with PKCS12 certificate bundles. To use a PKCS12 certificate bundle, you need to manually extract the certificates.

Extract the PKCS12 bundle
``````````````````````````

You can use ``openssl`` to extract the certificates.

Extract certificates without an encrypted private key:

.. code-block:: bash

   $ openssl pkcs12 -in certificate.p12 -out certificate.txt -nodes

Extract certificates with an encrypted private key:

.. code-block:: bash
   
   $ openssl pkcs12 -in certificate.p12 -out certificate.txt

Verify that the extracted certificates are in PEM format:

.. code-block:: bash

   openssl x509 -in name.pem -noout -text

Configure a Load Balancer
`````````````````````````

You can now use the extracted certificates to configure a load balancer with SSL offloading using |dashboard-long|.

To import the SSL certification using |dashboard-long|, take the following steps:

* Paste the server certificate into the :guilabel:`Certificate` field.
* Paste the intermediate certificate into the :guilabel:`Certificate Chain` field.
* Paste the private key into the :guilabel:`Private Key` field.
* Paste the private key passphrase into the :guilabel:`Passphrase` field. 

  .. note:: 

     The :guilabel:`Passphrase` field is only required if you encrypted the private key when extracting the PKCS12 files. 

* Click :guilabel:`Create` to create the SSL certificate.
