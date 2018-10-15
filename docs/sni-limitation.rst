.. _sni-limitation:

Configure SNI TLS Load Balancer
===============================

Background
----------

Creating a new *TERMINATED_HTTPS* load balancer(LB for short) with new certificate(s) on OpenStack dashboard will trigger a new virtual server(VS for short) with new SSL profile(s) to be created on BIGIP side.

It is possible to reuse existing certificates for creating new LB, which will trigger on BIGIP side, reusing existing SSL profiles for creating new VS.

It can be summarized as:

* Relation between certificate and SSL profile is one-to-one.
* Certificate can be used by multiple LBs just as SSL profile can be used by multiple VSs on BIGIP side.


SNI TLS Load Balancer
---------------------

The LB using multiple certificates called **SNI TLS Load Balancer**. Correspondingly, the triggerred VS has multiple SSL profiles.

Among SSL profiles of a specific LB, only one of them is set with attribute "*Default SSL Profile for SNI: Enabled*".

So, **Using two or more SSL profiles of "Default SSL Profile for SNI: Enabled" will cause VS creation failure.** That is the limitation of using multiple SSL profiles.

But unfortunately, we have no way on OpenStack dashboard to justify wether or not a certificate has been created as SSL profile of "*Default SSL Profile for SNI: Enabled*" on BIGIP side unless we

* On BIGIP side, check manually that SSL profile's attribute, or
* Name the certificate with special hint(i.e. suffix with _sni_default).

Well, they are 2 ways for solving the issue. But a better recommended way is

* **Create exclusive certificates for the SNI TLS Load Balancer**, which means, don't share the certificate with other LBs if you plan to use them to create a SNI TLS Load Balancer.
