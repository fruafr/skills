[]{#cloud-init}

# How to use [`cloud-init`]

[`cloud-init`](https://cloud-init.io/) is a tool for automatically initializing and customizing an instance of a Linux distribution.

By adding [`cloud-init`] configuration to your instance, you can instruct [`cloud-init`] to execute specific actions at the first start of an instance.
Possible actions include, for example:

-   Updating and installing packages

-   Applying certain configurations

-   Adding users

-   Enabling services

-   Running commands or scripts

-   Automatically growing the file system of a VM to the size of the disk

See the [Cloud-init documentation](https://docs.cloud-init.io/en/latest/index.html#index "(in cloud-init v26.1)") for detailed information.

**Note**

The [`cloud-init`] actions are run only once on the first start of the instance.
Rebooting the instance does not re-trigger the actions.

## [`cloud-init`] support in images

To use [`cloud-init`], you must base your instance on an image that has [`cloud-init`] installed.

Images from the [[`images`] remote](https://images.linuxcontainers.org/) have [`cloud-init`]-enabled variants, which are usually bigger in size than the default variant.
The cloud variants use the [`/cloud`] suffix, for example, [`images:debian/12/cloud`].

## [`cloud-init`] and virtual machines

For [`cloud-init`] to work inside of a virtual machine, you need to either have a functional [`incus-agent`] in the VM or need to provide the [`cloud-init`] data through a special extra disk.
All images coming from the [`images:`] remote will have the agent already setup and so are good to go from the start.

For instances that do not have the [`incus-agent`], you can pass in the extra [`cloud-init`] disk with:

    incus config device add INSTANCE cloud-init disk source=cloud-init:config

**Note**

When relying on the [`incus-agent`] to apply the [`cloud-init`] configuration, the instance needs to have a set of file templates in place for:

-   [`/var/lib/cloud/seed/nocloud-net/meta-data`]

-   [`/var/lib/cloud/seed/nocloud-net/network-config`]

-   [`/var/lib/cloud/seed/nocloud-net/user-data`]

-   [`/var/lib/cloud/seed/nocloud-net/vendor-data`]

When making a new image, it's easiest to replicate those from one of our own [`cloud-init`] enabled images.

Those file templates get applied by the [`incus-agent`] on first boot. The
agent then triggers a reboot, causing [`cloud-init`] to run cleanly and
apply the configuration to the virtual machine.

## Configuration options

Incus supports two different sets of configuration options for configuring [`cloud-init`]: [`cloud-init.*`] and [`user.*`].
Which of these sets you must use depends on the [`cloud-init`] support in the image that you use.
As a rule of thumb, newer images support the [`cloud-init.*`] configuration options, while older images support [`user.*`].
However, there might be exceptions to that rule.

The following configuration options are supported:

-   [`cloud-init.vendor-data`] or [`user.vendor-data`] (see [Vendor-data](https://docs.cloud-init.io/en/latest/explanation/vendordata.html#vendor-data "(in cloud-init v26.1)"))

-   [`cloud-init.user-data`] or [`user.user-data`] (see [User-data formats](https://docs.cloud-init.io/en/latest/explanation/format/index.html#user-data-formats "(in cloud-init v26.1)"))

-   [`cloud-init.network-config`] or [`user.network-config`] (see [Network configuration](https://docs.cloud-init.io/en/latest/reference/network-config.html#network-config "(in cloud-init v26.1)"))

For more information about the configuration options, see the [[[`cloud-init`] instance options]](../reference/instance_options/#instance-options-cloud-init).

### Vendor data and user data

Both [`vendor-data`] and [`user-data`] are used to provide cloud configuration data to [`cloud-init`].

The main idea is that [`vendor-data`] is used for the general default configuration, while [`user-data`] is used for instance-specific configuration.
This means that you should specify [`vendor-data`] in a profile and [`user-data`] in the instance configuration.
Incus does not enforce this method, but allows using both [`vendor-data`] and [`user-data`] in profiles and in the instance configuration.

If both [`vendor-data`] and [`user-data`] are supplied for an instance, [`cloud-init`] merges the two configurations.
However, if you use the same keys in both configurations, merging might not be possible.
In this case, configure how [`cloud-init`] should merge the provided data.
See the [cloud-init documentation](https://docs.cloud-init.io/en/latest/reference/merging.html#merging-user-data "(in cloud-init v26.1)") for instructions.

## How to configure [`cloud-init`]

To configure [`cloud-init`] for an instance, add the corresponding configuration options to a [profile](../profiles/#profiles) that the instance uses or directly to the [instance configuration](../howto/instances_configure/#instances-configure).

When configuring [`cloud-init`] directly for an instance, keep in mind that [`cloud-init`] runs only on the first start of the instance.
That means that you must configure [`cloud-init`] before you start the instance.
To do so, create the instance with [[[`incus`]` `[`init`]]](../reference/manpages/incus/create/#incus-create-md) instead of [[[`incus`]` `[`launch`]]](../reference/manpages/incus/launch/#incus-launch-md), and then start it after completing the configuration.

### YAML format for [`cloud-init`] configuration

The [`cloud-init`] options require YAML's [literal style format](https://spec.yaml.io/main/spec/1.2.2/#812-literal-style).
You use a pipe symbol ([`|`]) to indicate that all indented text after the pipe should be passed to [`cloud-init`] as a single string, with new lines and indentation preserved.

The [`vendor-data`] and [`user-data`] options usually start with [`#cloud-config`].

For example:

    config:
      cloud-init.user-data: |
        #cloud-config
        package_upgrade: true
        packages:
          - package1
          - package2

**Tip**

See [How to validate user data](https://docs.cloud-init.io/en/latest/howto/debug_user_data.html#check-user-data-cloud-config "(in cloud-init v26.1)") for information on how to check whether the syntax is correct.

## How to check the [`cloud-init`] status

[`cloud-init`] runs automatically on the first start of an instance.
Depending on the configured actions, it might take a while until it finishes.

To check the [`cloud-init`] status, log on to the instance and enter the following command:

    cloud-init status

If the result is [`status:`]` `[`running`], [`cloud-init`] is still working. If the result is [`status:`]` `[`done`], it has finished.

Alternatively, use the [`--wait`] flag to be notified only when [`cloud-init`] is finished:

[`root@instance:~#`]` `[`cloud-init`]` `[`status`]` `[`--wait`]

[`.....................................`][`status:`]` `[`done`]

## How to specify user or vendor data

The [`user-data`] and [`vendor-data`] configuration can be used to, for example, upgrade or install packages, add users, or run commands.

The provided values must have a first line that indicates what type of [user data format](https://docs.cloud-init.io/en/latest/explanation/format/index.html#user-data-formats "(in cloud-init v26.1)") is being passed to [`cloud-init`].
For activities like upgrading packages or setting up a user, [`#cloud-config`] is the data format to use.

The configuration data is stored in the following files in the instance's root file system:

-   [`/var/lib/cloud/instance/cloud-config.txt`]

-   [`/var/lib/cloud/instance/user-data.txt`]

### Examples

See the following sections for the user data (or vendor data) configuration for different example use cases.

You can find more advanced [examples](https://docs.cloud-init.io/en/latest/reference/examples.html#yaml-examples "(in cloud-init v26.1)") in the [`cloud-init`] documentation.

#### Upgrade packages

To trigger a package upgrade from the repositories for the instance right after the instance is created, use the [`package_upgrade`] key:

    config:
      cloud-init.user-data: |
        #cloud-config
        package_upgrade: true

#### Install packages

To install specific packages when the instance is set up, use the [`packages`] key and specify the package names as a list:

    config:
      cloud-init.user-data: |
        #cloud-config
        packages:
          - git
          - openssh-server

#### Set the time zone

To set the time zone for the instance on instance creation, use the [`timezone`] key:

    config:
      cloud-init.user-data: |
        #cloud-config
        timezone: Europe/Rome

#### Run commands

To run a command (such as writing a marker file), use the [`runcmd`] key and specify the commands as a list:

    config:
      cloud-init.user-data: |
        #cloud-config
        runcmd:
          - [touch, /run/cloud.init.ran]

#### Add a user account

To add a user account, use the [`users`] key.
See the [Including users and groups](https://docs.cloud-init.io/en/latest/reference/examples.html#including-users-and-groups "(in cloud-init v26.1)") example in the [`cloud-init`] documentation for details about default users and which keys are supported.

    config:
      cloud-init.user-data: |
        #cloud-config
        users:
          - name: documentation_example

## How to specify network configuration data

By default, [`cloud-init`] configures a DHCP client on an instance's [`eth0`] interface.
You can define your own network configuration using the [`network-config`] option to override the default configuration (this is due to how the template is structured).

[`cloud-init`] then renders the relevant network configuration on the system using either [`ifupdown`] or [`netplan`], depending on the distribution.

The configuration data is stored in the following files in the instance's root file system:

-   [`/var/lib/cloud/seed/nocloud-net/network-config`]

-   [`/etc/network/interfaces.d/50-cloud-init.cfg`] (if using [`ifupdown`])

-   [`/etc/netplan/50-cloud-init.yaml`] (if using [`netplan`])

### Example

To configure a specific network interface with a static IPv4 address and also use a custom name server, use the following configuration:

    config:
      cloud-init.network-config: |
        version: 2
        ethernets:
          eth1:
            addresses:
              - 10.10.101.20/24
            gateway4: 10.10.101.1
            nameservers:
              addresses:
                - 10.10.10.254
