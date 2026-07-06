# Devices

Devices are attached to an instance (see [Configure devices](../../howto/instances_configure/#instances-configure-devices)) or to a profile (see [Edit a profile](../../profiles/#profiles-edit)).

They include, for example, network interfaces, mount points, USB and GPU devices. These devices can have instance device options, depending on the type of the instance device.

Incus supports the following device types:

| ID (database) | Name | Condition | Description |
|---|---|---|---|
| 0 | [`none`](../devices_none/#devices-none) | - | Inheritance blocker |
| 1 | [`nic`](../devices_nic/#devices-nic) | - | Network interface |
| 2 | [`disk`](../devices_disk/#devices-disk) | - | Mount point inside the instance |
| 3 | [`unix-char`](../devices_unix_char/#devices-unix-char) | container | Unix character device |
| 4 | [`unix-block`](../devices_unix_block/#devices-unix-block) | container | Unix block device |
| 5 | [`usb`](../devices_usb/#devices-usb) | - | USB device |
| 6 | [`gpu`](../devices_gpu/#devices-gpu) | - | GPU device |
| 7 | [`infiniband`](../devices_infiniband/#devices-infiniband) | container | InfiniBand device |
| 8 | [`proxy`](../devices_proxy/#devices-proxy) | container | Proxy device |
| 9 | [`unix-hotplug`](../devices_unix_hotplug/#devices-unix-hotplug) | container | Unix hotplug device |
| 10 | [`tpm`](../devices_tpm/#devices-tpm) | - | TPM device |
| 11 | [`pci`](../devices_pci/#devices-pci) | VM | PCI device |

Each instance comes with a set of [Standard devices](../standard_devices/#standard-devices).
