.. _p12-limitation:

Configure terminated HTTPS load balancer with PKCS12 certificate bundle
=======================================================================
F5 Neutron Lbaas Dashboard support SSL offloading with PEM certificate.
When use F5 Neutron Lbaas Dashboard to create or update a certificate for listener, the ``server certificate``, ``intermediate certificate``, ``private key`` and ``passphrase`` need
to be filled into F5 Neutron Lbaas Dashboard.

F5 Neutron Lbaas Dashboard does not support load balancer SSL offloading function with PKCS 12 format certificates.
The PKCS 12 bundle must be extracted before using F5 Neutron Lbaas Dashboard.

1. To extract a PKCS 12 bundle via openssl tool.

.. code-block:: bash

  # extract certificate without encrypt private key
  openssl pkcs12 -in certificate.pfx -out certificate.txt -nodes

  # extract certificate with encrypt private key
  openssl pkcs12 -in certificate.pfx -out certificate.txt

2. Make sure the extracted certificates are PEM format by verifying each certificate.

.. code-block:: bash

  # verify the certificate PEM format
  openssl x509 -in name.pem -noout -text

3. Copy corresponding certificates and private key from extracted file to F5 Neutron Lbaas Dashboard.
4. If the PKCS 12 bundle is extracted with encrypt private key, the new passphrase also needs to be provide to F5 Neutron Lbaas Dashboard.

