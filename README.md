# What is Pod Security Policy?

In Kubernetes, workloads are deployed as Pods, which expose a lot of the functionality of running Docker containers.
Pod Security Policies are cluster-wide resources that control security sensitive aspects of pod specification. PSP objects define a set of conditions that a pod must run with in order to be accepted into the system, as well as defaults for their related fields. PodSecurityPolicy is an optional admission controller that is enabled by default through the API, thus policies can be deployed without the PSP admission plugin enabled. This functions as a validating and mutating controller simultaneously.

**With PodSecurityPolicy, you can control the following:**

- Running of privileged containers
- Usage of host namespaces
- Usage of host networking and ports
- Usage of volume types
- Usage of the host filesystem
- Allow specific FlexVolume drivers
- Allocating an FSGroup that owns the pod’s volumes
- Requiring the use of a read-only root file system
- The user and group IDs of the container
- Restricting escalation to root privileges
- Linux capabilities
- The SELinux context of the container
- The Allowed Proc Mount types for the container
- The AppArmor profile used by containers
- The seccomp profile used by containers
- The sysctl profile used by containers

While this seems to be an overwhelmingly long list, chances are, you might have already used a few of them when you are using Kubernetes, for example:

- You need to mount some storage to the pod-like PVC
- When you don’t need/want to run a pod with a root user, you use security context to run as a user/group
- For some of your applications, you need to mount some volumes to it
- For some logging applications, you need to access the logs from a path that lives on the host

You can do this, because either your cluster does not enable the PodSecurityPolicy admission controller, or it is enabled but there is a default policy that allows everything.

In the case of AWS EKS, the clusters with Kubernetes version 1.13 and higher have a default pod security policy named eks.privileged. This policy has no restriction on what kind of pod can be accepted into the system, which is equivalent to running Kubernetes with the PodSecurityPolicy controller disabled (or there is one that allows you to do everything).

**What PodSecurityPolicy Can Not Do**

However, PodSecurityPolicy can’t do everything.

If it’s not in the list above, it can’t do it.

Also, due to the nature of the admission controller, the policy only works when you are creating or updating the pod. If the pod violates the policy, it won’t be created.

However, if you modify the policy after pods are already up and running, making the pods violating the new policy, the pods won’t be shut down.

PodSecurityPolicy, as the name suggests, is only a set of policies that are enforced when creating/updating pod. It is not a container run-time security platform that can detect violations and shutdown pods. This is important to know, and this explains why you might want to consider tools like aqua, sysdig, Falco when you want more control over Kubernetes run-time security.


https://res.cloudinary.com/practicaldev/image/fetch/s--hrJarSwJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/pwtsbifjjfu3ooyup3wu.png


  kubectl get psp eks.privileged

<img width="705" alt="image" src="https://user-images.githubusercontent.com/74225291/197373481-270dd932-e715-4b39-8b95-23233598c119.png">

Default policy looks like this:

    ---
    apiVersion: policy/v1beta1
    kind: PodSecurityPolicy
    metadata:
      name: privileged
      annotations:
        kubernetes.io/description: 'privileged allows full unrestricted access to
          pod features, as if the PodSecurityPolicy controller was not enabled.'
        seccomp.security.alpha.kubernetes.io/allowedProfileNames: '*'
      labels:
        kubernetes.io/cluster-service: "true"
        eks.amazonaws.com/component: pod-security-policy
    spec:
      privileged: true
      allowPrivilegeEscalation: true
      allowedCapabilities:
      - '*'
      volumes:
      - '*'
      hostNetwork: true
      hostPorts:
      - min: 0
        max: 65535
      hostIPC: true
      hostPID: true
      runAsUser:
        rule: 'RunAsAny'
      seLinux:
        rule: 'RunAsAny'
      supplementalGroups:
        rule: 'RunAsAny'
      fsGroup:
        rule: 'RunAsAny'
      readOnlyRootFilesystem: false

