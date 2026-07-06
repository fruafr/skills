# INCUS - CLI - Help

| **Command / Sub-command** | **Description** |
| - | - |
| [incus](./cli/incus.txt) | Command line client for Incus |
| [incus admin](./cli/incus_admin.txt) | Manage incus daemon |
| [incus cluster](./cli/incus_cluster.txt) | Manage cluster members |
| [incus config](./cli/incus_config.txt) | Manage instance and server configuration options |
| [incus console](./cli/incus_console.txt) | Attach to instance consoles |
| [incus copy](./cli/incus_copy.txt) | Copy instances within or in between servers |
| [incus create](./cli/incus_create.txt) | Create instances from images |
| [incus delete](./cli/incus_delete.txt) | Delete instances |
| [incus exec](./cli/incus_exec.txt) | Execute commands in instances |
| [incus export](./cli/incus_export.txt) | Export instances as backup tarballs. |
| [incus file](./cli/incus_file.txt) | Manage files in instances |
| [incus help](./cli/incus_help.txt) | Help provides help for any command in the application. |
| [incus image](./cli/incus_image.txt) | Manage images |
| [incus import](./cli/incus_import.txt) | Import backups of instances including their snapshots. |
| [incus info](./cli/incus_info.txt) | Show instance or server information |
| [incus launch](./cli/incus_launch.txt) | Create and start instances from images |
| [incus list](./cli/incus_list.txt) | List instances |
| [incus move](./cli/incus_move.txt) | Move instances within or in between servers |
| [incus network](./cli/incus_network.txt) | Manage and attach instances to networks |
| [incus pause](./cli/incus_pause.txt) | Pause instances |
| [incus profile](./cli/incus_profile.txt) | Manage profiles |
| [incus project](./cli/incus_project.txt) | Manage projects |
| [incus publish](./cli/incus_publish.txt) | Publish instances as images |
| [incus rebuild](./cli/incus_rebuild.txt) | Wipe the instance root disk and re-initialize with a new image (or empty volume). |
| [incus remote](./cli/incus_remote.txt) | Manage the list of remote servers |
| [incus rename](./cli/incus_rename.txt) | Rename instances |
| [incus restart](./cli/incus_restart.txt) | Restart instances |
| [incus resume](./cli/incus_resume.txt) | Resume instances |
| [incus snapshot](./cli/incus_snapshot.txt) | Manage instance snapshots |
| [incus start](./cli/incus_start.txt) | Start instances |
| [incus stop](./cli/incus_stop.txt) | Stop instances |
| [incus storage](./cli/incus_storage.txt) | Manage storage pools and volumes |
| [incus top](./cli/incus_top.txt) | Displays CPU usage, memory usage, and disk usage per instance |
| [incus version](./cli/incus_version.txt) | Show local and remote versions |
| [incus webui](./cli/incus_webui.txt) | Open the web interface |
| [incus admin cluster](./cli/incus_admin_cluster.txt) | Low level administration tools for inspecting and recovering clusters. |
| [incus admin init](./cli/incus_admin_init.txt) | Configure the daemon |
| [incus admin recover](./cli/incus_admin_recover.txt) | Recover missing instances and volumes from existing and unknown storage pools |
| [incus admin shutdown](./cli/incus_admin_shutdown.txt) | Tell the daemon to shutdown all instances and exit |
| [incus admin sql](./cli/incus_admin_sql.txt) | Execute a SQL query against the local or global database |
| [incus admin waitready](./cli/incus_admin_waitready.txt) | Wait for the daemon to be ready to process requests |
| [incus cluster add](./cli/incus_cluster_add.txt) | Request a join token for adding a cluster member |
| [incus cluster edit](./cli/incus_cluster_edit.txt) | Edit cluster member configurations as YAML |
| [incus cluster enable](./cli/incus_cluster_enable.txt) | Enable clustering on a single non-clustered server |
| [incus cluster evacuate](./cli/incus_cluster_evacuate.txt) | Evacuate cluster member |
| [incus cluster get](./cli/incus_cluster_get.txt) | Get values for cluster member configuration keys |
| [incus cluster group](./cli/incus_cluster_group.txt) | Manage cluster groups |
| [incus cluster info](./cli/incus_cluster_info.txt) | Show useful information about a cluster member |
| [incus cluster list](./cli/incus_cluster_list.txt) | List all the cluster members |
| [incus cluster list-tokens](./cli/incus_cluster_list-tokens.txt) | List all active cluster member join tokens |
| [incus cluster remove](./cli/incus_cluster_remove.txt) | Remove a member from the cluster |
| [incus cluster rename](./cli/incus_cluster_rename.txt) | Rename a cluster member |
| [incus cluster restore](./cli/incus_cluster_restore.txt) | Restore cluster member |
| [incus cluster revoke-token](./cli/incus_cluster_revoke-token.txt) | Revoke cluster member join token |
| [incus cluster role](./cli/incus_cluster_role.txt) | Manage cluster roles |
| [incus cluster set](./cli/incus_cluster_set.txt) | Set a cluster member's configuration keys |
| [incus cluster show](./cli/incus_cluster_show.txt) | Show details of a cluster member |
| [incus cluster unset](./cli/incus_cluster_unset.txt) | Unset a cluster member's configuration keys |
| [incus cluster update-certificate](./cli/incus_cluster_update-certificate.txt) | Update cluster certificate with PEM certificate and key read from input files. |
| [incus cluster group add](./cli/incus_cluster_group_add.txt) | Add a cluster member to a cluster group |
| [incus cluster group assign](./cli/incus_cluster_group_assign.txt) | Assign sets of groups to cluster members |
| [incus cluster group create](./cli/incus_cluster_group_create.txt) | Create a cluster group |
| [incus cluster group delete](./cli/incus_cluster_group_delete.txt) | Delete a cluster group |
| [incus cluster group edit](./cli/incus_cluster_group_edit.txt) | Edit a cluster group |
| [incus cluster group list](./cli/incus_cluster_group_list.txt) | List all the cluster groups |
| [incus cluster group remove](./cli/incus_cluster_group_remove.txt) | Remove a cluster member from a cluster group |
| [incus cluster group rename](./cli/incus_cluster_group_rename.txt) | Rename a cluster group |
| [incus cluster group show](./cli/incus_cluster_group_show.txt) | Show cluster group configurations |
| [incus cluster role add](./cli/incus_cluster_role_add.txt) | Add roles to a cluster member |
| [incus cluster role remove](./cli/incus_cluster_role_remove.txt) | Remove roles from a cluster member |
| [incus config device](./cli/incus_config_device.txt) | Manage devices |
| [incus config edit](./cli/incus_config_edit.txt) | Edit instance or server configurations as YAML |
| [incus config get](./cli/incus_config_get.txt) | Get values for instance or server configuration keys |
| [incus config metadata](./cli/incus_config_metadata.txt) | Manage instance metadata files |
| [incus config set](./cli/incus_config_set.txt) | Set instance or server configuration keys |
| [incus config show](./cli/incus_config_show.txt) | Show instance or server configurations |
| [incus config template](./cli/incus_config_template.txt) | Manage instance file templates |
| [incus config trust](./cli/incus_config_trust.txt) | Manage trusted clients |
| [incus config unset](./cli/incus_config_unset.txt) | Unset instance or server configuration keys |
| [incus config device add](./cli/incus_config_device_add.txt) | Add instance devices |
| [incus config device get](./cli/incus_config_device_get.txt) | Get values for device configuration keys |
| [incus config device list](./cli/incus_config_device_list.txt) | List instance devices |
| [incus config device override](./cli/incus_config_device_override.txt) | Copy profile inherited devices and override configuration keys |
| [incus config device remove](./cli/incus_config_device_remove.txt) | Remove instance devices |
| [incus config device set](./cli/incus_config_device_set.txt) | Set device configuration keys |
| [incus config device show](./cli/incus_config_device_show.txt) | Show full device configuration |
| [incus config device unset](./cli/incus_config_device_unset.txt) | Unset device configuration keys |
| [incus config metadata edit](./cli/incus_config_metadata_edit.txt) | Edit instance metadata files |
| [incus config metadata show](./cli/incus_config_metadata_show.txt) | Show instance metadata files |
| [incus config trust add](./cli/incus_config_trust_add.txt) | Add new trusted client |
| [incus config trust add-certificate](./cli/incus_config_trust_add-certificate.txt) | Add new trusted client certificate |
| [incus config trust edit](./cli/incus_config_trust_edit.txt) | Edit trust configurations as YAML |
| [incus config trust list](./cli/incus_config_trust_list.txt) | List trusted clients |
| [incus config trust list-tokens](./cli/incus_config_trust_list-tokens.txt) | List all active certificate add tokens |
| [incus config trust remove](./cli/incus_config_trust_remove.txt) | Remove trusted client |
| [incus config trust revoke-token](./cli/incus_config_trust_revoke-token.txt) | Revoke certificate add token |
| [incus config trust show](./cli/incus_config_trust_show.txt) | Show trust configurations |
| [incus file create](./cli/incus_file_create.txt) | Create files and directories in instances |
| [incus file delete](./cli/incus_file_delete.txt) | Delete files in instances |
| [incus file edit](./cli/incus_file_edit.txt) | Edit files in instances |
| [incus file mount](./cli/incus_file_mount.txt) | Mount files from instances |
| [incus file pull](./cli/incus_file_pull.txt) | Pull files from instances |
| [incus file push](./cli/incus_file_push.txt) | Push files into instances |
| [incus image alias](./cli/incus_image_alias.txt) | Manage image aliases |
| [incus image copy](./cli/incus_image_copy.txt) | Copy images between servers |
| [incus image delete](./cli/incus_image_delete.txt) | Delete images |
| [incus image edit](./cli/incus_image_edit.txt) | Edit image properties |
| [incus image export](./cli/incus_image_export.txt) | Export and download images |
| [incus image get-property](./cli/incus_image_get-property.txt) | Get image properties |
| [incus image import](./cli/incus_image_import.txt) | Import image into the image store |
| [incus image info](./cli/incus_image_info.txt) | Show useful information about images |
| [incus image list](./cli/incus_image_list.txt) | List images |
| [incus image refresh](./cli/incus_image_refresh.txt) | Refresh images |
| [incus image set-property](./cli/incus_image_set-property.txt) | Set image properties |
| [incus image show](./cli/incus_image_show.txt) | Show image properties |
| [incus image unset-property](./cli/incus_image_unset-property.txt) | Unset image properties |
| [incus image alias create](./cli/incus_image_alias_create.txt) | Create aliases for existing images |
| [incus image alias delete](./cli/incus_image_alias_delete.txt) | Delete image aliases |
| [incus image alias list](./cli/incus_image_alias_list.txt) | List image aliases |
| [incus image alias rename](./cli/incus_image_alias_rename.txt) | Rename aliases |
| [incus network acl](./cli/incus_network_acl.txt) | Manage network ACLs |
| [incus network attach](./cli/incus_network_attach.txt) | Attach new network interfaces to instances |
| [incus network attach-profile](./cli/incus_network_attach-profile.txt) | Attach network interfaces to profiles |
| [incus network create](./cli/incus_network_create.txt) | Create new networks |
| [incus network delete](./cli/incus_network_delete.txt) | Delete networks |
| [incus network detach](./cli/incus_network_detach.txt) | Detach network interfaces from instances |
| [incus network detach-profile](./cli/incus_network_detach-profile.txt) | Detach network interfaces from profiles |
| [incus network edit](./cli/incus_network_edit.txt) | Edit network configurations as YAML |
| [incus network forward](./cli/incus_network_forward.txt) | Manage network forwards |
| [incus network get](./cli/incus_network_get.txt) | Get values for network configuration keys |
| [incus network info](./cli/incus_network_info.txt) | Get runtime information on networks |
| [incus network integration](./cli/incus_network_integration.txt) | Manage network integrations |
| [incus network list](./cli/incus_network_list.txt) | List available networks |
| [incus network list-allocations](./cli/incus_network_list-allocations.txt) | List network allocations in use |
| [incus network list-leases](./cli/incus_network_list-leases.txt) | List DHCP leases |
| [incus network load-balancer](./cli/incus_network_load-balancer.txt) | Manage network load balancers |
| [incus network peer](./cli/incus_network_peer.txt) | Manage network peerings |
| [incus network rename](./cli/incus_network_rename.txt) | Rename networks |
| [incus network set](./cli/incus_network_set.txt) | Set network configuration keys |
| [incus network show](./cli/incus_network_show.txt) | Show network configurations |
| [incus network unset](./cli/incus_network_unset.txt) | Unset network configuration keys |
| [incus network zone](./cli/incus_network_zone.txt) | Manage network zones |
| [incus network acl create](./cli/incus_network_acl_create.txt) | Create new network ACLs |
| [incus network acl delete](./cli/incus_network_acl_delete.txt) | Delete network ACLs |
| [incus network acl edit](./cli/incus_network_acl_edit.txt) | Edit network ACL configurations as YAML |
| [incus network acl get](./cli/incus_network_acl_get.txt) | Get values for network ACL configuration keys |
| [incus network acl list](./cli/incus_network_acl_list.txt) | List available network ACL |
| [incus network acl rename](./cli/incus_network_acl_rename.txt) | Rename network ACLs |
| [incus network acl rule](./cli/incus_network_acl_rule.txt) | Manage network ACL rules |
| [incus network acl set](./cli/incus_network_acl_set.txt) | Set network ACL configuration keys |
| [incus network acl show](./cli/incus_network_acl_show.txt) | Show network ACL configurations |
| [incus network acl show-log](./cli/incus_network_acl_show-log.txt) | Show network ACL log |
| [incus network acl unset](./cli/incus_network_acl_unset.txt) | Unset network ACL configuration keys |
| [incus network acl rule add](./cli/incus_network_acl_rule_add.txt) | Add rules to an ACL |
| [incus network acl rule remove](./cli/incus_network_acl_rule_remove.txt) | Remove rules from an ACL |
| [incus network forward create](./cli/incus_network_forward_create.txt) | Create new network forwards |
| [incus network forward delete](./cli/incus_network_forward_delete.txt) | Delete network forwards |
| [incus network forward edit](./cli/incus_network_forward_edit.txt) | Edit network forward configurations as YAML |
| [incus network forward get](./cli/incus_network_forward_get.txt) | Get values for network forward configuration keys |
| [incus network forward list](./cli/incus_network_forward_list.txt) | List available network forwards |
| [incus network forward port](./cli/incus_network_forward_port.txt) | Manage network forward ports |
| [incus network forward set](./cli/incus_network_forward_set.txt) | Set network forward keys |
| [incus network forward show](./cli/incus_network_forward_show.txt) | Show network forward configurations |
| [incus network forward unset](./cli/incus_network_forward_unset.txt) | Unset network forward keys |
| [incus network forward port add](./cli/incus_network_forward_port_add.txt) | Add ports to a forward |
| [incus network forward port remove](./cli/incus_network_forward_port_remove.txt) | Remove ports from a forward |
| [incus network integration create](./cli/incus_network_integration_create.txt) | Create network integrations |
| [incus network integration delete](./cli/incus_network_integration_delete.txt) | Delete network integrations |
| [incus network integration edit](./cli/incus_network_integration_edit.txt) | Edit network integration configurations as YAML |
| [incus network integration get](./cli/incus_network_integration_get.txt) | Get values for network integration configuration keys |
| [incus network integration list](./cli/incus_network_integration_list.txt) | List network integrations |
| [incus network integration rename](./cli/incus_network_integration_rename.txt) | Rename network integrations |
| [incus network integration set](./cli/incus_network_integration_set.txt) | Set network integration configuration keys |
| [incus network integration show](./cli/incus_network_integration_show.txt) | Show network integration options |
| [incus network integration unset](./cli/incus_network_integration_unset.txt) | Unset network integration configuration keys |
| [incus profile add](./cli/incus_profile_add.txt) | Add profiles to instances |
| [incus profile assign](./cli/incus_profile_assign.txt) | Assign sets of profiles to instances |
| [incus profile copy](./cli/incus_profile_copy.txt) | Copy profiles |
| [incus profile create](./cli/incus_profile_create.txt) | Create profiles |
| [incus profile delete](./cli/incus_profile_delete.txt) | Delete profiles |
| [incus profile device](./cli/incus_profile_device.txt) | Manage devices |
| [incus profile edit](./cli/incus_profile_edit.txt) | Edit profile configurations as YAML |
| [incus profile get](./cli/incus_profile_get.txt) | Get values for profile configuration keys |
| [incus profile list](./cli/incus_profile_list.txt) | List profiles |
| [incus profile remove](./cli/incus_profile_remove.txt) | Remove profiles from instances |
| [incus profile rename](./cli/incus_profile_rename.txt) | Rename profiles |
| [incus profile set](./cli/incus_profile_set.txt) | Set profile configuration keys |
| [incus profile show](./cli/incus_profile_show.txt) | Show profile configurations |
| [incus profile unset](./cli/incus_profile_unset.txt) | Unset profile configuration keys |
| [incus profile device add](./cli/incus_profile_device_add.txt) | Add instance devices |
| [incus profile device get](./cli/incus_profile_device_get.txt) | Get values for device configuration keys |
| [incus profile device list](./cli/incus_profile_device_list.txt) | List instance devices |
| [incus profile device remove](./cli/incus_profile_device_remove.txt) | Remove instance devices |
| [incus profile device set](./cli/incus_profile_device_set.txt) | Set device configuration keys |
| [incus profile device show](./cli/incus_profile_device_show.txt) | Show full device configuration |
| [incus profile device unset](./cli/incus_profile_device_unset.txt) | Unset device configuration keys |
| [incus project create](./cli/incus_project_create.txt) | Create projects |
| [incus project delete](./cli/incus_project_delete.txt) | Delete projects |
| [incus project edit](./cli/incus_project_edit.txt) | Edit project configurations as YAML |
| [incus project get](./cli/incus_project_get.txt) | Get values for project configuration keys |
| [incus project get-current](./cli/incus_project_get-current.txt) | Show the current project |
| [incus project info](./cli/incus_project_info.txt) | Get a summary of resource allocations |
| [incus project list](./cli/incus_project_list.txt) | List projects |
| [incus project rename](./cli/incus_project_rename.txt) | Rename projects |
| [incus project set](./cli/incus_project_set.txt) | Set project configuration keys |
| [incus project show](./cli/incus_project_show.txt) | Show project options |
| [incus project switch](./cli/incus_project_switch.txt) | Switch the current project |
| [incus project unset](./cli/incus_project_unset.txt) | Unset project configuration keys |
| [incus remote add](./cli/incus_remote_add.txt) | Add new remote servers |
| [incus remote generate-certificate](./cli/incus_remote_generate-certificate.txt) | Manually trigger the generation of a client certificate |
| [incus remote get-default](./cli/incus_remote_get-default.txt) | Show the default remote |
| [incus remote list](./cli/incus_remote_list.txt) | List the available remotes |
| [incus remote proxy](./cli/incus_remote_proxy.txt) | Run a local API proxy for the remote |
| [incus remote remove](./cli/incus_remote_remove.txt) | Remove remotes |
| [incus remote rename](./cli/incus_remote_rename.txt) | Rename remotes |
| [incus remote set-url](./cli/incus_remote_set-url.txt) | Set the URL for the remote |
| [incus remote switch](./cli/incus_remote_switch.txt) | Switch the default remote |
| [incus snapshot create](./cli/incus_snapshot_create.txt) | Create instance snapshots |
| [incus snapshot delete](./cli/incus_snapshot_delete.txt) | Delete instance snapshots |
| [incus snapshot list](./cli/incus_snapshot_list.txt) | List instance snapshots |
| [incus snapshot rename](./cli/incus_snapshot_rename.txt) | Rename instance snapshots |
| [incus snapshot restore](./cli/incus_snapshot_restore.txt) | Restore instance from snapshots |
| [incus snapshot show](./cli/incus_snapshot_show.txt) | Show instance snapshot configuration |
| [incus storage bucket](./cli/incus_storage_bucket.txt) | Manage storage buckets. |
| [incus storage create](./cli/incus_storage_create.txt) | Create storage pools |
| [incus storage delete](./cli/incus_storage_delete.txt) | Delete storage pools |
| [incus storage edit](./cli/incus_storage_edit.txt) | Edit storage pool configurations as YAML |
| [incus storage get](./cli/incus_storage_get.txt) | Get values for storage pool configuration keys |
| [incus storage info](./cli/incus_storage_info.txt) | Show useful information about storage pools |
| [incus storage list](./cli/incus_storage_list.txt) | List available storage pools |
| [incus storage set](./cli/incus_storage_set.txt) | Set storage pool configuration keys |
| [incus storage show](./cli/incus_storage_show.txt) | Show storage pool configurations and resources |
| [incus storage unset](./cli/incus_storage_unset.txt) | Unset storage pool configuration keys |
| [incus storage volume](./cli/incus_storage_volume.txt) | Manage storage volumes |
| [incus storage bucket create](./cli/incus_storage_bucket_create.txt) | Create new custom storage buckets |
| [incus storage bucket delete](./cli/incus_storage_bucket_delete.txt) | Delete storage buckets |
| [incus storage bucket edit](./cli/incus_storage_bucket_edit.txt) | Edit storage bucket configurations as YAML |
| [incus storage bucket export](./cli/incus_storage_bucket_export.txt) | Export storage buckets as tarball. |
| [incus storage bucket get](./cli/incus_storage_bucket_get.txt) | Get values for storage bucket configuration keys |
| [incus storage bucket import](./cli/incus_storage_bucket_import.txt) | Import backups of storage buckets. |
| [incus storage bucket key](./cli/incus_storage_bucket_key.txt) | Manage storage bucket keys. |
| [incus storage bucket list](./cli/incus_storage_bucket_list.txt) | List storage buckets |
| [incus storage bucket set](./cli/incus_storage_bucket_set.txt) | Set storage bucket configuration keys |
| [incus storage bucket show](./cli/incus_storage_bucket_show.txt) | Show storage bucket configurations |
| [incus storage bucket unset](./cli/incus_storage_bucket_unset.txt) | Unset storage bucket configuration keys |
| [incus storage volume attach](./cli/incus_storage_volume_attach.txt) | Attach new custom storage volumes to instances |
| [incus storage volume attach-profile](./cli/incus_storage_volume_attach-profile.txt) | Attach new custom storage volumes to profiles |
| [incus storage volume copy](./cli/incus_storage_volume_copy.txt) | Copy custom storage volumes |
| [incus storage volume create](./cli/incus_storage_volume_create.txt) | Create new custom storage volumes |
| [incus storage volume delete](./cli/incus_storage_volume_delete.txt) | Delete custom storage volumes |
| [incus storage volume detach](./cli/incus_storage_volume_detach.txt) | Detach custom storage volumes from instances |
| [incus storage volume detach-profile](./cli/incus_storage_volume_detach-profile.txt) | Detach custom storage volumes from profiles |
| [incus storage volume edit](./cli/incus_storage_volume_edit.txt) | Edit storage volume configurations as YAML |
| [incus storage volume export](./cli/incus_storage_volume_export.txt) | Export custom storage volume |
| [incus storage volume get](./cli/incus_storage_volume_get.txt) | Get values for storage volume configuration keys |
| [incus storage volume import](./cli/incus_storage_volume_import.txt) | Import custom storage volumes. |
| [incus storage volume info](./cli/incus_storage_volume_info.txt) | Show storage volume state information |
| [incus storage volume list](./cli/incus_storage_volume_list.txt) | List storage volumes |
| [incus storage volume move](./cli/incus_storage_volume_move.txt) | Move custom storage volumes between pools |
| [incus storage volume rename](./cli/incus_storage_volume_rename.txt) | Rename custom storage volumes |
| [incus storage volume set](./cli/incus_storage_volume_set.txt) | Set storage volume configuration keys |
| [incus storage volume show](./cli/incus_storage_volume_show.txt) | Show storage volume configurations |
| [incus storage volume snapshot](./cli/incus_storage_volume_snapshot.txt) | Manage storage volume snapshots |
| [incus storage volume unset](./cli/incus_storage_volume_unset.txt) | Unset storage volume configuration keys |

## Global Flags
      --debug          Show all debug messages
      --force-local    Force using the local unix socket
  -h, --help           Print help
      --project        Override the source project
  -q, --quiet          Don't show progress information
      --sub-commands   Use with help or --help to view sub-commands
  -v, --verbose        Show all information messages
      --version        Print version number

      
