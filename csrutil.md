
```
csrutil enable --no-internal
csrutil enable --without kext
csrutil enable --without fs
csrutil enable --without debug
csrutil enable --without dtrace
csrutil enable --without nvram

csrutil enable --without kext --without debug --without dtrace
```
