# [Ansible role wireguard](#wireguard)

description

|GitHub|Downloads|Version|
|------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-wireguard/actions/workflows/molecule.yml/badge.svg)](https://github.com/mullholland/ansible-role-wireguard/actions/workflows/molecule.yml)|[![downloads](https://img.shields.io/ansible/role/d/mullholland/wireguard)](https://galaxy.ansible.com/mullholland/wireguard)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-wireguard.svg)](https://github.com/mullholland/ansible-role-wireguard/releases/)|
## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-wireguard/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  # vars:
  #   example_var: "value"
  roles:
    - role: "mullholland.wireguard"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-wireguard/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true

  roles:
    - role: mullholland.repository_epel
    - role: mullholland.repository_elrepo
    - role: mullholland.repository_debian_backports
      when: ansible_distribution_major_version|int <= 10
```



## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-wireguard/blob/master/defaults/main.yml):

```yaml
---
# Wireguard configuration folder
wireguard_path: "/etc/wireguard"

# Overwrite docker/containerd service handling
wireguard_ignore_docker: false

# Use PreSharedKeys?
wireguard_presharedkeys: true

# Just needed once for the Server
# private/public keys will be used from play
wireguard_server_config:
  address: "10.0.0.1/24"  # Required
  port: 51820             # Required
  postup: "iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE"              # Optional
  postdown: "iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE"            # Optional

# Additional Server config, will be pasted line-by-line
# in the [Interface] block on the server
wireguard_server_additional_config: []
# wireguard_server_additional_config:
#   - "FwMark = Example"
#   - "Table = Sit"

# Additional peers on the wireguard_server
# whcih are not managed by this playbook
wireguard_manual_peers: []
# wireguard_manual_peers:
#   - name: wireguard_custom_host
#     config:
#       - ' PublicKey = cf6QQ0ORMZoOnodCg7RfpisolmBGnLkFRQaZXZpBBFs='
#       - 'AllowedIPs = 10.0.0.3/32'
#   - name: wireguard_custom_host
#     config:
#       - ' PublicKey = cf6QQ0ORMZoOnodCg7RfpisolmBGnLkFRQaZXZpBBFs='
#       - 'AllowedIPs = 10.0.0.4/32'

# Needed for each client
wireguard_peer_config:
  address: "10.0.0.2/24"                # Required
  server_allowedips: "10.0.0.2/32"      # Required
  client_allowedips: "0.0.0.0/0, ::/0"  # Required
  endpoint: "{{ wireguard_server }}"    # Optional

# Additional peer config, will be pasted line-by-line
# in the [peer] block on the peer
wireguard_peer_additional_config: []
# wireguard_peer_additional_config:
#   - "PersistentKeepalive = 30"
#   - "PresharedKey = Lg5BZHogOIfJ1tJFlMSEZysIfYLfjFU1lO1Kq1V2Ib4="
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-wireguard/blob/master/requirements.txt).


## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-wireguard/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/mullholland/enterpriselinux)|all|
|[Amazon](https://hub.docker.com/r/mullholland/amazonlinux)|Candidate|
|[Fedora](https://hub.docker.com/r/mullholland/fedora/)|all|
|[Ubuntu](https://hub.docker.com/r/mullholland/ubuntu)|all|
|[Debian](https://hub.docker.com/r/mullholland/debian)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-wireguard/issues).

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-wireguard/blob/master/LICENSE).

## [Author Information](#author-information)

[Mullholland](https://mullholland.net)
