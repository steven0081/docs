# Install iRedMail on OpenBSD

[TOC]

## System Requirements

> __WARNING__: iRedMail is designed to be deployed on a FRESH server system,
> which means your server does __NOT__ have mail related components installed,
> e.g. MySQL, OpenLDAP, Postfix, Dovecot, Amavisd, etc. iRedMail will install
> and configure them for you automatically. Otherwise it may override your
> existing files/configurations althought it will backup files before modifing,
> and it may be not working as expected.

To install iRedMail on OpenBSD, you need:

* A FRESH, working OpenBSD system. Supported releases are listed on
  [Download](../download.html) page.
* At least `1 GB` of memory is required for low traffic production server.
  Spam/Virus scanning will take most system resource.
* Required OpenBSD installation file sets are (replace `[XX]` by the real
  OpenBSD release number):

    * base[XX].tgz
    * etc[XX].tgz
    * comp[XX].tgz
    * man[XX].tgz
    * xbase[XX].tgz

Notes:

* All binary packages will be installed with command `pkg_add -i`.
* Apache chroot is disabled by default, required by iRedAdmin - the web-based
  admin panel.
* PF is enabled by default, with basic rules for ssh and mail services.
* System built-in [`spamd(8)`](http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man8/spamd.8) is enabled by default for greylisting,
  whitelisting, blacklisting.
* Sendmail is disabled by default, replaced by Postfix.

## Preparations

### Set a fully qualified domain name (FQDN) hostname on your server

No matter your server is a testing machine or production server, it's strongly
recommended to set a fully qualified domain name (FQDN) hostname.

Enter command `hostname -f` to view the current hostname:

```shell
$ hostname -f
mx.example.com
```

On OpenBSD, hostname is set in two files: `/etc/myname` and `/etc/hosts`.

* `/etc/myname`: FQDN.

```
mx.example.com
```

* `/etc/hosts`: static table lookup for hostnames. __Warning__: Please list the
  FQDN hostname as first item.

```
# Part of file: /etc/hosts
127.0.0.1   mx.example.com mx localhost localhost.localdomain
```

Verify the FQDN hostname. If it wasn't changed after updating above two files,
please reboot server to make it work.

```
$ hostname -f
mx.example.com
```

### Choose a nearest mirror site for installing binary packages

iRedMail will install all required binary packages with command `pkg_add -i`,
it will check whether you have mirror site defined in `PKG_PATH` environment
variable, if defined, `pkg_add` will install packages from defined mirror site.

