# Containers

**Virtualisation** is running multiple operating systems on the same physical hardware.

```Bash
------------------------------------------
       App 1 | App 2 | ... | App N
------------------------------------------
 Runtime 1 | Runtime 2 | ... | Runtime N
------------------------------------------
Guest OS 1 | Guest OS 2 | ... | Guest OS N
------------------------------------------
          AWS Hypervisor (NITRO)
------------------------------------------
                AWS EC2 host
------------------------------------------
```

The OS alone takes a lot of resources 60% to 70% of the disk and a lot of memory leaving little resources for the applications which run in the VMs.

Most of the time, the same OS is running on multiple VMs i.e. there's duplication.

**Containerisation** is when a container runs a copy of a Docker image.

```Bash
---------------------------------------------
Container 1 | Container 2 | ... | Container N
---------------------------------------------
        Container Engine (a process)
---------------------------------------------
                   Host OS
---------------------------------------------
               AWS EC2 hardware
---------------------------------------------
```

The container which is (app + runtime) consumes little resources and is lightweight.

**Docker image**

* The Docker image is made of multiple independent layers, it is a stack of layers.
* The Dockerfile is used to create the Docker image
* Each line creates a new filesystem layer inside the Docker image
* `FROM` is used for the base image
* The filesystem layers that make up an image are read-only
* The Docker container is a running copy of the Docker image plus a read/write layer
* Multiple containers can be created from 1 image
* Layers are re-used minimising resource usage, the read/write layer is what differentiates containers.
* A container registry is a hub of container images
* Containers are islated from the outside world and they can expose ports so that the services in the container can be accessed
