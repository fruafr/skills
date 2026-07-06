# Instance properties

Instance properties are set when the instance is created. They cannot be part of a [profile](../../profiles/#profiles).

The following instance properties are available:

| Property | Read-only | Description |
|---|---|---|
| `architecture` | no | Instance architecture |
| `created_at` | yes | Timestamp of the instance creation |
| `description` | no | User provided description of the instance |
| `ephemeral` | no | Whether the instance is ephemeral (gets deleted when stopped) |
| `last_used_at` | yes | Timestamp of the instance last usage |
| `location` | no | Current location of the instance within a cluster |
| `name` | yes | Instance name (see Instance name requirements) |
| `project` | yes | Project that the instance is a part of |
| `stateful` | yes | Whether saved runtime state is currently present |
| `status` | yes | Human readable status of the instance |
| `status_code` | yes | Machine readable status of the instance |
| `type` | yes | Type of instance (container or virtual machine) |

## Instance name requirements

The instance name can be changed only by renaming the instance with the [`incus rename`](../manpages/incus/rename/#incus-rename-md) command.

Valid instance names must fulfill the following requirements:

- The name must be between 1 and 63 characters long.
- The name must contain only letters, numbers and dashes from the ASCII table.
- The name must not start with a digit or a dash.
- The name must not end with a dash.
