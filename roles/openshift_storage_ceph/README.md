OpenShift ceph Cluster
===========================

OpenShift ceph Cluster Configuration

This role handles the configuration of ceph clusters by deploying an initial single instance of a Ceph monitor and manager.
Once that's up and running, operators will have to interact with the Ceph CLI to further pursue the deployment.

Requirements
------------

* Ansible 2.2

Host Groups
-----------

The following group is expected to be populated for this role to run:

* `[ceph]`

Role Variables
--------------

This role has the following variables that control the integration of a ceph cluster:

| Name                                             | Default value           | Description                             |
|--------------------------------------------------|-------------------------|-----------------------------------------|
| openshift_storage_ceph_namespace                 | 'ceph'                  | Namespace/project in which to create ceph resources
| openshift_storage_ceph_name                      | 'storage'               | A name to identify the ceph cluster, which will be used in resource names
| openshift_storage_ceph_nodeselector              | 'ceph=storage-host'     | Selector to determine which nodes will host ceph pods in native mode. **NOTE:** The label value is taken from the cluster name
| openshift_storage_ceph_use_default_selector      | False                   | Whether to use a default node selector for the ceph namespace/project. If False, the namespace/project will have no restricting node selector. If True, uses pre-existing or default (e.g. osm_default_node_selector) node selectors. **NOTE:** If True, nodes which will host ceph pods must already have the additional labels.
| openshift_storage_ceph_image                     | 'ceph/daemon'           | Container image to use for ceph pods, enterprise default is 'rhcs2/rhcs-server-rhel7'
| openshift_storage_ceph_version                   | 'latest'                | Container image version to use for ceph pods
| openshift_storage_ceph_wipe                      | False                   | Destroy any existing ceph resources


Dependencies
------------

* os_firewall
* openshift_hosted_facts
* openshift_repos
* lib_openshift

Example Playbook
----------------

```
- name: configure ceph hosts
  hosts: oo_first_master
  roles:
  - role: openshift_storage_ceph
    when: groups.oo_ceph_to_config | default([]) | count > 0
```

License
-------

Apache License, Version 2.0

Author Information
------------------

Sebastien Han (seb@redhat.com)
