# Ansible Role: iSCSI

This role will install and configure the iSCSI initiator and iSCSI targets.

Dedicated iSCSI interfaces will be placed in a separate Firewalld zone which will drop all
incoming new connections, allowing only outgoing connections.

## Requirements

This role has to be executed as user 'root'.

Following external roles are used:

  * [linux-system-roles.firewall](https://github.com/linux-system-roles/firewall)

## Variables

| Variable                        | Type   | Required | Default | Comment                                        |
|---------------------------------|--------|----------|---------|------------------------------------------------|
| iscsi_interfaces                | list() | No       | []      | Dedicated iSCSI network interfaces             |
| iscsi_portal                    | string | Yes      | N/A     | Domain name or IP address of the iSCSI targets |
| iscsi_targets                   | list() | Yes      | N/A     | iSCSI target names                             |

**Note:** The optional 'iscsi_interfaces' variable is only used to configure Firewalld for the dedicated iSCSI interfaces.

### Targets

The iSCSI targets always need to have a username and password for CHAP authentication.

```yaml
# Dedicated iSCSI interfaces
iscsi_interfaces:
  - ens1f0
  - ens1f1

# iSCSI Target
iscsi_portal: 172.16.0.0
iscsi_targets:
  - target: iqn.2000-01.com.synology:nas.default-target.19707b26383
    user: <USERNAME>
    password: <PASSWORD>
```

## Management

### Open-iSCSI Administrator Utility

The 'iscsiadm' command line tool can be used to display information about the active iSCSI sessions.

```shell
root # iscsiadm -m session -P 0
tcp: [1] 172.16.0.0:3260,1 iqn.2000-01.com.synology:nas.default-target.19707b26383 (non-flash)
tcp: [2] 172.16.1.0:3260,1 iqn.2000-01.com.synology:nas.default-target.19707b26383 (non-flash)
```

The '-P' parameter defines the printlevel (0-4). 
