# ufw

This role installs and configures [Uncomplicated Firewall][ufw] (UFW). This role performs basic
configuration only, such as opening and closing ports. For more fine-grained control of UFW, use the
[UFW Ansible module][ufw-ansible] directly.

## Variables

- `ufw_closed_ports` -- A list of ports on which to deny traffic.

- `ufw_default_policy` -- The default firewall policy: `allow`, `deny`, or `reject`.

- `ufw_firewall_state` -- The firewall [state][ufw-ansible]: `enabled`, `disabled`, `reloaded`, or
`reset`.

- `ufw_limited_ports` -- A list of ports on which to allow traffic and rate-limit connections.

- `ufw_open_ports` -- A list of ports on which to allow traffic.

- `ufw_rejected_ports` -- A list of ports on which to reject traffic.

## Example configuration

```
# Open port 22 and rate-limit connections
ufw_limited_ports: [22]

# Open webserver ports
ufw_open_ports: [80, 443]
```

## Notes

Use caution when manipulating firewall rules. If the port used by your ssh connection is closed,
you may be locked out of the server. To reduce the risk of lock-out, this role opens ports
specified by `ufw_open_ports` and `ufw_limited_ports` before setting the default policy and
enabling the firewall.


[ufw]: https://wiki.ubuntu.com/UncomplicatedFirewall
[ufw-ansible]: https://docs.ansible.com/ansible/ufw_module.html
