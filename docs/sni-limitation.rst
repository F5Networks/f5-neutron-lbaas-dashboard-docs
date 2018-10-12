.. _sni-limitation:

Configure SNI TLS Load Balancer
===============================

Background
----------

When creating a new *TERMINATED_HTTPS* load balancer(LB for short) with new certificate(s) on OpenStack dashboard, it will trigger a new virtual server(VS for short) with new SSL profile(s) to be created on BIGIP side.

It is possible to reuse existing certificates for creating LB, which triggers on BIGIP side, reusing existing SSL profiles for creating VS.

It can be summarized as:

* Relation between certificate and SSL profile is one-to-one.
* Certificate can be used by multiple LBs just as on BIGIP side, SSL profile can be used by multiple VSs.


SNI TLS Load Balancer
---------------------

The LB is called **SNI TLS Load Balancer** if multiple certificates are used.

Among SSL profiles of a specific LB, only one of them is set with attribute "*Default SSL Profile for SNI: Enabled*".

So, **Using two or more SSL profiles of "Default SSL Profile for SNI: Enabled" will cause failure of VS creation.** That is the limitation want to deliever.

But unfortunately, we have no way on OpenStack dashboard to justify wether or not a certificate has been created as SSL profile of "*Default SSL Profile for SNI: Enabled*" on BIGIP side unless we

* On BIGIP UI, check the SSL profile's attribute, or
* Name the certificate with special hint(i.e. suffix with _sni_default).

Well, they are 2 ways for solving the issue. But a better recommended way for this is

* **Create exclusive certificates for your SNI TLS load balancer**: Don't share the certificate with other load balancers if you plan to use it to create a LB.
