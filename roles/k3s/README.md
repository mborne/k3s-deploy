# ansible-role-k3s

This ansible role allows to deploy a [K3S](https://k3s.io/) cluster with a single master and multiple worker nodes. **It is not intented to be used for production.**


## Parameters

See [defaults/main.yml](defaults/main.yml) :

* `k3s_channel` allows to configure **K8S version** (ex : `"v1.29"`)

* `k3s_docker_mirror` allows to **configure a proxy for DockerHub** (docker.io) at containerd level.

* `k3s_flannel_iface` allows to select the good parent network for [Flannel](https://docs.k3s.io/networking/basic-network-options) :
  * `"enp0s8"` with VirtualBox and ubuntu/jammy64
  * `"eth1"` with KVM and generic/ubuntu2204


## How it works?

Ansible allows to handle parameters to customize the installation method from the [Quick Start guide](https://docs.k3s.io/quick-start) :

```bash
curl -sfL https://get.k3s.io | sh -s -
```

See steps in [tasks/main.yml](tasks/main.yml) :

* [tasks/disable-swap.yml](tasks/disable-swap.yml)
* [tasks/install-requirements.yml](tasks/install-requirements.yml)
* [tasks/configure-registries.yml](tasks/configure-registries.yml) generates `/etc/rancher/k3s/registries.yaml` using [templates/etc/rancher/k3s/registries.yaml.j2](templates/etc/rancher/k3s/registries.yaml.j2) to configure docker.io mirror at containerd level.
* [tasks/build-installer-opts.yml](tasks/build-installer-opts.yml) converts ansible options in 

curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=mynodetoken sh -


## Ressources

* [docs.k3s.io - Quick-Start Guide](https://docs.k3s.io/quick-start)
* [stackoverflow.com - Is there any way to bind K3s / flannel to another interface?](https://stackoverflow.com/questions/66449289/is-there-any-way-to-bind-k3s-flannel-to-another-interface/66495119#66495119)
