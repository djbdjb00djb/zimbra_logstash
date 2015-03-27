# About

This is our logstash setup to parse Zimbra logs.

## Kibana example

![Screenshoot](https://photos-6.dropbox.com/t/2/AAA1hyTJcBbBOLazDfAYJmNMVDhLUMDSltuD3-5ycWq77g/12/22991380/png/1024x768/3/1425686400/0/2/kibana_zbox_1.png/CJSk-wogASACIAMoASgCKAM/WnDy-lsVqTKLEaWa2p8Ku0kQ6_GBk5KDlpExXbxKnBQ) ![Screenshoot](https://photos-3.dropbox.com/t/2/AAA_nbucPx1ToqOQf8htK55GdfQiQxIqxAIvPKLtXIQGXg/12/22991380/png/1024x768/3/1425686400/0/2/kibana_zbox_2.png/CJSk-wogASACIAMoASgCKAM/ZtU5idOS-8TW828M6l30ntVF7zAGPd2lJELmIwq7LZU)


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
