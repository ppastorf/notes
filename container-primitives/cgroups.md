# Control groups

- organizes (groups) processes in the system
- track resource usage by each group of processes
- ability to allocate, deny, limit, prioritize and monitor resources to each group of tasks

- cgroups are organized hierarchically, as trees
  - child cgroups inherit some attributes of their parents
  - many different hierarchies can exist simultaneously on a system

- Cgroups is an abstract framework
  - Subsystems are the concrete implementations

- each Subsystem organizes processes independently
- each gcroup tree is attached to *one or more* subsystems
- every *PID* is represented exactly once in each subsystem
- when a new process starts, it begins in the same cgroup as its parent process

- cgroups are typically mounted at `/sys/fs/cgroup/`
  - settings files
  - stats files
  - list of PIDs in that  

- List cgroups of a particular process: `cat /proc/$PID/cgroup`  

- Examples of available Subsystems:
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

##
