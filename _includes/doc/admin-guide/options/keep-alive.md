## keep-alive()

| Accepted values: | `yes`, `no` |
| Default:         | `yes`       |

*Description:* Specifies whether connections should be kept open across
configuration reloads (upon the receipt of a SIGHUP signal). When set to
`yes` (the default), {{ site.product.short_name }} preserves existing
connections when reloading its configuration. Set it to `no` to force
{{ site.product.short_name }} to close and reopen connections on every reload.
