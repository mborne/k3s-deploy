# OIDC experimentation

## Configure keycloak

* Create a "kubernetes" client
* Add "groups" property for this client "Client Scopes / kubernetes-dedicated" :

![keycloak group mapping](img/keycloak-group-mapping.png)

## Configure K3S to enable OIDC

Add the following arguments to `k3s_installer_opts` in [inventory/vagrantbox/group_vars/k3s_master/k3s.yml](../inventory/vagrantbox/group_vars/k3s_master/k3s.yml) :

```bash
--kube-apiserver-arg oidc-issuer-url=https://keycloak.quadtreeworld.net/realms/master
--kube-apiserver-arg oidc-client-id=kubernetes
--kube-apiserver-arg oidc-groups-claim=groups
```

## Configure kubectl

```bash
# install oidc-login plugin
kubectl krew install oidc-login

# configurer oidc-login
kubectl oidc-login setup \
  --oidc-issuer-url=https://keycloak.quadtreeworld.net/realms/master \
  --oidc-client-id=kubernetes \
  --oidc-client-secret=SecretFromKeycloak

# login in browser and follow instructions...
```

```bash
# adapt ClusterRoleBinding to bind group
kubectl create clusterrolebinding oidc-cluster-admin --clusterrole=cluster-admin --group='k8s_admin'
```

## Ressources

* [kubernetes.io - Authenticating - Configuring the API Server](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#configuring-the-api-server)

```bash
--oidc-issuer-url=https://keycloak.quadtreeworld.net/realms/master
--oidc-client-id=kubernetes
--oidc-groups-claim=group
```

* [docs.k3s.io - Customized Flags for Kubernetes Processes](https://docs.k3s.io/reference/server-config#customized-flags-for-kubernetes-processes)

> `--kube-apiserver-arg`

* [github.com - int128/kubelogin](https://github.com/int128/kubelogin)

