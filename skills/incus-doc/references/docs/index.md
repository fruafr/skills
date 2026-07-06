# Incus Documentation

> Generated from the sidebar tree of [linuxcontainers.org/incus/docs/main/](https://linuxcontainers.org/incus/docs/main/)

---

- [Incus](main.md)
- [Getting Started](tutorial/first_steps.md)

### General
- [General](general.md)
- [Containers and VMs](explanation/containers_and_vms.md)
- [Install Incus](installing.md)
- [Initialize Incus](howto/initialize.md)
- [Get support](support.md)
- [Frequently asked](faq.md)

### Client
- [Client overview](client.md)
- [Add remote servers](remotes.md)
- [Add command aliases](howto/incus_alias.md)
- [CLI configuration file](client-config.md)
- [Man pages](reference/manpages.md)
  - [`incus`](reference/manpages/incus.md)
    - [admin](reference/manpages/incus/incus_admin.md)
    - [alias](reference/manpages/incus/incus_alias.md)
    - [cluster](reference/manpages/incus/incus_cluster.md)
    - [config](reference/manpages/incus/incus_config.md)
    - [console](reference/manpages/incus/incus_console.md)
    - [copy](reference/manpages/incus/incus_copy.md)
    - [create](reference/manpages/incus/incus_create.md)
    - [debug](reference/manpages/incus/incus_debug.md)
    - [default](reference/manpages/incus/incus_default.md)
    - [delete](reference/manpages/incus/incus_delete.md)
    - [exec](reference/manpages/incus/incus_exec.md)
    - [export](reference/manpages/incus/incus_export.md)
    - [file](reference/manpages/incus/incus_file.md)
    - [image](reference/manpages/incus/incus_image.md)
    - [import](reference/manpages/incus/incus_import.md)
    - [info](reference/manpages/incus/incus_info.md)
    - [launch](reference/manpages/incus/incus_launch.md)
    - [list](reference/manpages/incus/incus_list.md)
    - [manpage](reference/manpages/incus/incus_manpage.md)
    - [monitor](reference/manpages/incus/incus_monitor.md)
    - [move](reference/manpages/incus/incus_move.md)
    - [network](reference/manpages/incus/incus_network.md)
    - [operation](reference/manpages/incus/incus_operation.md)
    - [pause](reference/manpages/incus/incus_pause.md)
    - [profile](reference/manpages/incus/incus_profile.md)
    - [project](reference/manpages/incus/incus_project.md)
    - [publish](reference/manpages/incus/incus_publish.md)
    - [query](reference/manpages/incus/incus_query.md)
    - [rebuild](reference/manpages/incus/incus_rebuild.md)
    - [remote](reference/manpages/incus/incus_remote.md)
    - [rename](reference/manpages/incus/incus_rename.md)
    - [restart](reference/manpages/incus/incus_restart.md)
    - [resume](reference/manpages/incus/incus_resume.md)
    - [snapshot](reference/manpages/incus/incus_snapshot.md)
    - [start](reference/manpages/incus/incus_start.md)
    - [stop](reference/manpages/incus/incus_stop.md)
    - [storage](reference/manpages/incus/incus_storage.md)
    - [top](reference/manpages/incus/incus_top.md)
    - [version](reference/manpages/incus/incus_version.md)
    - [wait](reference/manpages/incus/incus_wait.md)
    - [warning](reference/manpages/incus/incus_warning.md)
    - [webui](reference/manpages/incus/incus_webui.md)

### Server
- [Server overview](server.md)
- [Configure the server](howto/server_configure.md)
- [Server configuration](server_config.md)
- [System settings](reference/server_settings.md)
- [Backups](backup.md)
- [Performance tuning](explanation/performance_tuning.md)
- [Benchmarking](howto/benchmark_performance.md)
- [Monitor metrics](metrics.md)
- [Recover instances](howto/disaster_recovery.md)
- [Database](database.md)
- [Architectures](architectures.md)

### Instances
- [Instances overview](instances.md)
- [About instances](explanation/instances.md)
- [Create instances](howto/instances_create.md)
- [Manage instances](howto/instances_manage.md)
- [Configure instances](howto/instances_configure.md)
- [Back up instances](howto/instances_backup.md)
- [Use profiles](profiles.md)
- [Use cloud-init](cloud-init.md)
- [Run commands](instance-exec.md)
- [Access the console](howto/instances_console.md)
- [Access files](howto/instances_access_files.md)
- [Add a routed NIC to a VM](howto/instances_routed_nic_vm.md)
- [Troubleshoot errors](howto/instances_troubleshoot.md)
- [Instance configuration](explanation/instance_config.md)
  - [Instance properties](reference/instance_properties.md)
  - [Instance options](reference/instance_options.md)
  - [Devices](reference/devices.md)
    - [Standard devices](reference/standard_devices.md)
    - [Type: `none`](reference/devices_none.md)
    - [Type: `nic`](reference/devices_nic.md)
    - [Type: `disk`](reference/devices_disk.md)
    - [Type: `unix-char`](reference/devices_unix_char.md)
    - [Type: `unix-block`](reference/devices_unix_block.md)
    - [Type: `usb`](reference/devices_usb.md)
    - [Type: `gpu`](reference/devices_gpu.md)
    - [Type: `infiniband`](reference/devices_infiniband.md)
    - [Type: `proxy`](reference/devices_proxy.md)
    - [Type: `unix-hotplug`](reference/devices_unix_hotplug.md)
    - [Type: `tpm`](reference/devices_tpm.md)
    - [Type: `pci`](reference/devices_pci.md)
  - [Units for storage, memory and network limits](reference/instance_units.md)
- [Container environment](container-environment.md)
- [Migration](migration.md)
  - [Move instances](howto/move_instances.md)
  - [Import existing machines](howto/import_machines_to_instances.md)
  - [Migrate from LXC](howto/migrate_from_lxc.md)

### Storage
- [Storage overview](storage.md)
- [About storage](explanation/storage.md)
- [Manage pools](howto/storage_pools.md)
- [Create an instance in a pool](howto/storage_create_instance.md)
- [Manage volumes](howto/storage_volumes.md)
- [Move or copy a volume](howto/storage_move_volume.md)
- [Back up a volume](howto/storage_backup_volume.md)
- [Manage buckets](howto/storage_buckets.md)
- [Storage drivers](reference/storage_drivers.md)
  - [Directory - `dir`](reference/storage_dir.md)
  - [Btrfs - `btrfs`](reference/storage_btrfs.md)
  - [LVM - `lvm`](reference/storage_lvm.md)
  - [ZFS - `zfs`](reference/storage_zfs.md)
  - [Ceph RBD - `ceph`](reference/storage_ceph.md)
  - [CephFS - `cephfs`](reference/storage_cephfs.md)
  - [Ceph Object - `cephobject`](reference/storage_cephobject.md)
  - [LINSTOR - `linstor`](reference/storage_linstor.md)
    - [Setup LINSTOR](howto/storage_linstor_setup.md)
    - [Driver internals](reference/storage_linstor_internals.md)
  - [TrueNAS - `truenas`](reference/storage_truenas.md)

### Networks
- [Networks overview](networks.md)
- [About networking](explanation/networks.md)
- [Create and configure a network](howto/network_create.md)
- [Configure a network](howto/network_configure.md)
- [Configure network ACLs](howto/network_acls.md)
- [Configure network address sets](howto/network_address_sets.md)
- [Configure network forwards](howto/network_forwards.md)
- [Configure network integrations](howto/network_integrations.md)
- [Configure network zones](howto/network_zones.md)
- [Configure Incus as BGP server](howto/network_bgp.md)
- [Display Incus IPAM information](howto/network_ipam.md)
- [Bridge network](reference/network_bridge.md)
  - [Integrate with resolved](howto/network_bridge_resolved.md)
  - [Configure your firewall](howto/network_bridge_firewalld.md)
- [OVN network](reference/network_ovn.md)
  - [Set up OVN](howto/network_ovn_setup.md)
  - [Create routing relationships](howto/network_ovn_peers.md)
  - [Configure network load balancers](howto/network_load_balancers.md)
- [External networks](reference/network_external.md)
  - [Macvlan network](reference/network_macvlan.md)
  - [SR-IOV network](reference/network_sriov.md)
  - [Physical network](reference/network_physical.md)
- [Increase bandwidth](howto/network_increase_bandwidth.md)

### Images
- [Images overview](images.md)
- [About images](image-handling.md)
- [Use remote images](howto/images_remote.md)
- [Manage images](howto/images_manage.md)
- [Copy and import images](howto/images_copy.md)
- [Create images](howto/images_create.md)
- [Associate profiles](howto/images_profiles.md)
- [Image format](reference/image_format.md)
- [Default image server](reference/image_servers.md)

### Projects
- [Projects overview](projects.md)
- [About projects](explanation/projects.md)
- [Create and configure projects](howto/projects_create.md)
- [Work with different projects](howto/projects_work.md)
- [Confine projects to users](howto/projects_confine.md)
- [Project configuration](reference/projects.md)

### Clustering
- [Clustering overview](clustering.md)
- [About clustering](explanation/clustering.md)
- [Form a cluster](howto/cluster_form.md)
- [Access a cluster](howto/cluster_access.md)
- [Manage a cluster](howto/cluster_manage.md)
- [Recover a cluster](howto/cluster_recover.md)
- [Manage cluster groups](howto/cluster_groups.md)
- [Manage instances](howto/cluster_manage_instance.md)
- [Configure storage](howto/cluster_config_storage.md)
- [Configure networks](howto/cluster_config_networks.md)
- [Cluster member configuration](reference/cluster_member_config.md)

### API (REST API)
- [API overview](api.md)
- [Main API documentation](rest-api.md)
- [Main API specification](rest-api-spec.md)
- [Main API extensions](api-extensions.md)
- [Instance API documentation](dev-incus.md)
- [Events API documentation](events.md)
- [Metrics API documentation](reference/provided_metrics.md)

### Security
- [Security overview](security.md)
- [About security](explanation/security.md)
- [BPF token delegation](explanation/bpf-tokens.md)
- [Remote API authentication](authentication.md)
- [Authorization](authorization.md)
- [Expose Incus to the network](howto/server_expose.md)

### Internals
- [Internals overview](internals.md)
- [Daemon behavior](daemon-behavior.md)
- [Debug Incus](debugging.md)
- [Requirements](requirements.md)
- [Packaging recommendations](packaging.md)
- [Environment variables](environment.md)
- [System call interception](syscall-interception.md)
- [User namespace setup](userns-idmap.md)

### Contributing
- [Contributing overview](contributing.md)
- [Introduction](contributing/introduction.md)
- [Contribute to the code](contributing/code.md)
- [Contribute to the documentation](contributing/docs.md)

### External resources
- [External resources overview](external_resources.md)
- [Project repository](https://github.com/lxc/incus)
- [Image server](https://images.linuxcontainers.org)
- [Third party tools](third_party.md)

---

**Local docs structure:**
```
📁 .
├── 📄 main.md
├── 📁 tutorial/
│   └── 📄 first_steps.md
├── 📁 explanation/
│   ├── bpf-tokens.md
│   ├── clustering.md
│   ├── containers_and_vms.md
│   ├── instance_config.md
│   ├── instances.md
│   ├── networks.md
│   ├── performance_tuning.md
│   ├── projects.md
│   ├── security.md
│   └── storage.md
├── 📁 howto/
│   ├── benchmark_performance.md
│   ├── cluster_access.md
│   ├── cluster_config_networks.md
│   ├── cluster_config_storage.md
│   ├── cluster_form.md
│   ├── cluster_groups.md
│   ├── cluster_manage_instance.md
│   ├── cluster_manage.md
│   ├── cluster_recover.md
│   ├── disaster_recovery.md
│   ├── images_copy.md
│   ├── images_create.md
│   ├── images_manage.md
│   ├── images_profiles.md
│   ├── images_remote.md
│   ├── import_machines_to_instances.md
│   ├── incus_alias.md
│   ├── initialize.md
│   ├── instances_access_files.md
│   ├── instances_backup.md
│   ├── instances_configure.md
│   ├── instances_console.md
│   ├── instances_create.md
│   ├── instances_manage.md
│   ├── instances_routed_nic_vm.md
│   ├── instances_troubleshoot.md
│   ├── migrate_from_lxc.md
│   ├── move_instances.md
│   ├── network_acls.md
│   ├── network_address_sets.md
│   ├── network_bgp.md
│   ├── network_bridge_firewalld.md
│   ├── network_bridge_resolved.md
│   ├── network_configure.md
│   ├── network_create.md
│   ├── network_forwards.md
│   ├── network_increase_bandwidth.md
│   ├── network_integrations.md
│   ├── network_ipam.md
│   ├── network_load_balancers.md
│   ├── network_ovn_peers.md
│   ├── network_ovn_setup.md
│   ├── network_zones.md
│   ├── projects_confine.md
│   ├── projects_create.md
│   ├── projects_work.md
│   ├── server_configure.md
│   ├── server_expose.md
│   ├── storage_backup_volume.md
│   ├── storage_buckets.md
│   ├── storage_create_instance.md
│   ├── storage_linstor_setup.md
│   ├── storage_move_volume.md
│   ├── storage_pools.md
│   └── storage_volumes.md
├── 📁 reference/
│   ├── cluster_member_config.md
│   ├── devices.md
│   ├── devices_disk.md
│   ├── devices_gpu.md
│   ├── devices_infiniband.md
│   ├── devices_nic.md
│   ├── devices_none.md
│   ├── devices_pci.md
│   ├── devices_proxy.md
│   ├── devices_tpm.md
│   ├── devices_unix_block.md
│   ├── devices_unix_char.md
│   ├── devices_unix_hotplug.md
│   ├── devices_usb.md
│   ├── image_format.md
│   ├── image_servers.md
│   ├── incus.md
│   ├── instance_options.md
│   ├── instance_properties.md
│   ├── instance_units.md
│   ├── manpages.md
│   ├── network_bridge.md
│   ├── network_external.md
│   ├── network_macvlan.md
│   ├── network_ovn.md
│   ├── network_physical.md
│   ├── network_sriov.md
│   ├── projects.md
│   ├── provided_metrics.md
│   ├── server_settings.md
│   ├── standard_devices.md
│   ├── storage_btrfs.md
│   ├── storage_ceph.md
│   ├── storage_cephfs.md
│   ├── storage_cephobject.md
│   ├── storage_dir.md
│   ├── storage_drivers.md
│   ├── storage_linstor_internals.md
│   ├── storage_linstor.md
│   ├── storage_lvm.md
│   ├── storage_truenas.md
│   ├── storage_zfs.md
│   └── 📁 manpages/
│       └── 📁 incus/
│           ├── incus_admin.md
│           ├── incus_alias.md
│           ├── incus_cluster.md
│           ├── incus_config.md
│           ├── incus_console.md
│           ├── incus_copy.md
│           ├── incus_create.md
│           ├── incus_debug.md
│           ├── incus_default.md
│           ├── incus_delete.md
│           ├── incus_exec.md
│           ├── incus_export.md
│           ├── incus_file.md
│           ├── incus_image.md
│           ├── incus_import.md
│           ├── incus_info.md
│           ├── incus_launch.md
│           ├── incus_list.md
│           ├── incus_manpage.md
│           ├── incus_monitor.md
│           ├── incus_move.md
│           ├── incus_network.md
│           ├── incus_operation.md
│           ├── incus_pause.md
│           ├── incus_profile.md
│           ├── incus_project.md
│           ├── incus_publish.md
│           ├── incus_query.md
│           ├── incus_rebuild.md
│           ├── incus_remote.md
│           ├── incus_rename.md
│           ├── incus_restart.md
│           ├── incus_resume.md
│           ├── incus_snapshot.md
│           ├── incus_start.md
│           ├── incus_stop.md
│           ├── incus_storage.md
│           ├── incus_top.md
│           ├── incus_version.md
│           ├── incus_wait.md
│           ├── incus_warning.md
│           └── incus_webui.md
├── 📁 contributing/
│   ├── introduction.md
│   ├── code.md
│   └── docs.md
├── api-extensions.md
├── api.md
├── architectures.md
├── authentication.md
├── authorization.md
├── backup.md
├── client-config.md
├── client.md
├── cloud-init.md
├── clustering.md
├── container-environment.md
├── contributing.md
├── daemon-behavior.md
├── database.md
├── debugging.md
├── dev-incus.md
├── environment.md
├── events.md
├── external_resources.md
├── faq.md
├── general.md
├── image-handling.md
├── images.md
├── installing.md
├── instance-exec.md
├── instances.md
├── internals.md
├── metrics.md
├── migration.md
├── networks.md
├── packaging.md
├── profiles.md
├── projects.md
├── remotes.md
├── requirements.md
├── rest-api-spec.md
├── rest-api.md
├── security.md
├── server_config.md
├── server.md
├── storage.md
├── support.md
├── syscall-interception.md
├── third_party.md
├── userns-idmap.md
└── 📄 index.md
```
