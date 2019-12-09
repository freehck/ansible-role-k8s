freehck.k8s
=========

Install kubernetes cluster

Description
-----------

This role installs kubernetes cluster. Full automation: installs all the binaries, initializes cluster, join nodes to cluster, installs CNI.

This role is a dummy one, and it uses other simple roles from Galaxy.

Role Variables
--------------

#### Common cluster configuration

##### Global

`k8s_ver`: Kubernetes version to install (default is `1.16.2-00`)

`k8s_pod_network_cidr`: CIDR your pod network will use (default is `192.168.0.0/16`)


##### Unique per host

`k8s_node_ip`: ip address of the node (no default value, must be set explicitly)

`k8s_node_name`: node name (default is `{{ inventory_hostname }}`


##### Special

`k8s_is_master`: this flag must be set to `true` only for kubernetes master node, so do it in `host_vars` or in your `inventory` (default is `false`)


##### Calico CNI

`k8s_enable_calico`: flag enabling Calico CNI (default is `true`)

`k8s_calico_ver`: Calico CNI's version to install (default is `v3.10`)


##### Flannel CNI

`k8s_enable_flannel`: flag enabling Flannel CNI (default is `false`)

`k8s_flannel_ver`: Flannel CNI's version to install (default is `master-2019-12-09`)

`k8s_flannel_iface`: Flannel CNI's interface for nodes inter-communications, not set by default


Example
-------

###### inventory

    k8s-node-0 ansible_host=10.118.19.10 k8s_is_master=true
    k8s-node-1 ansible_host=10.118.19.11
    k8s-node-2 ansible_host=10.118.19.12
    
    [k8s_cluster]
    k8s-node-0
    k8s-node-1
    k8s-node-2

###### group_vars/k8s_cluster.yml

    ---
    k8s_ver: "1.16.2-00"
    k8s_node_ip: "{{ ansible_host }}"
    k8s_pod_network_cidr: "192.168.0.0/16"
    k8s_enable_calico: true
    k8s_calico_ver: "v3.10"

###### playbook.yml

    - hosts: k8s_cluster
      become: true
      roles:
        - role: freehck.k8s

Tests
-----

This role provides you with a Vagrant-based test suite under `tests/` directory.

To get a fully operational Kubernetes cluster, type following command in terminal:

    cd tests && make

Install
-------

This role can be installed from [Ansible Galaxy](https://galaxy.ansible.com/):

`ansible-galaxy install freehck.k8s`

License
-------

MIT

Author Information
------------------

Dmitrii Kashin, <freehck@freehck.ru>
