# About

This is our logstash setup to parse Zimbra logs.

## Filters
Our setup is divided in several filters files, each one working with an specific kind of log type.

### 10-filter-postfix, 15-filter-sasl and 16-filter-amavis
Correspond to the `postfix|amavis|sasl` information recorded in the `/var/log/zimbra.log`.

#### Fields

* `component`, postfix dameon (`smtp`, `smtpd`, etc),
* `dst_(relayhost|relayip)`, destination host,
* `from`, `from_domain`, email address and domain from the sender,
* `maild_id`, amavis mail_id
* `message_id`, the email [message id](http://en.wikipedia.org/wiki/Message-ID),
* `provider`, the auth mech used by `saslauthd`,
* `queued_as`, amavis record after re-injecting the email to postfix,
* `queue_status`, postfix queue status,
* `result`, result of smtp when delivering the message,
* `size`, size of messages,
* `src_(relayhost|relayip)`, source host from where the message comes,
* `status`, amavis filtering result,
* `username`, user authenticated by `saslauthd`,
* `to`, `to_domain`, email address and domain of the recipient,


### 17.filter-nginx
Correspond to the logs of `zimbra-proxy`

#### Fields

* `agent`, the cliente software contacting the server,
* `username`, the authenticated user,
* `clientip`, the IP Address of the client,
* `httpversion`,
* `referrer`, the URI used for connecting,
* `request`, the HTTP request made,
* `http_code`, the [HTTP response code](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes),
* `verb`, the HTTP [REST verb](http://en.wikipedia.org/wiki/Representational_state_transfer)


### 18.filter-zimbra
Correspond to the logs of the Zimbra server for the `/opt/zimbra/audit.log` file.

#### Fields

* `command`, the acction executed like `Auth` or `ModifyServer` among others,
* `level`, the log level: `INFO | WARN`
* `clienteip`, the IP Address of the client,
* `process`, the server or process running the request, like `Pop3Server | ImapServer`,
* `protocol`, the protocol used, like `pop3 | imap`,
* `target_type`, the object type upon wich the `command` is running, can be a mailbox, server, etc
* `username`, the object name


## Example with Kibana 4
We try to use moslty `filters`when we can, because this are fasters than `queries` as you can read it [Queries And Filters](http://www.elastic.co/guide/en/elasticsearch/guide/current/_queries_and_filters.html).


### Zimbra logs Dashboard
In this Dashboard we show information mostly getted from the `17-filter-nginx` and `18-filter-zimbra`.

!(Kibana Dashboard)[https://raw.githubusercontent.com/ITLinuxCL/zimbra_logstash/master/examples/dashboard.png]

### SASL Failed Login query source code

```json
{
  "index": "logstash-*",
  "query": {
    "query_string": {
      "query": "host:*.zbox*",
      "analyze_wildcard": true
    }
  },
  "highlight": {
    "pre_tags": [
      "@kibana-highlighted-field@"
    ],
    "post_tags": [
      "@/kibana-highlighted-field@"
    ],
    "fields": {
      "*": {}
    }
  },
  "filter": [
    {
      "meta": {
        "disabled": false,
        "index": "logstash-*",
        "key": "result",
        "negate": false,
        "value": "failed"
      },
      "query": {
        "match": {
          "result": {
            "query": "failed",
            "type": "phrase"
          }
        }
      }
    },
    {
      "meta": {
        "disabled": false,
        "index": "logstash-*",
        "key": "tags",
        "negate": false,
        "value": "sasld"
      },
      "query": {
        "match": {
          "tags": {
            "query": "sasld",
            "type": "phrase"
          }
        }
      }
    },
    {
      "meta": {
        "index": "logstash-*",
        "negate": false,
        "key": "tags",
        "value": "result",
        "disabled": false
      },
      "query": {
        "match": {
          "tags": {
            "query": "result",
            "type": "phrase"
          }
        }
      }
    }
  ]
}
```


# TODO

Complete this README
