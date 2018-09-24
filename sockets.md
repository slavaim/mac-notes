
Socket file operations

```
const struct fileops socketops = {
	.fo_type = DTYPE_SOCKET,
	.fo_read = soo_read,
	.fo_write = soo_write,
	.fo_ioctl = soo_ioctl,
	.fo_select = soo_select,
	.fo_close = soo_close,
	.fo_kqfilter = soo_kqfilter,
	.fo_drain = soo_drain,
};
```

AF_UNIX socket connection and structure

```
(lldb) bt
* thread #7, name = '0xffffff80193b0080', queue = '0x0', stop reason = breakpoint 1.1
  * frame #0: 0xffffff8006af26f8 kernel.development`unp_connect(so=<unavailable>, nam=<unavailable>, p=<unavailable>) at uipc_usrreq.c:1104 [opt]
    frame #1: 0xffffff8006adc83d kernel.development`soconnectlock(so=0xffffff801671dee0, nam=<unavailable>, dolock=<unavailable>) at uipc_socket.c:1671 [opt]
    frame #2: 0xffffff8006aeca53 kernel.development`connect_nocancel [inlined] connectit(so=<unavailable>, sa=<unavailable>) at uipc_syscalls.c:926 [opt]
    frame #3: 0xffffff8006aec9f2 kernel.development`connect_nocancel(p=<unavailable>, uap=<unavailable>, retval=<unavailable>) at uipc_syscalls.c:722 [opt]
    frame #4: 0xffffff8006ba69fa kernel.development`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #5: 0xffffff80065229d6 kernel.development`hndl_unix_scall64 + 22
    
int
soconnectlock(struct socket *so, struct sockaddr *nam, int dolock)
{
...
	error = (*so->so_proto->pr_usrreqs->pru_connect)(so, nam, p);
...
}

static int
unp_connect(struct socket *so, struct sockaddr *nam, __unused proc_t p)
{
    struct sockaddr_un *soun = (struct sockaddr_un *)nam;
...
	bcopy(soun->sun_path, buf, len);
	buf[len] = 0;
...
	NDINIT(&nd, LOOKUP, OP_LOOKUP, FOLLOW | LOCKLEAF, UIO_SYSSPACE,
	    CAST_USER_ADDR_T(buf), ctx);
	error = namei(&nd);
...
}

