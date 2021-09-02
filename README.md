# Homelab

```diff
! ⚠️ WORK IN PROGRESS
```

## Hardware

![Hardware](https://user-images.githubusercontent.com/27996771/98970963-25137200-2543-11eb-8f2d-f9a2d45756ef.JPG)

- 4 nodes of NEC SFF `PC-MK26ECZDR` (Japanese version of the ThinkCentre M700):
  - CPU: `Intel Core i5-6600T @ 2.70GHz`
  - RAM: `16GB`
  - SSD: `128GB`
- TP-Link `TL-SG108` switch:
  - Ports: `8`
  - Speed: `1000Mbps`

## Overview

![Provisioning flow](https://khuedoan.github.io/homelab/diagrams/provisioning_flow.jpg)

A single `make` command will automatically:

- Build the `./metal` layer:
  - Create an ephemeral, stateless PXE server
  - Install Linux on all servers in parallel
  - Build a Kubernetes cluster (based on k3s)
- Build the `./bootstrap` layer:
  - Install ArgoCD
  - Kustomize creates the [root Argo application](./bootstrap/root-app) that will install other Argo applications

From now on, the root app will install the remaining layers (`./system`, `./apps`) and the `./bootstrap` layer will manage itself.

Please visit the [Provisioning flow document](https://khuedoan.github.io/homelab/deployment/provisioning_flow.html) to learn more.

## Get Started

### Harware requirements

Any modern `x86_64` computer(s) should work, you can use old PCs, laptops or servers.
A total of 3 or more nodes is recommended for high availability.

To view the detailed requirements, please visit the [Hareware requirements document](https://khuedoan.github.io/homelab/deployment/hardware_requirements.html).

### Prerequisite

For the controller (your laptop or desktop):

- SSH keys in `~/.ssh/{id_ed25519,id_ed25519.pub}` (you can generate it with `ssh-keygen -t ed25519`)
- Docker with `host` networking driver (which means [only Docker on Linux hosts](https://docs.docker.com/network/host/), you can use a Linux virtual machine with bridged networking if you're on macOS or Windows)

For bare metal nodes:

- PXE IPv4 enabled
- Wake-on-LAN enabled and boot to network mode by default if turned on via Wake-on-LAN
- Secure boot disabled (optional, depending on the OS)
- Note their MAC addresses

To view the detailed instruction, please visit the [Prerequisite document](https://khuedoan.github.io/homelab/deployment/prerequisite.html).

### Configurations

Change these configuration files to match your hardware and network setup:

- [Bare metal nodes settings](./metal/hosts.yml) (IP, MAC...)
- [OS settings](./metal/group_vars/all.yml) (PXE, network...)

To view the detailed instruction, please visit the [Configuration document](https://khuedoan.github.io/homelab/deployment/configuration.html).

### Build

You can install all the tools manually, or you can use the convenience tools container:

```sh
make tools
```

Then build the homelab:

```sh
make
```

If you encounter any issue, please visit [Trouble shooting guide](https://khuedoan.github.io/homelab/troubleshooting/README.html)

## Roadmap

See [roadmap](https://khuedoan.github.io/homelab/roadmap.html) and [open issues](https://github.com/khuedoan/homelab/issues) for a list of proposed features and known issues.

## Contributing

Any contributions you make are greatly appreciated (feature, bug fixes, documentation, grammar or typo fix...).

## License

Distributed under the GPLv3 License. See `LICENSE` for more information.

## Technology stack

<table>
  <tr>
    <td align="center"><a><img src="https://simpleicons.org/icons/ansible.svg" width="50px;"/><br/>Ansible</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/cloudflare.svg" width="50px;"/><br/>Cloudflare</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/docker.svg" width="50px;"/><br/>Docker</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/fedora.svg" width="50px;"/><br/>Fedora</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/gitea.svg" width="50px;"/><br/>Gitea</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/helm.svg" width="50px;"/><br/>Helm</td>
  </tr>
  <tr>
    <td align="center"><a><img src="https://simpleicons.org/icons/kubernetes.svg" width="50px;"/><br/>Kubernetes</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/prometheus.svg" width="50px;"/><br/>Prometheus</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/rancher.svg" width="50px;"/><br/>Rancher</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/terraform.svg" width="50px;"/><br/>Terraform</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/vault.svg" width="50px;"/><br/>Vault</td>
    <td align="center"><a><img src="https://simpleicons.org/icons/wireguard.svg" width="50px;"/><br/>Wireguard</td>
  </tr>
  <tr>
  </tr>
</table>

## Acknowledgements

- ArgoCD usage in [my coworker's homelab](https://github.com/locmai/humble)
- [README template](https://github.com/othneildrew/Best-README-Template)
- [Run the same Cloudflare Tunnel across many `cloudflared` processes](https://developers.cloudflare.com/cloudflare-one/tutorials/many-cfd-one-tunnel)
- [MAC address environment variable in GRUB config](https://askubuntu.com/questions/1272400/how-do-i-automate-network-installation-of-many-ubuntu-18-04-systems-with-efi-and)
- [Official k3s systemd service file](https://github.com/k3s-io/k3s/blob/master/k3s.service)
