# What is Pod Security Policy?

In Kubernetes, workloads are deployed as Pods, which expose a lot of the functionality of running Docker containers.
Pod Security Policies are cluster-wide resources that control security sensitive aspects of pod specification. PSP objects define a set of conditions that a pod must run with in order to be accepted into the system, as well as defaults for their related fields. PodSecurityPolicy is an optional admission controller that is enabled by default through the API, thus policies can be deployed without the PSP admission plugin enabled. This functions as a validating and mutating controller simultaneously.

**Pod Security Policies allow you to control:**

- The running of privileged containers
- Usage of host namespaces
- Usage of host networking and ports
- Usage of volume types
- Usage of the host filesystem
- A white list of Flexvolume drivers
- The allocation of an FSGroup that owns the pod’s volumes
- Requirements for use of a read only root file system
- The user and group IDs of the container
- Escalations of root privileges
- Linux capabilities, SELinux context, AppArmor, seccomp, sysctl profile

