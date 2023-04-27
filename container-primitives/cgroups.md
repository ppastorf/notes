# Control groups

## Good resources

- https://www.youtube.com/watch?v=sK5i-N34im8
- https://www.youtube.com/watch?v=x1npPrzyKfs
- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/ch01

## Cgroups

Linux Kernel primitive that organizes (groups) processes in the system in groups
  - Can have child cgroups, on a tree structure

Can be used to track resource usage and to manage (allocate, deny, limit, prioritize) resources to each group of tasks

Cgroups are organized hierarchically
  - Child cgroups inherit some attributes of their parents
  - Not a single tree: many different hierarchies can coexist simultaneously on a system, each one attached to a different subsystem 

Cgroups is actually an abstract framework, Subsystems are the concrete implementations
  
- Each cgroup organizes processes independently
  - Process A and B could be in the same `cpu` Subsystem cgroup and on different `memory` subsystem cgroups
- Each cgroup tree is attached to *one or more* subsystems
  
Every process belongs to an cgroup. If not, it belongs on the root cgroup for that subsystem.
  - Every *PID* is represented exactly once in each subsystem
  - When a new process starts, it begins in the same cgroup as its parent process

Cgroups are typically mounted at `/sys/fs/cgroup/` virtual filesystem. There are files for:
  - Cgroup settings
  - Cgroup resources statistics
  - List of PIDs in that cgroup
  - Directories are child cgroups

List cgroups of a particular process: `cat /proc/$PID/cgroup`  

Examples of available Subsystems:
  - `cpu`
    - leverages the scheduler to manage CPU time of tasks
  
  - `cpuset`
    - assings tasks to individual CPU cores and memory nodes

  - `pid`
    - limits the number of PIDs on given cgroup hierarchies

  - `freezer`
    - suspends or resumes tasks

  - `cpuacct`
    - generates reports on CPU resources used by tasks in cgroups
  
  - `memory`
    - sets limits on memory usage by tasks
    - generates reports on memory used by tasks

  - `hugetlb`
    - limits huge tables usage

  - `blkio`
    - sets limits on input/output access to block devices (disks, SSDs, USB)

  - `devices`
    - allows or denies access to devices by tasks

  - `net_cls`
    - tags network packets with class identifiers that allows the traffic controller (tc) to identify egress packets originating from each task

  - `net_prio`
    - dinamically sets the priority of network traffic per network interface

  - `ns`
    - provides the *namespace* subsystem

  - `perf_event`
    - identifies cgroup membership of tasks
    - can be used for performance analysis

