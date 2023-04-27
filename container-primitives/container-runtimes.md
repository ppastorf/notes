# Container Runtimes

## Resources

- [About the Open Containers Initiative](https://opencontainers.org/about/overview/)
- [linuxfestnorthwest: Linux Container Primitives: cgroups, namespaces, and more!](https://youtu.be/x1npPrzyKfs?t=1860)

## Container Runtimes

Container runtimes are the software that leverages the Linux container primitives to provide an clean and automated interface to running and managing containers and packaging container images

Examples:
- Docker
- containerd
- runc
- CRI-O

### OCI

OCI is the Open Containers Initiative, that aims to standardize container runtimes, image format and distribution

OCI implementation is runc, and it powers Docker, containerd and CRI-O

The OCI specifies each container as a "bundle" which consists of
- Filesystem (can be a Union FS)
- JSON Document describing the container
  - groups
  - Namespaces
  - Additional mounts (volumes)
  - Linux capabilities
  - Linux security modules
  - etc

OCI specifies `Hooks`, that are special programs that are able to mutate the bundle during the container lifecycle

Hooks run sequentially, in an order defined at the JSON document. They can run
- Before a container starts
- After a container starts
- After a container stops

Hooks can modify
- the filesystem
- the JSON document

Or take other actions related to the container runtimes