Example: Create A PKCS 12 Certificate
=====================================
.. code-block:: bash

   # cat certificate
   cat server.pem
   -----BEGIN CERTIFICATE-----
   MIICYzCCAcwCCQDVd/HiGcfo8DANBgkqhkiG9w0BAQsFADBwMQswCQYDVQQGEwJD
   TjELMAkGA1UECAwCQkoxCzAJBgNVBAcMAkJKMQwwCgYDVQQKDANYWVoxCzAJBgNV
   BAsMAklUMRAwDgYDVQQDDAd4eXouY29tMRowGAYJKoZIhvcNAQkBFgthc2tAeHl6
   LmNvbTAeFw0xODEwMTYwNDMyNDVaFw0xODExMTUwNDMyNDVaMHwxCzAJBgNVBAYT
   AkNOMQswCQYDVQQIDAJCSjELMAkGA1UEBwwCQkoxEDAOBgNVBAoMB0V4YW1wbGUx
   CzAJBgNVBAsMAklUMRQwEgYDVQQDDAtleGFtcGxlLmNvbTEeMBwGCSqGSIb3DQEJ
   ARYPYXNrQGV4YW1wbGUuY29tMIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDF
   JYrC2pg14dR3pwSxo8OpAHGNbQATXdSsSPGpo8OqvL6OSqTEb+0yO/eaRrl4i4N4
   zDLVM1Nrd8OqPdNnI57+S7QljGMHcWow4AFC24zY7kFNrFlWjutRAsq/oF1G0ALk
   IGlxF8viWMZ2Dq5HMnqbJqBz0tuHQ2JIqlO7B0+HrwIDAQABMA0GCSqGSIb3DQEB
   CwUAA4GBAEg1aNVexn4ghv0dEeyszh1PVK/khOPPRLIiPBrwRQYGUcDI9arAivsV
   cNAysWzSETBWWxRrI9JqxxSrhNghRQvLJFgTcBBuNwHIaqhq1sAbIdN3rxv22zVD
   P9ScjuBFg2xXkPBsOTJ7lPCslLJhcCJXBjNQcWDFAUev60QCJAZI
   -----END CERTIFICATE-----

   # cat intermediate certificate
   cat int-ca.pem
   -----BEGIN CERTIFICATE-----
   MIICbjCCAdegAwIBAgIJAMPIx273oielMA0GCSqGSIb3DQEBCwUAMHAxCzAJBgNV
   BAYTAkNOMQswCQYDVQQIDAJCSjELMAkGA1UEBwwCQkoxDDAKBgNVBAoMA0FCQzEL
   MAkGA1UECwwCSVQxEDAOBgNVBAMMB2FiYy5jb20xGjAYBgkqhkiG9w0BCQEWC2Fz
   a0BhYmMuY29tMB4XDTE4MTAxNjA0MzIyNFoXDTE4MTExNTA0MzIyNFowcDELMAkG
   A1UEBhMCQ04xCzAJBgNVBAgMAkJKMQswCQYDVQQHDAJCSjEMMAoGA1UECgwDWFla
   MQswCQYDVQQLDAJJVDEQMA4GA1UEAwwHeHl6LmNvbTEaMBgGCSqGSIb3DQEJARYL
   YXNrQHh5ei5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAOX8VIM9O4wK
   QT5KPcs0dVvCzJf944J3ssl+Gaa5WLyuRmYyme5YTST0CjWqDnN8ROR8OgNZi+qF
   TJHrfk1txlWh91SPV4msKh3c5ZJp9LT/kMGnlgd8blu6FGmFZkBK6tTx7gO5LPME
   ATvyKi1yYis0pup3AyCQgoJgjwPUWrjdAgMBAAGjEDAOMAwGA1UdEwQFMAMBAf8w
   DQYJKoZIhvcNAQELBQADgYEAFoTBXkSm9sT1Bd3kWudqYFSQCi+xAUZ2OvDwUnxO
   m3nO7+U9xX86ULBnYfmeT9ZohXQc44cG630ynBNOfT64/d5yJGbH7+ri/p9SkMXy
   BVbxQHUHJjA07oyw/gJaFcOkMiSYaRObALxvaXqKFLogS0vaYb+c7P2lfkEtjOvQ
   07Y=
   -----END CERTIFICATE-----

   # cat private Key
   cat server.key
   -----BEGIN RSA PRIVATE KEY-----
   MIICXAIBAAKBgQDFJYrC2pg14dR3pwSxo8OpAHGNbQATXdSsSPGpo8OqvL6OSqTE
   b+0yO/eaRrl4i4N4zDLVM1Nrd8OqPdNnI57+S7QljGMHcWow4AFC24zY7kFNrFlW
   jutRAsq/oF1G0ALkIGlxF8viWMZ2Dq5HMnqbJqBz0tuHQ2JIqlO7B0+HrwIDAQAB
   AoGAIfyA2WqZxuAxopb2ZjFXL7FV4g2ib7RDT5gboSUMPEjhiOIxWXP6LijMXJpI
   qxFSDucU9FAu114EKzsRULyBUgQBbpp8LtcaNGLlgmHr4L7aPlvH/SlhHxdH5tge
   U2hFhkLr5EDujTwQYcpz8cEnBqz/FtB93qR+BgRX7sjJmKECQQDh7vvn2o8PxMEW
   MKDvY3auZ/vCixe1FGwmy4nLLE5WKcWFXAf3oq4uRglTKQH1cD7udSKvQ8DPL4Wk
   WiHnlwS5AkEA32Hbj+VL2atW7ePorSoAzLgqThYB6bG5BymIw4lCpckFCU6YpTIV
   EGhGDrrgbUlD4h251SHgxnrxAzY1LpOLpwJBAIo14v3rkpan2yKS7vBinSiFzdot
   snwAmUSGQK38VZOaDA3PxcP0Ta9bArtPm7YkSyselvA2d02HGa73wEPm+2kCQCoj
   oLKtc7iVLOnlg4AfG1WDLF/coPG/2AK04Bra6tqxaCTQUdVf9D9LHGQs9qdHGeou
   516AbJGkoZCUikXGCaMCQDoqk+L9VlMiW522vfNwBUfiiRrhQDdUBq+kaEJlqRr/
   poZy3MRVLQomb7RSyJuXG43SazvEtbdhHt46QP6FHmk=
   -----END RSA PRIVATE KEY-----

   # bundle certificate, intermediate certificate, and private Key
   openssl pkcs12 -export -out certificate.pfx -inkey server.key  -in server.crt -certfile int-ca.crt
   Enter Export Password: your_password
   Verifying - Enter Export Password: your_password

   # a PKCS 12 ceritficate certificate.pfx is generated, this certificate is used in following examples.
   certificate.pfx

After the above process, a PKCS 12 ceritficate certificate.pfx is generated, this certificate is used in following examples.

Example: Extract A PKCS 12 Certificate without Encrypt Private Key
==================================================================

