
# Step by step deployment

## Install K3S on master node

```bash
ansible-playbook -i inventory/vagrantbox playbooks/k3s-master.yml
```

## Configure kubectl

```bash
export KUBECONFIG=$PWD/output/kubeconfig.yaml
```

## Check kubectl config

```bash
kubectl cluster-info
kubectl get nodes -o wide
```

## Wait for master to be ready

```bash
kubectl wait --all-namespaces=true --for=condition=Ready pods  --timeout=60s --all
watch kubectl get nodes -o wide
```

## Install K3S on agent nodes

```bash
ansible-playbook -i inventory playbooks/k3s-agent.yml
```

