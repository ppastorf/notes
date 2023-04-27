# Filesystem images

## Resources

- [Union FS - Wikipedia](https://en.wikipedia.org/wiki/UnionFS)
- [Overlay FS - Kernel Docs](https://docs.kernel.org/filesystems/overlayfs.html)
- [https://youtu.be/x1npPrzyKfs?t=1236](linuxfestnorthwest: Linux Container Primitives: cgroups, namespaces, and more!)

## Filesystem images

Container images are nothing more than files and directories that make the userland of the container

Contains all the software that runs inside the container, not differently from a regular Linux OS or VM

A container image could be compared as a upgraded `.tar` file of the container's root directory. The upgrade would be the layering technology

Mounts for each process can be inspected at `/proc/$PID/mountinfo` and `/proc/$PID/mounts`

### Layers

Layers are way of creating a layered view of the filesystem, in which each new layer inherits the contents of the last layer, "below" it, and adds its own modifications to it 

A new layer adds, deletes or modifies files from the layer just below it

Only the top-most layer is writable (read-write), the base and intermediate layers are all read-only

When a file is modified, it is copied up to the top layeri

Files exist only in the layer they were last added or modified

Deleted files are hidden from the new layers, but still exist in the layers below

Layers are typically implemented in a variance of UnionFS, and enables for efficient use of storage when
  - making modifications to images
  - starting multiple containers from the same image

### Union file systems

Container images are typically implemented in a sort of Union file system

UnionFS is a filesystem service which implements a union mount other file systems into a new virtual file system. 

It allows files and directories of separate FS's (branches) to be merged, forming a single coherent FS.

The priority of one branch over the other is specified when mounting. When one both branches contain a file with the same path, the one with priority is used.

The new branches may be read-only or read-write fily systems. Writes to the generated virtual FS can directed to a real FS, allowing for writes on top of a read-only UnionsFS.

### OverlayFS

OverlayFS is an implementation of Union file system for Linux.
  - Its the default layer storage used by Docker (overlay2)

Joins two directories called upper and lower to form the union.

Upper directory has priority over the lower directory for files existing in both directories

When writing to the the overlay, only the upperdir is modified.
  - When a file to be modified exists on the lowerdir, the file is first copied to the upperdir
  - The whole file is copied, not just modified blocks

An upperdir can have multiple lowerdirs

OverlayFSs can be created using the program `mount`
 
