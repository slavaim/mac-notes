A list of default macOS MAC policies

```
(lldb) p mac_policy_list.entries[0].mpc
(mac_policy_conf *) $90 = 0xffffff7f823db4b8
(lldb) p *mac_policy_list.entries[0].mpc
(mac_policy_conf) $91 = {
  mpc_name = 0xffffff7f823d7f13 "AMFI"
  mpc_fullname = 0xffffff7f823d7f18 "Apple Mobile File Integrity"
  mpc_labelnames = 0xffffff7f823da6d0
  mpc_labelname_count = 1
  mpc_ops = 0xffffff7f823daa40
  mpc_loadtime_flags = 0
  mpc_field_off = 0xffffff7f823da9a0
  mpc_runtime_flags = 1
  mpc_list = 0x0000000000000000
  mpc_data = 0x0000000000000000
}
(lldb) p *mac_policy_list.entries[1].mpc
(mac_policy_conf) $92 = {
  mpc_name = 0xffffff7f82bda2fd "Sandbox"
  mpc_fullname = 0xffffff7f82bda53a "Seatbelt sandbox policy"
  mpc_labelnames = 0xffffff7f82be0110
  mpc_labelname_count = 1
  mpc_ops = 0xffffff7f82be0118
  mpc_loadtime_flags = 0
  mpc_field_off = 0xffffff7f82be1378
  mpc_runtime_flags = 1
  mpc_list = 0x0000000000000000
  mpc_data = 0x0000000000000000
}
(lldb) p *mac_policy_list.entries[2].mpc
(mac_policy_conf) $93 = {
  mpc_name = 0xffffff7f83d73fd4 "TMSafetyNet"
  mpc_fullname = 0xffffff7f83d73fe0 "Safety net for Time Machine"
  mpc_labelnames = 0xffffff7f83d74060
  mpc_labelname_count = 1
  mpc_ops = 0xffffff7f83d74068
  mpc_loadtime_flags = 2
  mpc_field_off = 0xffffff7f83d74be8
  mpc_runtime_flags = 1
  mpc_list = 0x0000000000000000
  mpc_data = 0x0000000000000000
}
(lldb) p *mac_policy_list.entries[3].mpc
(mac_policy_conf) $94 = {
  mpc_name = 0xffffff7f84159c06 "Quarantine"
  mpc_fullname = 0xffffff7f84159c7c "Quarantine policy"
  mpc_labelnames = 0xffffff7f8415a160
  mpc_labelname_count = 1
  mpc_ops = 0xffffff7f8415a168
  mpc_loadtime_flags = 0
  mpc_field_off = 0xffffff7f8415acfc
  mpc_runtime_flags = 1
  mpc_list = 0x0000000000000000
  mpc_data = 0x0000000000000000
}
```
