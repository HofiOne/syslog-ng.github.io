---
title: riemann() destination options
srv: Riemann server
port: '5555'
id: adm-dest-riemann-opt
description: >-
    This section describes the options of the riemann() destination in {{ site.product.short_name }}.
---

The riemann() destination has the following options:

## attributes()

|  Type:|      parameter list of the value-pairs() option|
  |Default:|   |

*Description:* The attributes() option adds extra metadata to the
Riemann event, that can be displayed on the Riemann dashboard. To
specify the metadata to add, use the syntax of the value-pairs() option.
For details on using value-pairs(), see
Structuring macros, metadata, and other value-pairs.

{% include doc/admin-guide/options/batch-bytes.md %}

{% include doc/admin-guide/options/batch-lines.md %}

{% include doc/admin-guide/options/batch-timeout.md %}

## description()

|  Type:|      template, macro, or string|
  |Default:|   |

*Description:* The value to add as the description field of the Riemann
event.

{% include doc/admin-guide/options/disk-buffer.md %}

## event-time()

|  Type:|      template, macro, or string|
  |Default:|   ${UNIXTIME}|

*Description:* Instead of the arrival time into Riemann, {{ site.product.short_name }}
can also send its own timestamp value.

This can be useful if Riemann is inaccessible for a while, and the
messages are collected in the disk buffer until Riemann is accessible
again. In this case, it would be difficult to differentiate between
messages based on the arrival time only, because this would mean that
there would be hundreds of messages with the same arrival time. This
issue can be solved by using this option.

The event-time() option takes an optional parameter specifying whether
the time format is in seconds or microseconds. For example:

```config
event-time("$(* ${UNIXTIME} 1000000)" microseconds)
event-time("12345678" microseconds)
event-time("12345678" seconds)
event-time("12345678")
```

In case the parameter is omitted, {{ site.product.short_name }} defaults to the seconds
version. In case the event-time() option is omitted altogether,
{{ site.product.short_name }} defaults to the seconds version with ${UNIXTIME}.

Note that the time format parameter requires:

- riemann-c-client 1.10.0 or newer

    In older versions of riemann-c-client, the microseconds option is
    not available.

    In case your distribution does not contain a recent enough version
    of riemann-c-client and you wish to use microseconds,
	[[install a new version of the client|riemann-cli]].

    If you installed the new version in a custom location (instead of
    the default one), make sure that you append the directory of the
    pkg-config file (.pc file) to the environment variable export
    PKG_CONFIG_PATH=....

    After calling configure, you should see the following message in the
    case of successful installation:

    [...]
      Riemann destination (module): yes, microseconds: yes
    [...]

- Riemann 2.13 or newer

    Older versions of Riemann cannot handle microseconds. No error will
    be indicated, however, the time of the event will be set to the
    timestamp when the message arrived to Riemann.

### Example: Example event-time() option

```config
destination d_riemann {
    riemann(
    server("127.0.0.1")
    port(5555)
    event-time("${UNIXTIME}")
    [...]
    );
};
```

{% include doc/admin-guide/options/hook.md %}

## host()

|  Type:|      template, macro, or string|
  |Default:|   ${HOST}|

*Description:* The value to add as the host field of the Riemann event.

{% include doc/admin-guide/options/log-fifo-size.md %}

## metric()

|  Type:|      template, macro, or string|
  |Default:|   |

*Description:* The numeric value to add as the metric field of the
Riemann event. If possible, include type-hinting as well, otherwise the
Riemann server will interpret the value as a floating-point number. The
following example specifies the ${SEQNUM} macro as an integer.

```config
metric(int("${SEQNUM}"))
```

{% include doc/admin-guide/options/port.md %}

{% include doc/admin-guide/options/retries.md %}

## server()

|  Type:|      hostname or IP address|
  |Default:|   127.0.0.1|

*Description:* The hostname or IP address of the Riemann server.

## service()

|  Type:|      template, macro, or string|
  |Default:|   ${PROGRAM}|

*Description:* The value to add as the service field of the Riemann
event.

## state()

|  Type:|      template, macro, or string|
  |Default:|   |

*Description:* The value to add as the state field of the Riemann event.

## tags()

|  Type:|      string list|
  |Default:|   the tags already assigned to the message|

*Description:* The list of tags to add as the tags field of the Riemann
event. If not specified {{ site.product.short_name }} automatically adds the tags
already assigned to the message. If you set the tags() option, only the
tags you specify will be added to the event.

{% include doc/admin-guide/options/throttle.md %}

{% include doc/admin-guide/options/time-reopen.md %}

{% include doc/admin-guide/options/timeout.md %}

## ttl()

|  Type:|      template, macro, or number|
  |Default:|   |

*Description:* The value (in seconds) to add as the ttl (time-to-live)
field of the Riemann event.

## type()

|  Type:|      tcp \| tls \| udp|
  |Default:|   tcp|

*Description:* The type of the network connection to the Riemann server:
TCP, TLS, or UDP. For TLS connections, set the ca-file() option to
authenticate the Riemann server, and the cert-file() and key-file()
options if the Riemann server requires authentication from its clients.

### Declaration 1

```config
destination d_riemann {
        riemann(
                server("127.0.0.1")
                port(5672)
                type(
                    "tls"
                    ca-file("ca")
                    cert-file("cert") 
                    key-file("key")
                )
        );
};
```

An alternative way to specify TLS options is to group them into a tls()
block. This allows you to separate them and ensure better readability.

### Declaration 2

```config
destination d_riemann {
        riemann(
                server("127.0.0.1")
                port(5672)
                type("tls")                tls(
                        ca-file("ca")
                        cert-file("cert") 
                        key-file("key")
                )
        );
};
```

Make sure that you specify TLS options either using type() or using the
tls() block. Avoid mixing the two methods. In case you do specify TLS
options in both ways, the one that comes later in the configuration file
will take effect.

{% include doc/admin-guide/options/ca-file.md %}

{% include doc/admin-guide/options/cert-file.md %}

{% include doc/admin-guide/options/key-file.md %}
