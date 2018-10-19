A collection of useful lldb commands.

Lookup for a type definition.
```
(lldb) image lookup -t mpo_vnode_check_signature_t
Best match found in /Developer/KernelDebugKit/10.14/18A391/System/Library/Kernels/kernel:
id = {0x0103d232}, name = "mpo_vnode_check_signature_t", decl = mac_policy.h:5417, compiler_type = "typedef mpo_vnode_check_signature_t"
     typedef 'mpo_vnode_check_signature_t': id = {0x0103d23e}, name = "int (vnode *, label *, cpu_type_t, cs_blob *, unsigned int *, unsigned int *, int, char **, size_t *)", compiler_type = "int (struct vnode *, struct label *, cpu_type_t, struct cs_blob *, unsigned int *, unsigned int *, int, char **, size_t *)"
```
