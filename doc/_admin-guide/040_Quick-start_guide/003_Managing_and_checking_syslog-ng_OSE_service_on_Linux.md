---
title: Managing and checking {{ site.product.short_name }} service on Linux
id: adm-qs-service
description: >-
    This section describes how to start, stop and check the status of
    {{ site.product.short_name }} service on Linux.
---

## Starting {{ site.product.short_name }}

To start {{ site.product.short_name }}, execute the following command as root.

```bash
systemctl start syslog-ng
```

If the service starts successfully, no output will be displayed.

The following message indicates that {{ site.product.short_name }} can not start (see
Checking {{ site.product.short_name }} status):

Job for syslog-ng.service failed because the control process exited with
error code. See **systemctl status syslog-ng.service** and **journalctl
-xe** for details.

## Stopping {{ site.product.short_name }}

To stop {{ site.product.short_name }}

1. Execute the following command as root.

    ```bash
    systemctl stop syslog-ng
    ```

2. Check the status of {{ site.product.short_name }} service (see Checking {{ site.product.short_name }} status).

## Restarting {{ site.product.short_name }}

To restart {{ site.product.short_name }}, execute the following command as root.

```bash
systemctl restart syslog-ng
```

## Reloading configuration file without restarting {{ site.product.short_name }}

To reload the configuration file without restarting {{ site.product.short_name }},
execute the following command as root.

```bash
systemctl reload syslog-ng
```

## Checking {{ site.product.short_name }} status

To check the following status-related components, observe the
suggestions below.

### Checking the status of {{ site.product.short_name }} service

To check the status of {{ site.product.short_name }} service

1. Execute the following command as root.

    ```bash
    systemctl --no-pager status syslog-ng
    ```

2. Check the Active: field, which shows the status of {{ site.product.short_name }} service. The following statuses are possible:

- **active (running)** - {{ site.product.short_name }} service is up and running

    Example: {{ site.product.short_name }} service active  

    > syslog-ng.service - System Logger Daemon  
    > Loaded: loaded (/lib/systemd/system/syslog-ng.service; enabled; vendor preset: enabled)  
    > Active: active (running) since Tue 2019-06-25 08:58:09 CEST; 5s ago  
    > Main PID: 6575 (syslog-ng)  
    > Tasks: 3  
    > Memory: 13.3M  
    > CPU: 268ms  
    > CGroup: /system.slice/syslog-ng.service  
    > 6575 /opt/syslog-ng/libexec/syslog-ng -F --no-caps --enable-core  

- **inactive (dead)** - syslog-ng service is stopped

    Example: {{ site.product.short_name }} status inactive

    > syslog-ng.service - System Logger Daemon  
    > Loaded: loaded (/lib/systemd/system/syslog-ng.service; enabled; vendor preset: enabled)  
    > Active: inactive (dead) since Tue 2019-06-25 09:14:16 CEST; 2min 18s ago  
    > Process: 6575 ExecStart=/opt/syslog-ng/sbin/syslog-ng -F --no-caps --enable-core $SYSLOGNG_OPTIONS(code=exited, status=0/SUCCESS)  
    > Main PID: 6575 (code=exited, status=0/SUCCESS)  
    > Status: "Shutting down... Tue Jun 25 09:14:16 2019"  
    > Jun 25 09:14:31 as-syslog-srv systemd: Stopped System Logger Daemon.

### Checking the process of {{ site.product.short_name }}

To check the process of {{ site.product.short_name }}, execute one of the following commands.

```bash
ps u `pidof syslog-ng`
```

Expected output example:

> USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
>  
> syslogng 6709 0.0 0.6 308680 13432 ? Ss 09:17 0:00  
> /opt/syslog-ng/libexec/syslog-ng -F --no-caps --enable-core

```bash
ps axu | grep syslog-ng | grep -v grep
```

Expected output example:

> syslogng 6709 0.0 0.6 308680 13432 ? Ss 09:17 0:00  
> /opt/syslog-ng/libexec/syslog-ng -F --no-caps --enable-core

### Checking the internal logs of {{ site.product.short_name }}**

The internal logs of {{ site.product.short_name }} contains informal, warning and error messages.

By default, {{ site.product.short_name }} log messages (generated on the internal() source) are written to **/var/log/messages**.

Check the internal logs of {{ site.product.short_name }} for any issue.

### Message processing

The {{ site.product.short_name }} application collects statistics about the number of processed messages on the different sources and destinations.

**NOTE:** When using syslog-ng-ctl stats, consider that while the output
is generally consistent, there is no explicit ordering behind the
command. Consequently, One Identity does not recommend creating
parsers that depend on a fix output order.
{: .notice--info}

If needed, you can sort the output with an external application, for
example, `| sort`.

### Central statistics

To check the central statistics, execute the following command to see the number of received and queued (sent) messages by {{ site.product.short_name }}.

```bash
watch "/opt/syslog-ng/sbin/syslog-ng-ctl stats | grep ^center"
```

The output will be updated in every 2 seconds.

If the numbers are changing, {{ site.product.short_name }} is processing the messages.

Example: output example

> Every 2.0s: /opt/syslog-ng/sbin/syslog-ng-ctl stats | grep  
> ^center       Tue Jun 25 10:33:25 2019  
> center;;queued;a;processed;112  
> center;;received;a;processed;28  

### Source statistics

To check the source statistics, execute the following command to see the number of received messages on the configured sources.

```bash
watch "/opt/syslog-ng/sbin/syslog-ng-ctl stats | grep ^source"
```

The output will be updated in every 2 seconds.

If the numbers are changing, {{ site.product.short_name }} is receiving messages on the sources.

Example: output example

> Every 2.0s: /opt/syslog-ng/sbin/syslog-ng-ctl stats | grep  
> ^source      Tue Jun 25 10:40:50 2019  
> source;s_null;;a;processed;0  
> source;s_net;;a;processed;0  
> source;s_local;;a;processed;90  

### Destination statistics

To check the source statistics, execute the following command to see the number of received messages on the configured sources.

```bash
watch "/opt/syslog-ng/sbin/syslog-ng-ctl stats | grep ^destination"
```

The output will be updated in every 2 seconds.

If the numbers are changing, {{ site.product.short_name }} is receiving messages on the sources.

Example: output example

> Every 2.0s: /opt/syslog-ng/sbin/syslog-ng-ctl stats | grep  
> ^destination      Tue Jun 25 10:41:02 2019  
> destination;d_logserver2;;a;processed;90  
> destination;d_messages;;a;processed;180  
> destination;d_logserver;;a;processed;90  
> destination;d_null;;a;processed;0  

**NOTE:** If you find error messages in the internal logs, messages are not
processed by {{ site.product.short_name }} or you encounter any issue, you have the
following options:
{: .notice--info}

- Search for the error or issue in our knowledge base.
- Check the following troubleshooting articles.
- Open a support service request including the results.
