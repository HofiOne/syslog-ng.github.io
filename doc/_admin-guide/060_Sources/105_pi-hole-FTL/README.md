---
title: Pi-hole Faster Than Light log source
short_title: pihole-ftl
id: adm-src-piftl
description: >-
    In {{ site.product.short_name }} 4.6 and later versions it is possible to collect logs of the Pi-hole FTL (Faster Than Light) application.
---

```config
source s_pihole_ftl {
  pihole-ftl();
};
```

By default, the source reads the `/var/log/pihole/FTL.log` file. If the root directory of the Pi-hole installation is different, specify the directory where the `FTL.log` file is with the dir() option. The root log directory can be accessed by selecting **Tools** > **Pi-hole diagnosis** in the Pi-hole application. The `pihole-ftl()` source has the same parameters as the file() source.

The `pihole-ftl()` driver is a reusable configuration snippet. For details on using or writing configuration snippets, see Reusing configuration blocks. The source of the Pi-hole configuration snippet can be accessed on GitHub.
