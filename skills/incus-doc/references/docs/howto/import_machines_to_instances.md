# How to import physical or virtual machines to Incus instances

Incus provides a tool (`incus-migrate`) to create an Incus instance or volume based on an existing disk or image.

You can run the tool on any Linux machine. It connects to a local or remote Incus server and creates a blank instance or volume, which you can configure during or after the migration. The tool then copies the data from the disk or image that you provide.

`incus-migrate` can import images in `raw`, `qcow2`, `ova` and `vmdk` file formats. It can also directly download an image from a provided URL.

Note

If you want to configure a new instance during the migration process, set up the entities that you want your instance to use before starting the migration process.

By default, the new instance will use the entities specified in the `default` profile. You can specify a different profile (or a profile list) to customize the configuration. See [How to use profiles](../../profiles/#profiles) for more information. You can also override [Instance options](../../reference/instance_options/#instance-options), the [storage pool](../../explanation/storage/#storage-pools) to be used and the size for the [storage volume](../../explanation/storage/#storage-volumes), and the [network](../../networks/#networking) to be used.

Alternatively, you can update the instance configuration after the migration is complete.

When working with instances, the tool can create both containers and virtual machines:

-   When creating a container, you must provide a disk or partition that contains the root file system for the container. For example, this could be the `/` root disk of the machine or container where you are running the tool.

-   When creating a virtual machine, you must provide a bootable disk, partition or image. This means that just providing a file system is not sufficient, and you cannot create a virtual machine from a container that you are running. It is also not possible to create a virtual machine from the physical machine that you are using to do the migration, because the migration tool would be using the disk that it is copying. Instead, you could provide a bootable image, or a bootable partition or disk that is currently not in use.

    Tip

    If you want to convert a Windows VM from a foreign hypervisor (not from QEMU/KVM with Q35/`virtio-scsi`), you must install the `virtio-win` drivers to your Windows. Otherwise, your VM won't boot.

    Expand to see how to integrate the required drivers to your Windows VM Install the required tools on the host:

    1.  Install `virt-v2v` version >= 2.3.4 (this is the minimal version that supports the `--block-driver` option).

    2.  Install the `virtio-win` package, or download the [`virtio-win.iso`](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso) image and put it into the `/usr/share/virtio-win` folder.

    3.  You might also need to install [`rhsrvany`](https://github.com/rwmjones/rhsrvany).

    Now you can use `virt-v2v` to convert images from a foreign hypervisor to `raw` images for Incus and include the required drivers:

        # Example 1. Convert a vmdk disk image to a raw image suitable for incus-migrate
        sudo virt-v2v --block-driver virtio-scsi -o local -of raw -os ./os -i vmx ./test-vm.vmx
        # Example 2. Convert a QEMU/KVM qcow2 image and integrate virtio-scsi driver
        sudo virt-v2v --block-driver virtio-scsi -o local -of raw -os ./os -if qcow2 -i disk test-vm-disk.qcow2

    You can find the resulting image in the `os` directory and use it with `incus-migrate` on the next steps.

Complete the following steps to migrate an existing machine to an Incus instance:

1.  Download the `bin.linux.incus-migrate` tool ([`bin.linux.incus-migrate.aarch64`](https://github.com/lxc/incus/releases/latest/download/bin.linux.incus-migrate.aarch64) or [`bin.linux.incus-migrate.x86_64`](https://github.com/lxc/incus/releases/latest/download/bin.linux.incus-migrate.x86_64)) from the **Assets** section of the latest [Incus release](https://github.com/lxc/incus/releases).

2.  Place the tool on the machine that you want to use to create the instance. Make it executable (usually by running `chmod u+x bin.linux.incus-migrate`).

3.  Make sure that the machine has `rsync` installed. If it is missing, install it (for example, with `sudo apt install rsync`).

4.  Run the tool:

        sudo ./bin.linux.incus-migrate

    The tool then asks you to provide the information required for the migration.

    Tip

    As an alternative to running the tool interactively, you can provide the configuration as parameters to the command. See `./bin.linux.incus-migrate --help` for more information.

    1.  Specify the Incus server URL, either as an IP address or as a DNS name.

        Note

        The Incus server must be [exposed to the network](../server_expose/#server-expose). If you want to import to a local Incus server, you must still expose it to the network. You can then specify `127.0.0.1` as the IP address to access the local server.

    2.  Check and confirm the certificate fingerprint.

    3.  Choose a method for authentication (see [Remote API authentication](../../authentication/#authentication)).

        For example, if you choose using a certificate token, log on to the Incus server and create a token for the machine on which you are running the migration tool with [`incus config trust add`](../../reference/manpages/incus/config/trust/add/#incus-config-trust-add-md). Then use the generated token to authenticate the tool.

    4.  Choose whether to create a container or a virtual machine. See [About containers and VMs](../../explanation/containers_and_vms/#containers-and-vms).

    5.  Specify a name for the instance that you are creating.

    6.  Provide the path to a root file system (for containers) or a bootable disk, partition or image file (for virtual machines).

    7.  For containers, optionally add additional file system mounts.

    8.  For virtual machines, specify whether secure boot is supported.

    9.  Optionally, configure the new instance. You can do so by specifying [profiles](../../profiles/#profiles), directly setting [configuration options](../../reference/instance_options/#instance-options) or changing [storage](../../storage/#storage) or [network](../../networks/#networking) settings.

        Alternatively, you can configure the new instance after the migration.

    10.  When you are done with the configuration, start the migration process.

    Expand to see an example output for importing to a container

        ~$ sudo ./bin.linux.incus-migrate
        The local Incus server is the target [default=yes]:
        What would you like to create?
        1) Container
        2) Virtual Machine
        3) Virtual Machine (from .ova)
        4) Custom Volume
        Please enter the number of your choice: 1
        Name of the new instance: foo
        Please provide the path to a root filesystem: /
        Do you want to add additional filesystem mounts? [default=no]:
        Instance to be created:
          Name: foo
          Project: default
          Type: container
          Source: /
        Additional overrides can be applied at this stage:
        1) Begin the migration with the above configuration
        2) Override profile list
        3) Set additional configuration options
        4) Change instance storage pool or volume size
        5) Change instance network
        Please pick one of the options above [default=1]: 3
        Please specify config keys and values (key=value ...): limits.cpu=2
        Instance to be created:
          Name: foo
          Project: default
          Type: container
          Source: /
          Config:
            limits.cpu: "2"
        Additional overrides can be applied at this stage:
        1) Begin the migration with the above configuration
        2) Override profile list
        3) Set additional configuration options
        4) Change instance storage pool or volume size
        5) Change instance network
        Please pick one of the options above [default=1]: 4
        Please provide the storage pool to use: default
        Do you want to change the storage size? [default=no]: yes
        Please specify the storage size: 20GiB
        Instance to be created:
          Name: foo
          Project: default
          Type: container
          Source: /
          Storage pool: default
          Storage pool size: 20GiB
          Config:
            limits.cpu: "2"
        Additional overrides can be applied at this stage:
        1) Begin the migration with the above configuration
        2) Override profile list
        3) Set additional configuration options
        4) Change instance storage pool or volume size
        5) Change instance network
        Please pick one of the options above [default=1]: 5
        Please specify the network to use for the instance: incusbr0
        Instance to be created:
          Name: foo
          Project: default
          Type: container
          Source: /
          Storage pool: default
          Storage pool size: 20GiB
          Network name: incusbr0
          Config:
            limits.cpu: "2"
        Additional overrides can be applied at this stage:
        1) Begin the migration with the above configuration
        2) Override profile list
        3) Set additional configuration options
        4) Change instance storage pool or volume size
        5) Change instance network
        Please pick one of the options above [default=1]: 1
        Instance foo successfully created

    Expand to see an example output for importing to a VM

        ~$ sudo ./bin.linux.incus-migrate
        The local Incus server is the target [default=yes]:
        What would you like to create?
        1) Container
        2) Virtual Machine
        3) Virtual Machine (from .ova)
        4) Custom Volume
        Please enter the number of your choice: 2
        Name of the new instance: foo
        Please provide the path to a root filesystem: ./virtual-machine.img
        Does the VM support UEFI Secure Boot? [default=no]: no
        Instance to be created:
          Name: foo
          Project: default
          Type: virtual-machine
          Source: ./virtual-machine.img
          Config:
            security.secureboot: "false"
        Additional overrides can be applied at this stage:
        1) Begin the migration with the above configuration
        2) Override profile list
        3) Set additional configuration options
        4) Change instance storage pool or volume size
        5) Change instance network
        Please pick one of the options above [default=1]: 3
        Please specify config keys and values (key=value ...): limits.cpu=2
        Instance to be created:
          Name: foo
          Project: default
          Type: virtual-machine
          Source: ./virtual-machine.img
          Config:
            limits.cpu: "2"
            security.secureboot: "false"
        Additional overrides can be applied at this stage:
        1) Begin the migration with the above configuration
        2) Override profile list
        3) Set additional configuration options
        4) Change instance storage pool or volume size
        5) Change instance network
        Please pick one of the options above [default=1]: 4
        Please provide the storage pool to use: default
        Do you want to change the storage size? [default=no]: yes
        Please specify the storage size: 20GiB
        Instance to be created:
          Name: foo
          Project: default
          Type: virtual-machine
          Source: ./virtual-machine.img
          Storage pool: default
          Storage pool size: 20GiB
          Config:
            limits.cpu: "2"
            security.secureboot: "false"
        Additional overrides can be applied at this stage:
        1) Begin the migration with the above configuration
        2) Override profile list
        3) Set additional configuration options
        4) Change instance storage pool or volume size
        5) Change instance network
        Please pick one of the options above [default=1]: 5
        Please specify the network to use for the instance: incusbr0
        Instance to be created:
          Name: foo
          Project: default
          Type: virtual-machine
          Source: ./virtual-machine.img
          Storage pool: default
          Storage pool size: 20GiB
          Network name: incusbr0
          Config:
            limits.cpu: "2"
            security.secureboot: "false"
        Additional overrides can be applied at this stage:
        1) Begin the migration with the above configuration
        2) Override profile list
        3) Set additional configuration options
        4) Change instance storage pool or volume size
        5) Change instance network
        Please pick one of the options above [default=1]: 1
        Instance foo successfully created

5.  When the migration is complete, check the new instance and update its configuration to the new environment. Typically, you must update at least the storage configuration (`/etc/fstab`) and the network configuration.
