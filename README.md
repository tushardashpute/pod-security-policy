#What is a Pod Security Policy and why should I care?

As a cluster admin, you may have wondered how to enforce certain policies concerning runtime properties for pods in a cluster. For example, you may want to prevent developers from running a pod with containers that don’t define a user (hence, run as root). You may have documentation for developers about setting the security context in a pod specification, and developers may follow it … or they may choose not to. In any case, you need a mechanism to enforce such policies cluster-wide.

The solution is to use Pod Security Policies (PSP) as part of a defense-in-depth strategy.