(lldb) p *so->so_proto
(protosw) $39 = {
  pr_entry = {
    tqe_next = 0xffffff8006ea11a8
    tqe_prev = 0xffffff8006ea10c0
  }
  pr_domain = 0xffffff8006ea1088
  pr_protosw = 0xffffff8006ea1110
  pr_type = 1
  pr_protocol = 0
  pr_flags = 3356
  pr_input = 0x0000000000000000
  pr_output = 0x0000000000000000
  pr_ctlinput = 0x0000000000000000
  pr_ctloutput = 0xffffff8006af4170 (kernel.development`uipc_ctloutput at uipc_usrreq.c:709)
  pr_usrreqs = 0xffffff8006ea2130
  pr_init = 0x0000000000000000
  pr_drain = 0x0000000000000000
  pr_sysctl = 0x0000000000000000
  pr_lock = 0xffffff8006af4830 (kernel.development`unp_lock at uipc_usrreq.c:2429)
  pr_unlock = 0xffffff8006af4920 (kernel.development`unp_unlock at uipc_usrreq.c:2457)
  pr_getlock = 0xffffff8006af4e40 (kernel.development`unp_getlock at uipc_usrreq.c:2501)
  pr_filter_head = {
    tqh_first = 0x0000000000000000
    tqh_last = 0xffffff8006ea1190
  }
  pr_old = 0x0000000000000000
}
(lldb) p *so->so_proto->pr_usrreqs
(pr_usrreqs) $40 = {
  pru_flags = 0
  pru_abort = 0xffffff8006af1490 (kernel.development`uipc_abort at uipc_usrreq.c:207)
  pru_accept = 0xffffff8006af14e0 (kernel.development`uipc_accept at uipc_usrreq.c:220)
  pru_attach = 0xffffff8006af1530 (kernel.development`uipc_attach at uipc_usrreq.c:247)
  pru_bind = 0xffffff8006af1690 (kernel.development`uipc_bind at uipc_usrreq.c:257)
  pru_connect = 0xffffff8006af1a40 (kernel.development`uipc_connect at uipc_usrreq.c:273)
  pru_connect2 = 0xffffff8006af1a60 (kernel.development`uipc_connect2 at uipc_usrreq.c:289)
  pru_connectx = 0xffffff8006aeadf0 (kernel.development`pru_connectx_notsupp at uipc_socket2.c:2110)
  pru_control = 0xffffff8006aeae00 (kernel.development`pru_control_notsupp at uipc_socket2.c:2118)
  pru_detach = 0xffffff8006af1a80 (kernel.development`uipc_detach at uipc_usrreq.c:302)
  pru_disconnect = 0xffffff8006af1ac0 (kernel.development`uipc_disconnect at uipc_usrreq.c:316)
  pru_disconnectx = 0xffffff8006aeae30 (kernel.development`pru_disconnectx_notsupp at uipc_socket2.c:2139)
  pru_listen = 0xffffff8006af1ae0 (kernel.development`uipc_listen at uipc_usrreq.c:330)
  pru_peeraddr = 0xffffff8006af1b90 (kernel.development`uipc_peeraddr at uipc_usrreq.c:340)
  pru_rcvd = 0xffffff8006af1be0 (kernel.development`uipc_rcvd at uipc_usrreq.c:356)
  pru_rcvoob = 0xffffff8006aeae70 (kernel.development`pru_rcvoob_notsupp at uipc_socket2.c:2167)
  pru_send = 0xffffff8006af1cc0 (kernel.development`uipc_send at uipc_usrreq.c:421)
  pru_send_list = 0xffffff8006aeae90 (kernel.development`pru_send_list_notsupp at uipc_socket2.c:2183)
  pru_sense = 0xffffff8006af2510 (kernel.development`uipc_sense at uipc_usrreq.c:612)
  pru_shutdown = 0xffffff8006af2590 (kernel.development`uipc_shutdown at uipc_usrreq.c:658)
  pru_sockaddr = 0xffffff8006af2600 (kernel.development`uipc_sockaddr at uipc_usrreq.c:674)
  pru_sopoll = 0xffffff8006aeaf20 (kernel.development`pru_sopoll_notsupp at uipc_socket2.c:2259)
  pru_soreceive = 0xffffff8006ade6e0 (kernel.development`soreceive at uipc_socket.c:3095)
  pru_soreceive_list = 0xffffff8006aeaef0 (kernel.development`pru_soreceive_list_notsupp at uipc_socket2.c:2238)
  pru_sosend = 0xffffff8006add090 (kernel.development`sosend at uipc_socket.c:2040)
  pru_sosend_list = 0xffffff8006aeaed0 (kernel.development`pru_sosend_list_notsupp at uipc_socket2.c:2222)
  pru_socheckopt = 0xffffff8006aeaf30 (kernel.development`pru_socheckopt_null at uipc_socket2.c:2266)
  pru_preconnect = 0xffffff8006aeb180 (kernel.development`pru_preconnect_null at uipc_socket2.c:2276)
}

struct pr_usrreqs uipc_usrreqs = {
	.pru_abort =		uipc_abort,
	.pru_accept =		uipc_accept,
	.pru_attach =		uipc_attach,
	.pru_bind =		uipc_bind,
	.pru_connect =		uipc_connect,
	.pru_connect2 =		uipc_connect2,
	.pru_detach =		uipc_detach,
	.pru_disconnect =	uipc_disconnect,
	.pru_listen =		uipc_listen,
	.pru_peeraddr =		uipc_peeraddr,
	.pru_rcvd =		uipc_rcvd,
	.pru_send =		uipc_send,
	.pru_sense =		uipc_sense,
	.pru_shutdown =		uipc_shutdown,
	.pru_sockaddr =		uipc_sockaddr,
	.pru_sosend =		sosend,
	.pru_soreceive =	soreceive,
};
```

An AF_UNIX socket creation. A file system lookup returns EJUSTRETURN if a socket file doesn't exists, this error is converted to a successful code as CREATE is requested soo a lookup call(i.e. ```namei()```) is followed by ```vn_create```.

```

static int
uipc_bind(struct socket *so, struct sockaddr *nam, proc_t p)
{
	struct unpcb *unp = sotounpcb(so);

	if (unp == 0)
		return (EINVAL);

	return (unp_bind(unp, nam, p));
}

static int
unp_bind(
	struct unpcb *unp,
	struct sockaddr *nam,
	proc_t p)
{
...
	NDINIT(&nd, CREATE, OP_MKFIFO, FOLLOW | LOCKPARENT, UIO_SYSSPACE,
	    CAST_USER_ADDR_T(buf), ctx);
	/* SHOULD BE ABLE TO ADOPT EXISTING AND wakeup() ALA FIFO's */
	error = namei(&nd);
	if (error) {
		socket_lock(so, 0);
		return (error);
	}
	dvp = nd.ni_dvp;
	vp = nd.ni_vp;

	if (vp != NULL) {
		/*
		 * need to do this before the vnode_put of dvp
		 * since we may have to release an fs_nodelock
		 */
		nameidone(&nd);

		vnode_put(dvp);
		vnode_put(vp);

		socket_lock(so, 0);
		return (EADDRINUSE);
	}

	VATTR_INIT(&va);
	VATTR_SET(&va, va_type, VSOCK);
	VATTR_SET(&va, va_mode, (ACCESSPERMS & ~p->p_fd->fd_cmask));

#if CONFIG_MACF
	error = mac_vnode_check_create(ctx,
	    nd.ni_dvp, &nd.ni_cnd, &va);

	if (error == 0)
#endif /* CONFIG_MACF */
#if CONFIG_MACF_SOCKET_SUBSET
	error = mac_vnode_check_uipc_bind(ctx,
	    nd.ni_dvp, &nd.ni_cnd, &va);

	if (error == 0)
#endif /* MAC_SOCKET_SUBSET */
	/* authorize before creating */
	error = vnode_authorize(dvp, NULL, KAUTH_VNODE_ADD_FILE, ctx);

	if (!error) {
		/* create the socket */
		error = vn_create(dvp, &vp, &nd, &va, 0, 0, NULL, ctx);
	}
	
	nameidone(&nd);
	vnode_put(dvp);

	if (error) {
		socket_lock(so, 0);
		return (error);
	}
	vnode_ref(vp);	/* gain a longterm reference */
	socket_lock(so, 0);
	vp->v_socket = unp->unp_socket;
	unp->unp_vnode = vp;
	unp->unp_addr = (struct sockaddr_un *)dup_sockaddr(nam, 1);
	vnode_put(vp);		/* drop the iocount */

	return (0);
}
```

A UNIX (i.e. local) DNS resolver ```/var/run/mDNSResponder``` socket connection in a user mode

```
  * frame #0: 0x00007fff6cfec0a4 libsystem_kernel.dylib`__connect_nocancel
    frame #1: 0x00007fff6cf81909 libsystem_dnssd.dylib`ConnectToServer + 864
    frame #2: 0x00007fff6cf83032 libsystem_dnssd.dylib`DNSServiceCreateConnection + 55
    frame #3: 0x00007fff6cf91d5c libsystem_info.dylib`_mdns_search + 860
    frame #4: 0x00007fff6cf9153c libsystem_info.dylib`mdns_hostbyname + 428
    frame #5: 0x00007fff6cf91154 libsystem_info.dylib`si_host_byname + 116
    frame #6: 0x00007fff6cf91269 libsystem_info.dylib`search_host_byname + 249
    frame #7: 0x00007fff6cf91154 libsystem_info.dylib`si_host_byname + 116
    frame #8: 0x00007fff6cfad18b libsystem_info.dylib`gethostbyname2 + 235
    frame #9: 0x00000001000021bd ping`___lldb_unnamed_symbol2$$ping + 4125
    frame #10: 0x00007fff6ce9c015 libdyld.dylib`start + 1
```

and the same in a kernel mode

```
    frame #1: 0xffffff802cbe8d4c kernel.development`lookup [inlined] VNOP_LOOKUP(cnp=<unavailable>, ctx=<unavailable>) at kpi_vfs.c:3010 [opt]
    frame #2: 0xffffff802cbe8cfe kernel.development`lookup(ndp=<unavailable>) at vfs_lookup.c:1150 [opt]
    frame #3: 0xffffff802cbe7f12 kernel.development`namei(ndp=0xffffff8034c2bc58) at vfs_lookup.c:390 [opt]
    frame #4: 0xffffff802cef1cfc kernel.development`unp_connect(so=<unavailable>, nam=<unavailable>, p=<unavailable>) at uipc_usrreq.c:1130 [opt]
    frame #5: 0xffffff802cedbd9d kernel.development`soconnectlock(so=0xffffff803b8e0a08, nam=<unavailable>, dolock=<unavailable>) at uipc_socket.c:1671 [opt]
    frame #6: 0xffffff802ceebfb3 kernel.development`connect_nocancel [inlined] connectit(so=<unavailable>, sa=<unavailable>) at uipc_syscalls.c:926 [opt]
    frame #7: 0xffffff802ceebf52 kernel.development`connect_nocancel(p=<unavailable>, uap=<unavailable>, retval=<unavailable>) at uipc_syscalls.c:722 [opt]
    frame #8: 0xffffff802cfa60ca kernel.development`unix_syscall64(state=<unavailable>) at systemcalls.c:382 [opt]
    frame #9: 0xffffff802c920a36 kernel.development`hndl_unix_scall64 + 22
```
