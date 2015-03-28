# About

This is our logstash setup to parse Zimbra logs.

## Kibana example


## Information about fields

### target
This field is used to identify accounts (mailboxes) upon wich an action is executed.

This a SASL example, where `target` is `contabilidad@example.com`

```log
Mar 26 21:38:26 mta-in-01 saslauthd[29157]: auth_zimbra: contabilidad@example.com auth failed: authentication failed for [contabilidad@maquinet.cl]
```

This a `audit.log` example, where `target` is `jbarrera@example.com`

```log
2015-03-26 21:51:36,505 INFO  [Pop3Server-13] [ip=172.16.0.3;oip=200.111.128.196;] security - cmd=Auth; account=jbarrera@example.com; protocol=pop3;
```


# TODO

Complete this README
