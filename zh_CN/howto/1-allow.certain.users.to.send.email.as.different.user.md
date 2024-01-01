# 允许特定的用户以其它用户发送邮件

iRedMail configures Postfix to reject the request when sender specifies an
owner for the MAIL FROM address (`From:` header), but the client is not (SASL)
logged in as that MAIL FROM address owner; or when the client is (SASL) logged
in, but the client login name doesn't own the MAIL FROM address.

有时候我们确实需要作为谋个人来发送邮件，接下来的指南将会说明使用iRedAPD plugin插件的`reject_sender_login_mismatch`.来实现这个功能。

* Remove restriction rule `reject_sender_login_mismatch` from Postfix
  parameter `smtpd_sender_restrictions` in file `/etc/postfix/main.cf`. Our iRedAPD
  plugin will do same restriction but more flexible.

    After removed `reject_sender_login_mismatch`, Postfix setting looks like
    below:

```
smtpd_sender_restrictions = permit_mynetworks, permit_sasl_authenticated
```

* Enable plugin `reject_sender_login_mismatch` in iRedAPD config file
  `/opt/iredapd/settings.py`:

```python
plugins = ['reject_sender_login_mismatch', ...]
```

* List senders who are allowed to send email as different users in iRedAPD
  config file `/opt/iredapd/settings.py`, in parameter
  `ALLOWED_LOGIN_MISMATCH_SENDERS`. For example:

```python
ALLOWED_LOGIN_MISMATCH_SENDERS = ['user1@here.com', 'user2@here.com']
```

    注意: 这个参数默认是不提供，请手动添加。


Restart iRedAPD service. That's all.
