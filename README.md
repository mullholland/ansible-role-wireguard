# [wireguard](#wireguard)

|GitHub|GitLab|
|------|------|
|[![github](https://github.com/mullholland/ansible-role-wireguard/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-wireguard/actions)|[![gitlab](https://gitlab.com/mullholland/ansible-role-wireguard/badges/main/pipeline.svg)](https://gitlab.com/mullholland/ansible-role-wireguard)|

description

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
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


## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.
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

The machine needs to be prepared in CI this is done using `molecule/default/prepare.yml`:
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





## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

-   [debian10](https://hub.docker.com/r/mullholland/docker-molecule-debian10)
-   [debian11](https://hub.docker.com/r/mullholland/docker-molecule-debian11)
-   [ubuntu1804](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu1804)
-   [ubuntu2004](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu2004)
-   [ubuntu2204](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu2204)
-   [centos7](https://hub.docker.com/r/mullholland/docker-molecule-centos7)
-   [centos-stream8](https://hub.docker.com/r/mullholland/docker-molecule-centos-stream8)
-   [fedora35](https://hub.docker.com/r/mullholland/docker-molecule-fedora35)
-   [fedora36](https://hub.docker.com/r/mullholland/docker-molecule-fedora36)
-   [rockylinux8](https://hub.docker.com/r/mullholland/docker-molecule-rockylinux8)
-   [almalinux8](https://hub.docker.com/r/mullholland/docker-molecule-almalinux8)

The minimum version of Ansible required is 2.10, tests have been done to:

-   The previous versions.
-   The current version.





If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-wireguard/issues)

## [License](#license)

MIT


## [Author Information](#author-information)

[Mullholland](https://github.com/mullholland)

## [Special Thanks](#special-thanks)

Template inspired by [Robert de Bock](https://github.com/robertdebock)
