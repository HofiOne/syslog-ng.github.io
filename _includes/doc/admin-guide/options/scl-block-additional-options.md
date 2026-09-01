## Additional options

This driver is implemented as a {{ site.product.short_name }} SCL block. In addition to the options listed above, you can pass any option supported by the underlying driver directly to this block, since it accepts `...` (varargs) and forwards any extra parameters to the inner driver. For example, you can use `persist-name()` to override the automatically generated persist name.
