# Ansible Role: Harden

An ansible role to harden linux servers.

## Requirements

> This role has been tested on `Ubuntu 16.04` and `Ubuntu 16.10` only.

## Configuration Actions

### SSH

Locks down SSH connections by disabling password and root login as well as SSH over `ipv6`.

#### Variables

- `harden_ssh_disable_password_login`: password ssh login should be disabled.
  - Default: `true`
    > **Note**: ssh keys for login must be setup first. Else you will be locked out of the host.

- `harden_ssh_disable_root_login`: Indicate if root ssh login should be disabled.
  - Default: `true`
    > **Note**: users other than root must be created first. Else you will be locked out of the host.

- `harden_ssh_disable_ipv6`: disable ssh connectionns over ipv6.
  - Default: `true`

- `harden_ssh_disable_tcp_forwarding`: disable tcp forwarding.
  - Default: `true`


### Firewall

Enables the firewall and blocks all connections except ssh from allowed network subnets.

#### Variables

- `harden_firewall_ssh_allowed_ips`: list of ip addresses to allow ssh access from.
  - Default: `[ '10.0.0.0/8', '172.16.0.0/12', '192.168.0.0/16' ]`
  > **Note**: can be a subnet. private ips are allowed by default.

> #### Caveat
> This action is not idempotent and is very destructive. Each run will rebuild the firewall rules and erase any exising rules.
>
> Only run when setting up a server for the first time. Otherwise, rerun playbooks to properly setup firewall on affected hosts.


### TCP Wrapper

Disables all tcp connections via tcp wrapper, enabling all localhost connections and ssh connections from private network subnets only.

#### Variables

- `harden_tcp_wrapper_ssh_allowed_ips`: list of ip addresses to allow ssh access from.
  - Default: `[ '10.0.0.1/255.0.0.0', '172.16.0.0/255.240.0.0', '192.168.0.0/255.255.0.0' ]`
  > **Note**: can be a subnet. private ips are allowed by default.
  >
  > `/etc/hosts.allow` uses a different format for subnets


> #### Caveat
> Software that use tcp wrappers are affected by this directive.
>
> You will need to **remember** to allow connections in the hosts file as well as the  firewall.



## Usage Example

```yaml
- hosts: all
  vars:
    harden_firewall_ssh_allowed_ips:
      - 192.168.1.10
  roles:
    - thedumbtechguy.harden
```


## License

MIT / BSD

## Author Information

This role was created by [TheDumbTechGuy](https://github.com/thedumbtechguy) ( [twitter](https://twitter.com/frostymarvelous) | [blog](https://thedumbtechguy.blogspot.com) | [galaxy](https://galaxy.ansible.com/thedumbtechguy/) )

## Credits