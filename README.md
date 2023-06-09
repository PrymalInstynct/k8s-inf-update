K8s Infrastructure Update
=========================

This role will apply Operating System patches to your k8s infrastructure nodes using a rolling methodology to ensure the k8s cluster remains active. You can include multiple clusters in your inventory supplying a different kubeconfig as a variable for each of the clusters.

Requirements
------------

A Kubernetes Cluster running on a Fedora, Red Hat, or Ubuntu based distrobution

### Collections

  - kubernetes.core

Role Variables
--------------

```yaml
---
kubeconfig_location: ~/.kube/config
drain_delete_emptydir_data: true
drain_ignore_daemonsets: true
drain_terminate_grace_period: 60
drain_wait_timeout: 120
```

Example Inventory
-----------------

```yaml
---
prod_kubernetes:
  children:
    control:
      hosts:
        k8s-control-prod-0:
          ansible_host: 10.10.1.10
          kubeconfig_location: ~/Projects/prod-k8s-cluster/kubeconfig
        k8s-control-prod-1:
          ansible_host: 10.10.1.11
          kubeconfig_location: ~/Projects/prod-k8s-cluster/kubeconfig
        k8s-control-prod-2:
          ansible_host: 10.10.1.12
          kubeconfig_location: ~/Projects/prod-k8s-cluster/kubeconfig
    worker:
      hosts:
        k8s-worker-prod-3:
          ansible_host: 10.10.1.13
          kubeconfig_location: ~/Projects/prod-k8s-cluster/kubeconfig
        k8s-worker-prod-4:
          ansible_host: 10.10.1.14
          kubeconfig_location: ~/Projects/prod-k8s-cluster/kubeconfig
dev_kubernetes:
  children:
    control:
      hosts:
        k8s-control-dev-0:
          ansible_host: 10.10.111.211
          kubeconfig_location: ~/Projects/dev-k8s-cluster/kubeconfig
        k8s-control-dev-1:
          ansible_host: 10.10.111.228
          kubeconfig_location: ~/Projects/dev-k8s-cluster/kubeconfig
        k8s-control-dev-2:
          ansible_host: 10.10.111.235
          kubeconfig_location: ~/Projects/dev-k8s-cluster/kubeconfig
```

Example Playbook
----------------

`ansible-playbook -i inventory.yml playbooks/update-k8s-infrastructure.yml `

```yaml
---
- name: Update Kubernetes Infrastructure Nodes & Perform a rolling reboot
  hosts: all
  serial: 1
  any_errors_fatal: "{{ any_errors_fatal | default(true) }}"
  roles:
    - { role: k8s-inf-update }
```

License
-------

MIT

Author Information
------------------

[Chad Zimmerman](https://github.com/PrymalInstynct)
