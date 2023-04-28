# Namespaces

## Resources

- `man 7 namespaces`
- [Redhat: The 7 most used Linux namespaces](https://www.redhat.com/sysadmin/7-linux-namespaces)
- [Redhat: Manage containers in namespaces by using nsenter](https://www.redhat.com/sysadmin/container-namespaces-nsenter)
- [linuxfestnorthwest: Linux Container Primitives: cgroups, namespaces, and more!](https://youtu.be/x1npPrzyKfs?t=610)


## Namespaces

Linux Kernel primitive that controls visibility of resources, providing isolation and access control

Can be used to give processes isolated views of resouces

Can map resources outside a namespace to inside the namespace changing permissions, with changes only applied to processes in the namespace

Types of Namespaces currently available:

- `Network`: isolates IPv4 and IPv6 stacks, IP routing tables, firewall rules, sockets, etc
- `Mount`: isolates mounted Filesystems
- `PID`: isolates the mapping of PIDs to processes 
- `IPC`: isolates POSIX and System V message queues, semaphore sets and shared memory segments
- `UTS`: isolates Hostname and domain name
- `user`: isolates User and group IDs
- `cgroup`: isolates the view of /proc/$PID/cgroup
- `time`: isolates the view of the system clock

Just like cgroups, different namespaces can organize processes independently of each other

The kernel automatically garbage collets namespaces by counting the number of references, so if there are no processes inside a namespace it is automatically closed. New namespaces remains open as long as:
  - at least one process runs on it
  - at least one mount is open on it

### Manipulating Namespaces

Namespaces for a given process are visible on `/proc/$PID/ns/`, files are symbolic links to each namespace, containing the ns type and inode number to identify the ns

Unlike cgroups, namespaces are not to be created through the filesystem

Namespaces can be manipulated using the following syscalls / programs:

- `clone`: creates a child process possibly in a new namespace 
- `unshare`: runs a program in a new namespace
- `nsenter`: runs a program in an existing namespace 
- `setns`: sets a task to an existing namespace
- `ip netns`: manipulate tasks in network namespaces
- `lsns`: lists namespaces visible

#### Examples

##### nsenter
Run a (host) command in the network namespace of a running process
```
sudo nsenter --target $PID --net $COMMAND
```
If $COMMAND is ommited, it enters the default shell

Run a command defined inside a container (we need to enter the mount namespace to see it)
```
sudo nsenter --target $PID --mount $CONTAINER_COMMAND
```

Check processes running inside a container (it must have `ps` binary)
```
sudo nsenter --target $PID --pid --mount ps
```

Run a command in all the namespaces of a running process
```
sudo nsenter --target $PID --all $COMMAND
```

Check the hostname of a container
```
sudo nsenter --target $PID --uts hostname
```

Check the network interfaces of a container
```
sudo nsenter --target $PID --net ip a
```

Check `nsenter --help` for other namespaces

### About some namespaces

**Network namespaces**

Used to isolate containers networking

`[veth]`(https://man7.org/linux/man-pages/man4/veth.4.html) (Virtual Ethernet Devices) can be used to connect different nw namespaces 

Multiple containers can share a nw namespace, such as Kubernetes pods

**Mount namespaces**

[Redhat: The mount namespace](https://www.redhat.com/sysadmin/mount-namespaces)

Can provide a separate view of the filesystem with a different root, hiding the rest of the fs from the processes in the namespace. 

Used to give containers their own filesystem, the container image is mounted as the root filesystem.

`Volumes` are just mounts added to the container namespace.

**PIDs namespaces**

[Redhat: The PID namespace](https://www.redhat.com/sysadmin/pid-namespace)

Used to isolate the view of PIDs in the system.

A process can have X PID for processes inside his namespace, but Y PID for process outside that namespace.

Two processes can have the same PID if they are in different namespaces.

A process can also be hidden from processes inside certain namespaces

A process can see the PIDs of its children, but not of its parents.

The first process in a new PID namespace becomes the `init` process.

A namespace can see processes from other namespaces as long as they are in the same `mount` namespace, because it depends on what is set at `/proc`

