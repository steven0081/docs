# Integrate OpenDMARC in iRedMail

[TOC]

!!! warning

    This is still a DRAFT document, do not apply it on production server.

## What are DMARC and OpenDMARC?

Quote from [DMARC.org](https://dmarc.org):

> DMARC, which stands for __Domain-based Message Authentication, Reporting
> & Conformance__, is an email authentication, policy, and reporting protocol.
> It builds on the widely deployed SPF and DKIM protocols, adding linkage to
> the author (`From:`) domain name, published policies for recipient handling
> of authentication failures, and reporting from receivers to senders, to
> improve and monitor protection of the domain from fraudulent email.

OpenDMARC is a free open source software implementation of the DMARC
specification. Source code hosted on [GitHub](https://github.com/trusteddomainproject/OpenDMARC).

## Requirements

* OpenDMARC-1.3.1 is buggy, only the latest OpenDMARC-1.3.2 is required.
* Supported OS Linux/BSD distributions:

Distribution | Releases | Comment
---|---|---
CentOS | 6, 7 | Yum repo `epel` is required.
Debian | 9, 10 |
Ubuntu | 18.04 | 16.04 ships OpenDMARC-1.3.1 which is buggy.
OpenBSD | | The latest 6.5 release doesn't offer opendmarc binary package.
FreeBSD | 11.x, 12.x | Port `mail/opendmarc`.

## Install OpenDMARC

## Configure OpenDMARC