# Incus

Incus is a modern, secure and powerful system container and virtual machine manager.

It provides a unified experience for running and managing full Linux systems inside containers or virtual machines. Incus supports images for a large number of Linux distributions (official Ubuntu images and images provided by the community) and is built around a very powerful, yet pretty simple, REST API. Incus scales from one instance on a single machine to a cluster in a full data center rack, making it suitable for running workloads both for development and in production.

Incus allows you to easily set up a system that feels like a small private cloud. You can run any type of workload in an efficient way while keeping your resources optimized.

You should consider using Incus if you want to containerize different environments or run virtual machines, or in general run and manage your infrastructure in a cost-effective way.

You can try Incus online at: [[`https://linuxcontainers.org/incus/try-it/`]](https://linuxcontainers.org/incus/try-it/)
## Security

Consider the following aspects to ensure that your Incus installation is secure:

-   Keep your operating system up-to-date and install all available security patches.

-   Use only supported Incus versions.

-   Restrict access to the Incus daemon and the remote API.

-   Do not use privileged containers unless required. If you use privileged containers, put appropriate security measures in place. See the [LXC security page](https://linuxcontainers.org/lxc/security/) for more information.

-   Configure your network interfaces to be secure.

See [[Security]](security/) for detailed information.
Important

Local access to Incus through the Unix socket always grants full access to Incus.
This includes the ability to attach file system paths or devices to any instance as well as tweak the security features on any instance.

Therefore, you should only give such access to users who you'd trust with root access to your system.
## Project and community

Incus is free software and developed under the [Apache 2 license](https://www.apache.org/licenses/LICENSE-2.0).
It's an open source project that warmly welcomes community projects, contributions, suggestions, fixes and constructive feedback.

-   [Code of Conduct](https://github.com/lxc/incus/blob/main/CODE_OF_CONDUCT.md)

-   [[Contribute to the project]](contributing/)

-   [Release announcements](https://discuss.linuxcontainers.org/c/news/13)

-   [Release tarballs](https://github.com/lxc/incus/releases/)

-   [[Get support]](support/)

-   [Watch tutorials and announcements on YouTube](https://www.youtube.com/@TheZabbly)

-   [Ask and answer questions on the forum](https://discuss.linuxcontainers.org)
