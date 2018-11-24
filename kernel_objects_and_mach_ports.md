
```ipc_kobject_set``` and ```ipc_kobject_set_atomically``` associate any kernel object with a port by setting ```port->ip_kobject = kobject;``` where ```ip_kobject``` is a macro
```
#define ip_kobject		kdata.kobject
```

That said, ```ipc_kobject_set``` sets ```kdata``` union value for a port object
```
struct ipc_port {

	/*
	 * Initial sub-structure in common with ipc_pset
	 * First element is an ipc_object second is a
	 * message queue
	 */
	struct ipc_object ip_object;
	struct ipc_mqueue ip_messages;

	union {
		struct ipc_space *receiver;
		struct ipc_port *destination;
		ipc_port_timestamp_t timestamp;
	} data;

	union {
		ipc_kobject_t kobject;
		ipc_importance_task_t imp_task;
		ipc_port_t sync_qos_override_port;
	} kdata;
  ...
}
```

The examples are as follows
```
ipc_kobject_set_atomically(port, (ipc_kobject_t) semaphore, IKOT_SEMAPHORE);
ipc_kobject_set(task->itk_resume, (ipc_kobject_t)task, IKOT_TASK_RESUME);
```