It's recommended to install packages from a mirror site, to reduce server
load on OpenBSD primary servers. Also, installing package from a nearest
mirror site will speed up package installation. You can find mirror list
near you on OpenBSD web site:
[Getting OpenBSD](http://www.openbsd.org/ftp.html#http).

Now login to the OpenBSD server as root user, set variable `PKG_PATH` in file
`/root/.profile` like below (use your nearest mirror site instead):

```
export PKG_PATH="http://ftp.jaist.ac.jp/pub/OpenBSD/`uname -r`/packages/`machine -a`/"
```

Install Bash shell, it's required by iRedMail.

```
# . /root/.profile    # <- This steps is required, used to set PKG_PATH without re-login.
# pkg_add bash
```

### Download the latest release of iRedMail

* Visit [Download page](../download.html) to get the
  latest stable release of iRedMail.

* Upload iRedMail to your mail server via ftp or scp or whatever method you
  can use, login to the server to install iRedMail. We assume you uploaded
  it to `/root/iRedMail-x.y.z.tar.bz2` (replace x.y.z by the real version number).

* Uncompress iRedMail tarball:

```
# cd /root/
# tar xjf iRedMail-x.y.z.tar.bz2
```

## Start iRedMail installer

> For Chinese users: Our domain name "iredmail.org" is blocked in China mainland since Jun 04, 2011, please replace all 'iredmail.org' by its IP address "106.187.51.47" (without quotes) in /root/iRedMail-x.y.z/pkgs/get_all.sh BEFORE executing "iRedMail.sh". This is a Linode VPS hosted in Tokyo, Japan.

It's now ready to start iRedMail installer, it will ask you several simple
questions, that's all steps to setup a full-featured mail server.

```
# cd /root/iRedMail-x.y.z/
# bash iRedMail.sh
```

## Screenshots of installation:

* Welcome and thanks for your use

![](../images/installation/iredmail/welcome.png)

* Specify location to store all mailboxes. Default is `/var/vmail/`.

![](../images/installation/iredmail/location_to_store_mailboxes.png)

* Choose backend used to store mail accounts. You can manage mail accounts
with iRedAdmin, our web-based iRedMail admin panel.

__IMPORTANT NOTE__: There's no big difference between available backends, so
it's strongly recommended to choose the one you're familiar with for easier
management and maintenance after installation.

![](../images/installation/iredmail/backend.png)

* If you choose to store mail accounts in OpenLDAP, iRedMail installer will
ask you two questions about OpenLDAP.

LDAP suffix.

![](../images/installation/iredmail/ldap_suffix.png)

Password of LDAP root dn.

![](../images/installation/iredmail/pw_of_ldap_root_dn.png)

* Set password of MySQL or PostgreSQL admin user.

__NOTE__: MySQL is used to store data of other applications (e.g. Roundcube
webmail, Cluebringer, Amavisd-new) if you choose OpenLDAP or MySQL as backend.

![](../images/installation/iredmail/pw_of_mysql_root_user.png)

* Add your first mail domain name

![](../images/installation/iredmail/first_mail_domain.png)

* Set password of admin account of your first mail domain.

__Note__: This account is an admin account and a mail user. That means you can
login to webmail and admin panel (iRedAdmin) with this account, login username
is full email address.

![](../images/installation/iredmail/pw_of_domain_admin.png)

* Choose optional components

![](../images/installation/iredmail/optional_components.png)


After answered above questions, iRedMail installer will ask your confirm to
start installation. It will install and configure required packages
automatically. Type `y` or `Y` and press `Enter` to start.

```
Configuration completed.

*************************************************************************
**************************** WARNING ***********************************
*************************************************************************
*                                                                       *
* Please do remember to *REMOVE* configuration file after installation  *
* completed successfully.                                               *
*                                                                       *
*   * /root/iRedMail-x.y.z/config
*                                                                       *
*************************************************************************
<<< iRedMail >>> Continue? [Y|n]        # <- Type 'Y' or 'y' here, and press 'Enter' to continue
```

## Important things you __MUST__ know after installation

* Read file `/root/iRedMail-x.y.z/iRedMail.tips` first, it contains:

    * URLs, usernames and passwords of web-based applications
    * Location of mail serve related software configuration files
    * Some other important and sensitive information

* [Setup DNS records for your mail server](./setup_dns.html)
* [Configure Thunderbird as mail client (POP3/IMAP, SMTP and global ldap address book)](./configure.thunderbird.html)

## Access webmail and other web applications

After installation successfully completed, you can access web-based programs
if you choose to install them. Replace `your_server` below by your real server
hostname or IP address.

* __Roundcube webmail__: [https://your_server/mail/](https://your_server/mail/)
* __Web admin panel (iRedAdmin)__: [httpS://your_server/iredadmin/](httpS://your_server/iredadmin/)
* __phpLDAPadmin__ (available if you choose LDAP backend): [httpS://your_server/phpldapadmin/](httpS://your_server/phpldapadmin/)
* __Awstats__: [httpS://your_server/awstats/awstats.pl?config=web](httpS://your_server/awstats/awstats.pl?config=web) (or ?config=smtp)

## Get technical support

Please post all issues, feedbacks, feature requests, suggestions in our [online
support forum](http://www.iredmail.org/forum/), it's more responsible than you
expected.

## Notes about binary packages provided by iRedMail project

Most binary packages in iRedMail yum repository comes from below repositories,
packages with `-ired` flag were packed by iRedMail project.

* [Dag Wieers](http://packages.sw.be/)
* [EPEL](http://download.fedora.redhat.com/pub/epel/)
* [ATrpms](http://atrpms.net/)
