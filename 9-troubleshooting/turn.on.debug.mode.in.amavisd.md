# How to turn on debug mode in Amavisd

In Amavisd config file `/etc/amavisd/amavisd.conf`, change `$log_level`, then restart amavis service.

```
$log_level = 5;              # verbosity 0..5, -d
```

If you want to debug SpamAssassin, please update `$sa_debug` also:

```
$sa_debug = 1;
```