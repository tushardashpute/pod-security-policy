# What is a Pod Security Policy and why should I care?

As a cluster admin, you may have wondered how to enforce certain policies concerning runtime properties for pods in a cluster. For example, you may want to prevent developers from running a pod with containers that don’t define a user (hence, run as root). You may have documentation for developers about setting the security context in a pod specification, and developers may follow it … or they may choose not to. In any case, you need a mechanism to enforce such policies cluster-wide.

**The solution is to use Pod Security Policies (PSP) as part of a defense-in-depth strategy.**

PSP is a cluster-wide resource, enabling you as a cluster admin to enforce the usage of security contexts in your cluster. The enforcement of PSPs is carried out by the API server’s admission controller. In a nutshell: if a pod spec doesn’t meet what you defined in a PSP, the API server will refuse to launch it.For PSPs to work, the respective admission(https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#how-do-i-turn-on-an-admission-control-plug-in) plugin must be enabled, and permissions must be granted to users. From EKS 1.13 cluster now has the PSP admission plugin enabled by default, so there’s nothing EKS users need to do.

