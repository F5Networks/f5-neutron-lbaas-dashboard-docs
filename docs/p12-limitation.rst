.. _p12-limitation:

Configure terminated HTTPS load balancer with PKCS12 certificate bundle
=======================================================================
The |dashboard-long| supports SSL offloading using PEM certificates. When you use the |dashboard-short| to create or update the certificate for a listener, you need to provide the following information:

- ``server certificate``,
- ``intermediate certificate``,
- ``private key`` ,
- ``passphrase``.

The |dashboard-short| does not support SSL offloading with PKCS12 certificate bundles. To use a PKCS12 certificate bundle, you need to manually extract the certificates.

1. Use ``openssl`` to extract the PKCS12 bundle.

   .. code-block:: bash

      # extract certificate without encrypt private key
      openssl pkcs12 -in certificate.p12 -out certificate.txt -nodes

      # extract certificate with encrypt private key
      openssl pkcs12 -in certificate.p12 -out certificate.txt

2. Verify that the extracted certificates are in PEM format.

   .. code-block:: bash

      # verify the certificate PEM format
      openssl x509 -in name.pem -noout -text

3. You can now use the extracted certificates to configure a load balancer with SSL offloading using the |dashboard-short|.

To import the SSL certification using the |dashboard-short|, take the following steps:

  * Paste the server certificate into the ``Certificate`` field.
  * Paste the intermediate certificate into the ``Certificate Chain`` field.
  * Paste the private key into the ``Private Key`` field.
  * Paste the private key passphrase into the ``Passphrase`` field, if you encrypt the private key when extracting PKCS12 file. Otherwise, leave the ``Passphrase`` field blank.
  * Click :guilabel:`Create` to create the SSL certificate.
