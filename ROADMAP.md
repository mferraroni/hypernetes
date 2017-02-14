# Hypernetes roadmap of v1.6

Hypernetes v1.6.0 is planning to extract its core logic out so as not to touch the core kubernetes code. It this way, we could avoid rebasing nightmare and focus on core hypernetes features.

> From next release (v1.6.0), hypernetes will keep its release version same with kubernetes. That's why we don't have v1.4 and v1.5 releases.

## Auth

### Authentication

Hypernetes v1.6 will continue to auth by keystone, it is already part of upstream kubernetes. Check [here](https://kubernetes.io/docs/admin/authentication/#keystone-password) for more information.

### Authorization

Hypernetes v1.6 will use [RBAC (Role-Based Access Control)](https://kubernetes.io/docs/admin/authorization/) for authorization.

An extra controller (auth-controller) will be added for managing Roles, RolesBindings, ClusterRoles, and ClusterRoleBindings for user-created resources:

- Auth regular users to namespaces and their namespace scoped resources
- Auth admins to both namespaced and non-namespaced resources

## Container Runtime

Hypernetes v1.6 will swith to [frakti](https://github.com/kubernetes/frakti), which is also based on hypercontainer:

- frakti will be released at same time with kubernetes v1.6
- A stable hyperd release is expected before that

## Network

Hypernetes v1.6 will continue to support multi-tenant networking, but its network object will be moved to kubernetes [third party resource](https://kubernetes.io/docs/user-guide/thirdpartyresources/). [kubestack](https://github.com/hyperhq/kubestack) will watch those resource and manage networks in OpenStack Neutron.

Kubestack will be split into two parts:

- kubestack-controller, which watches network objects in third party resource and manage network/routes in OpenStack Neutron.
- kubestack-cni, which serves as an CNI network plugin for frakti and connects hypercontainer to OpenStack Neutron

Note that network is not namespace scoped. It could be assoicated with multiple namespaces. All pods belonging to same namespace share same network, but different namespaces may be associated with different networks.

## Volume

Hypernetes changed kubelet volume plugin interface to get volume metadata in current implementations. It passes the metadata directly to runtime instead of mounting the volume on the host. Hypernetes v1.6 will archive this by using [Flexvolume](https://github.com/kubernetes/kubernetes/blob/master/examples/volumes/flexvolume/README.md). An extra flex volume driver `cinder` will be introduced in charge of volume management.

## Client

Hypernetes v1.6 will use `kubectl` (same as kubernetes) as its client.

## Issues

**Upgrade from an old version**

Hypernetes v1.6 doesn't support online upgrading.
