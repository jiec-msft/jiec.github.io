---
layout: post
title: linux namespace and cgroup
date: 2024-02-19 15:22:00 +0800
categories: Linux
---
Read more from [What Are Namespaces and cgroups, and How Do They Work? - NGINX](https://www.nginx.com/blog/what-are-namespaces-cgroups-how-do-they-work/)

To markdown part of the doc:
### Types of Namespaces

Within the Linux kernel, there are different types of namespaces. Each namespace has its own unique properties:

- A [user namespace](https://man7.org/linux/man-pages/man7/user_namespaces.7.html) has its own set of user IDs and group IDs for assignment to processes. In particular, this means that a process can have `root` privilege within its user namespace without having it in other user namespaces.
- A [process ID (PID) namespace](https://man7.org/linux/man-pages/man7/pid_namespaces.7.html) assigns a set of PIDs to processes that are independent from the set of PIDs in other namespaces. The first process created in a new namespace has PID 1 and child processes are assigned subsequent PIDs. If a child process is created with its own PID namespace, it has PID 1 in that namespace as well as its PID in the parent process’ namespace. See [below](https://www.nginx.com/blog/what-are-namespaces-cgroups-how-do-they-work/#pid-namespaces) for an example.
- A [network namespace](https://man7.org/linux/man-pages/man7/network_namespaces.7.html) has an independent network stack: its own private routing table, set of IP addresses, socket listing, connection tracking table, firewall, and other network‑related resources.
- A [mount namespace](https://man7.org/linux/man-pages/man7/mount_namespaces.7.html) has an independent list of mount points seen by the processes in the namespace. This means that you can mount and unmount filesystems in a mount namespace without affecting the host filesystem.
- An [interprocess communication (IPC) namespace](https://man7.org/linux/man-pages/man7/ipc_namespaces.7.html) has its own IPC resources, for example [POSIX message queues](https://man7.org/linux/man-pages/man7/mq_overview.7.html).
- A [UNIX Time‑Sharing (UTS) namespace](https://man7.org/linux/man-pages/man7/uts_namespaces.7.html) allows a single system to appear to have different host and domain names to different processes.



## Conclusion[](https://www.nginx.com/blog/what-are-namespaces-cgroups-how-do-they-work/#Conclusion)

Namespaces and cgroups are the building blocks for containers and modern applications. Having an understanding of how they work is important as we refactor applications to more modern architectures.

Namespaces provide isolation of system resources, and cgroups allow for fine‑grained control and enforcement of limits for those resources.

Containers are not the only way that you can use namespaces and cgroups. Namespaces and cgroup interfaces are built into the Linux kernel, which means that other applications can use them to provide separation and resource constraints.