.. code-block:: bash

  # ===================== Output each certficate and private key separately ===================================

  # use openssl command to extract server certificate
  openssl pkcs12 -clcerts -nokeys -in certificate.pfx
  Enter Import Password: your_password
  MAC verified OK
  Bag Attributes
      localKeyID: 03 A7 84 BE 07 AF 22 08 E1 AD 2C 22 2F F9 16 28 D1 BE 90 F7
  subject=/C=CN/ST=BJ/L=BJ/O=Example/OU=IT/CN=example.com/emailAddress=ask@example.com
  issuer=/C=CN/ST=BJ/L=BJ/O=XYZ/OU=IT/CN=xyz.com/emailAddress=ask@xyz.com
  -----BEGIN CERTIFICATE-----
  MIICYzCCAcwCCQDVd/HiGcfo8DANBgkqhkiG9w0BAQsFADBwMQswCQYDVQQGEwJD
  TjELMAkGA1UECAwCQkoxCzAJBgNVBAcMAkJKMQwwCgYDVQQKDANYWVoxCzAJBgNV
  BAsMAklUMRAwDgYDVQQDDAd4eXouY29tMRowGAYJKoZIhvcNAQkBFgthc2tAeHl6
  LmNvbTAeFw0xODEwMTYwNDMyNDVaFw0xODExMTUwNDMyNDVaMHwxCzAJBgNVBAYT
  AkNOMQswCQYDVQQIDAJCSjELMAkGA1UEBwwCQkoxEDAOBgNVBAoMB0V4YW1wbGUx
  CzAJBgNVBAsMAklUMRQwEgYDVQQDDAtleGFtcGxlLmNvbTEeMBwGCSqGSIb3DQEJ
  ARYPYXNrQGV4YW1wbGUuY29tMIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDF
  JYrC2pg14dR3pwSxo8OpAHGNbQATXdSsSPGpo8OqvL6OSqTEb+0yO/eaRrl4i4N4
  zDLVM1Nrd8OqPdNnI57+S7QljGMHcWow4AFC24zY7kFNrFlWjutRAsq/oF1G0ALk
  IGlxF8viWMZ2Dq5HMnqbJqBz0tuHQ2JIqlO7B0+HrwIDAQABMA0GCSqGSIb3DQEB
  CwUAA4GBAEg1aNVexn4ghv0dEeyszh1PVK/khOPPRLIiPBrwRQYGUcDI9arAivsV
  cNAysWzSETBWWxRrI9JqxxSrhNghRQvLJFgTcBBuNwHIaqhq1sAbIdN3rxv22zVD
  P9ScjuBFg2xXkPBsOTJ7lPCslLJhcCJXBjNQcWDFAUev60QCJAZI
  -----END CERTIFICATE-----

  # use openssl command to extract intermediate certficate
  openssl pkcs12 -cacerts -nokeys -in certificate.pfx
  Enter Import Password: your_password
  MAC verified OK
  Bag Attributes: <No Attributes>
  subject=/C=CN/ST=BJ/L=BJ/O=XYZ/OU=IT/CN=xyz.com/emailAddress=ask@xyz.com
  issuer=/C=CN/ST=BJ/L=BJ/O=ABC/OU=IT/CN=abc.com/emailAddress=ask@abc.com
  -----BEGIN CERTIFICATE-----
  MIICbjCCAdegAwIBAgIJAMPIx273oielMA0GCSqGSIb3DQEBCwUAMHAxCzAJBgNV
  BAYTAkNOMQswCQYDVQQIDAJCSjELMAkGA1UEBwwCQkoxDDAKBgNVBAoMA0FCQzEL
  MAkGA1UECwwCSVQxEDAOBgNVBAMMB2FiYy5jb20xGjAYBgkqhkiG9w0BCQEWC2Fz
  a0BhYmMuY29tMB4XDTE4MTAxNjA0MzIyNFoXDTE4MTExNTA0MzIyNFowcDELMAkG
  A1UEBhMCQ04xCzAJBgNVBAgMAkJKMQswCQYDVQQHDAJCSjEMMAoGA1UECgwDWFla
  MQswCQYDVQQLDAJJVDEQMA4GA1UEAwwHeHl6LmNvbTEaMBgGCSqGSIb3DQEJARYL
  YXNrQHh5ei5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAOX8VIM9O4wK
  QT5KPcs0dVvCzJf944J3ssl+Gaa5WLyuRmYyme5YTST0CjWqDnN8ROR8OgNZi+qF
  TJHrfk1txlWh91SPV4msKh3c5ZJp9LT/kMGnlgd8blu6FGmFZkBK6tTx7gO5LPME
  ATvyKi1yYis0pup3AyCQgoJgjwPUWrjdAgMBAAGjEDAOMAwGA1UdEwQFMAMBAf8w
  DQYJKoZIhvcNAQELBQADgYEAFoTBXkSm9sT1Bd3kWudqYFSQCi+xAUZ2OvDwUnxO
  m3nO7+U9xX86ULBnYfmeT9ZohXQc44cG630ynBNOfT64/d5yJGbH7+ri/p9SkMXy
  BVbxQHUHJjA07oyw/gJaFcOkMiSYaRObALxvaXqKFLogS0vaYb+c7P2lfkEtjOvQ
  07Y=
  -----END CERTIFICATE-----

  # use openssl command to extract unencrypted private key
  openssl pkcs12 -nocerts -nodes -in certificate.pfx
  Enter Import Password: your_password
  MAC verified OK
  Bag Attributes
      localKeyID: 03 A7 84 BE 07 AF 22 08 E1 AD 2C 22 2F F9 16 28 D1 BE 90 F7
  Key Attributes: <No Attributes>
  -----BEGIN PRIVATE KEY-----
  MIICdgIBADANBgkqhkiG9w0BAQEFAASCAmAwggJcAgEAAoGBAMUlisLamDXh1Hen
  BLGjw6kAcY1tABNd1KxI8amjw6q8vo5KpMRv7TI795pGuXiLg3jMMtUzU2t3w6o9
  02cjnv5LtCWMYwdxajDgAULbjNjuQU2sWVaO61ECyr+gXUbQAuQgaXEXy+JYxnYO
  rkcyepsmoHPS24dDYkiqU7sHT4evAgMBAAECgYAh/IDZapnG4DGilvZmMVcvsVXi
  DaJvtENPmBuhJQw8SOGI4jFZc/ouKMxcmkirEVIO5xT0UC7XXgQrOxFQvIFSBAFu
  mnwu1xo0YuWCYevgvto+W8f9KWEfF0fm2B5TaEWGQuvkQO6NPBBhynPxwScGrP8W
  0H3epH4GBFfuyMmYoQJBAOHu++fajw/EwRYwoO9jdq5n+8KLF7UUbCbLicssTlYp
  xYVcB/eiri5GCVMpAfVwPu51Iq9DwM8vhaRaIeeXBLkCQQDfYduP5UvZq1bt4+it
  KgDMuCpOFgHpsbkHKYjDiUKlyQUJTpilMhUQaEYOuuBtSUPiHbnVIeDGevEDNjUu
  k4unAkEAijXi/euSlqfbIpLu8GKdKIXN2i2yfACZRIZArfxVk5oMDc/Fw/RNr1sC
  u0+btiRLKx6W8DZ3TYcZrvfAQ+b7aQJAKiOgsq1zuJUs6eWDgB8bVYMsX9yg8b/Y
  ArTgGtrq2rFoJNBR1V/0P0scZCz2p0cZ6i7nXoBskaShkJSKRcYJowJAOiqT4v1W
  UyJbnba983AFR+KJGuFAN1QGr6RoQmWpGv+mhnLcxFUtCiZvtFLIm5cbjdJrO8S1
  t2Ee3jpA/oUeaQ==
  -----END PRIVATE KEY-----

- Compare each output of ``openssl`` command to the original certificates ``server.pem`` and ``int-ca.pem``, the server and intermediate certificate can be easily found. The private key is regenerated when the PKCS 12 certificate is extracted.
- When create SSL certificate:

  * Fill the server certificate into the ``Certificate`` box.
  * Fill the intermediate certificate into the ''Certificate Chain'' box.
  * Fill the private key into the ``Private Key`` box.
  * The private key is not encrypted, when the PKCS 12 is extracted, thus, leave the ``Passphrase`` blank.
  * Click ``Create`` button to create a new SSL certificate for a listener.

Example: Extract A PKCS 12 Certificate with Encrypt Private Key
===============================================================
.. code-block:: bash

  # ===================== Output each certficate and private key separately ===================================

  # use openssl command to extract server certificate
  openssl pkcs12 -clcerts -nokeys -in certificate.pfx
  Enter Import Password: your_password
  MAC verified OK
  Bag Attributes
      localKeyID: 03 A7 84 BE 07 AF 22 08 E1 AD 2C 22 2F F9 16 28 D1 BE 90 F7
  subject=/C=CN/ST=BJ/L=BJ/O=Example/OU=IT/CN=example.com/emailAddress=ask@example.com
  issuer=/C=CN/ST=BJ/L=BJ/O=XYZ/OU=IT/CN=xyz.com/emailAddress=ask@xyz.com
  -----BEGIN CERTIFICATE-----
  MIICYzCCAcwCCQDVd/HiGcfo8DANBgkqhkiG9w0BAQsFADBwMQswCQYDVQQGEwJD
  TjELMAkGA1UECAwCQkoxCzAJBgNVBAcMAkJKMQwwCgYDVQQKDANYWVoxCzAJBgNV
  BAsMAklUMRAwDgYDVQQDDAd4eXouY29tMRowGAYJKoZIhvcNAQkBFgthc2tAeHl6
  LmNvbTAeFw0xODEwMTYwNDMyNDVaFw0xODExMTUwNDMyNDVaMHwxCzAJBgNVBAYT
  AkNOMQswCQYDVQQIDAJCSjELMAkGA1UEBwwCQkoxEDAOBgNVBAoMB0V4YW1wbGUx
  CzAJBgNVBAsMAklUMRQwEgYDVQQDDAtleGFtcGxlLmNvbTEeMBwGCSqGSIb3DQEJ
  ARYPYXNrQGV4YW1wbGUuY29tMIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDF
  JYrC2pg14dR3pwSxo8OpAHGNbQATXdSsSPGpo8OqvL6OSqTEb+0yO/eaRrl4i4N4
  zDLVM1Nrd8OqPdNnI57+S7QljGMHcWow4AFC24zY7kFNrFlWjutRAsq/oF1G0ALk
  IGlxF8viWMZ2Dq5HMnqbJqBz0tuHQ2JIqlO7B0+HrwIDAQABMA0GCSqGSIb3DQEB
  CwUAA4GBAEg1aNVexn4ghv0dEeyszh1PVK/khOPPRLIiPBrwRQYGUcDI9arAivsV
  cNAysWzSETBWWxRrI9JqxxSrhNghRQvLJFgTcBBuNwHIaqhq1sAbIdN3rxv22zVD
  P9ScjuBFg2xXkPBsOTJ7lPCslLJhcCJXBjNQcWDFAUev60QCJAZI
  -----END CERTIFICATE-----

  # use openssl command to extract intermediate certficate
  openssl pkcs12 -cacerts -nokeys -in certificate.pfx
  Enter Import Password: your_password
  MAC verified OK
  Bag Attributes: <No Attributes>
  subject=/C=CN/ST=BJ/L=BJ/O=XYZ/OU=IT/CN=xyz.com/emailAddress=ask@xyz.com
  issuer=/C=CN/ST=BJ/L=BJ/O=ABC/OU=IT/CN=abc.com/emailAddress=ask@abc.com
  -----BEGIN CERTIFICATE-----
  MIICbjCCAdegAwIBAgIJAMPIx273oielMA0GCSqGSIb3DQEBCwUAMHAxCzAJBgNV
  BAYTAkNOMQswCQYDVQQIDAJCSjELMAkGA1UEBwwCQkoxDDAKBgNVBAoMA0FCQzEL
  MAkGA1UECwwCSVQxEDAOBgNVBAMMB2FiYy5jb20xGjAYBgkqhkiG9w0BCQEWC2Fz
  a0BhYmMuY29tMB4XDTE4MTAxNjA0MzIyNFoXDTE4MTExNTA0MzIyNFowcDELMAkG
  A1UEBhMCQ04xCzAJBgNVBAgMAkJKMQswCQYDVQQHDAJCSjEMMAoGA1UECgwDWFla
  MQswCQYDVQQLDAJJVDEQMA4GA1UEAwwHeHl6LmNvbTEaMBgGCSqGSIb3DQEJARYL
  YXNrQHh5ei5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAOX8VIM9O4wK
  QT5KPcs0dVvCzJf944J3ssl+Gaa5WLyuRmYyme5YTST0CjWqDnN8ROR8OgNZi+qF
  TJHrfk1txlWh91SPV4msKh3c5ZJp9LT/kMGnlgd8blu6FGmFZkBK6tTx7gO5LPME
  ATvyKi1yYis0pup3AyCQgoJgjwPUWrjdAgMBAAGjEDAOMAwGA1UdEwQFMAMBAf8w
  DQYJKoZIhvcNAQELBQADgYEAFoTBXkSm9sT1Bd3kWudqYFSQCi+xAUZ2OvDwUnxO
  m3nO7+U9xX86ULBnYfmeT9ZohXQc44cG630ynBNOfT64/d5yJGbH7+ri/p9SkMXy
  BVbxQHUHJjA07oyw/gJaFcOkMiSYaRObALxvaXqKFLogS0vaYb+c7P2lfkEtjOvQ
  07Y=
  -----END CERTIFICATE-----

  # use openssl command to extract encrypted private key
  openssl pkcs12 -nocerts -in certificate.pfx
  Enter Import Password: your_password
  MAC verified OK
  Bag Attributes
      localKeyID: 03 A7 84 BE 07 AF 22 08 E1 AD 2C 22 2F F9 16 28 D1 BE 90 F7
  Key Attributes: <No Attributes>
  Enter PEM pass phrase: new_passpharse_for_private_key
  Verifying - Enter PEM pass phrase: new_passpharse_for_private_key
  -----BEGIN ENCRYPTED PRIVATE KEY-----
  MIICxjBABgkqhkiG9w0BBQ0wMzAbBgkqhkiG9w0BBQwwDgQIZZIffSZPQ1wCAggA
  MBQGCCqGSIb3DQMHBAjlq867MBx9nwSCAoCSSkJVhEJjSb74TgzbbFQGmd2dq9Gh
  YNzSQS7wxak1XTvJKjsIIXeyGBwcbTUSqwgQYd5L2r7ePYesm9rH70ZwUlRRRpCv
  rLERz2Jqmlns4q2efU2KdlYLBbTuRqdago0TGAhDLoHaE333ZBAtU009qudfNcHJ
  ZbsHwheDBA4KXIOEqpIpMYdRMgy71hj/vEN5kJ6WJylxEZ5vSZwJd6JjQUP0p9h8
  EoV/ta1m7TjHc6U5VPQoLlQhcCnwn4iZR60ahyMpLEDmHgQhBT7KbEJdTbYY7QfD
  U5KoBdH/3wiFGVfUivNHUjkktBahDlt5Ue3WCOx3LASD0Ht9+XADvRabzU2yFtRg
  3BQM6pE8gcs3uAdHu3nnrwKvNVickkS2msBDizV3XTZ4EHV8qXUFZ4JUAlD92G0U
  hkrs6GZW0uxdTZjFDtezJwI07D6dpf90qha3dW66ooVGNEbXb18giIK9ULbtE19s
  T2rv8lwVRZPw0vpR+n6m4v/8jqyj8ziXyqbMp7DoFldSQHfSD5Waaxy0zV8bYyNq
  tNXzO3DUVvrwMYsBlsBXIZlUYuhNzmZgzRJ7LyxECie5bRQSrrtSJKZOPbHgPlr
  YMsEeDY1Rj62RfQ5mLZ9DPbDSnLJXX2cXOG+1BMn2Q3n1xvbzWy6J12/hIDYVRIe
  lBAgIr5gecvKqNjc/sELG16J7dxytch399b01cOF5ehRljVs9KFV/fAhJ9cGdhQp
  26RsguwZG7aABRjjPjciBNK30ypm7ksKQHQv08yCkiYW0OfkccwvGaEG0cZhEg35
  tWsKQiEV+c1duKI1sDlPRlOhoQICJMj6EZhGHT+TPBnR+IJNCco0SpQR
  -----END ENCRYPTED PRIVATE KEY-----

- Compare each output of ``openssl`` command to the original certificates ``server.pem`` and ``int-ca.pem``, the server and intermediate certificate can be easily found. The private key is regenerated and encrypted with new passpharse when the PKCS 12 certificate is extracted.
- When create SSL certificate:

  * Fill the server certificate into the ``Certificate`` box.
  * Fill the intermediate certificate into the ''Certificate Chain'' box.
  * Fill the private key into the ``Private Key`` box.
  * The private key is encrypted with new passphrase, when the PKCS 12 is extracted, thus, fill the new passphrase into ``Passphrase`` box.
  * Click ``Create`` button to create a new SSL certificate for a listener.
