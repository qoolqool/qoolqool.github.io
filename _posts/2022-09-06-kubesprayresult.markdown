---
layout: post
title:  "Kubespray Howto"
date:   2022-09-06 12:59:00 +0800
categories: [linux,ansible,k8s]
---
<details><summary>
K8S Cluster Installation</summary>
<pre><code>
(venv) $ cat inventory/mycluster/hosts.yaml
all:
  vars:
    ansible_user: admin
  hosts:
    node1:
      ansible_host: 192.168.88.211
      ip: 192.168.88.211
      access_ip: 192.168.88.211
    node2:
      ansible_host: 192.168.88.212
      ip: 192.168.88.212
      access_ip: 192.168.88.212
    node3:
      ansible_host: 192.168.88.213
      ip: 192.168.88.213
      access_ip: 192.168.88.213
  children:
    kube_control_plane:
      hosts:
        node1:
    kube_node:
      hosts:
        node1:
        node2:
        node3:
    etcd:
      hosts:
        node1:
    k8s_cluster:
      children:
        kube_control_plane:
        kube_node:
    calico_rr:
      hosts: {}
(venv) $ ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml

PLAY [localhost] ******************************************************************************************************
Monday 05 September 2022  13:55:39 +0000 (0:00:00.020)       0:00:00.020 ******

TASK [Check 2.9.0 <= Ansible version < 2.12.0] ************************************************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:55:39 +0000 (0:00:00.022)       0:00:00.043 ******

TASK [Check Ansible version > 2.10.11 when using ansible 2.10] ********************************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:55:39 +0000 (0:00:00.021)       0:00:00.065 ******

TASK [Check that python netaddr is installed] *************************************************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:55:39 +0000 (0:00:00.032)       0:00:00.097 ******

TASK [Check that jinja is not too old (install via pip)] **************************************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}
[WARNING]: Could not match supplied host pattern, ignoring: kube-master

PLAY [Add kube-master nodes to kube_control_plane] ********************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: kube-node

PLAY [Add kube-node nodes to kube_node] *******************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: k8s-cluster

PLAY [Add k8s-cluster nodes to k8s_cluster] ***************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: calico-rr

PLAY [Add calico-rr nodes to calico_rr] *******************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: no-floating

PLAY [Add no-floating nodes to no_floating] ***************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: bastion

PLAY [bastion[0]] *****************************************************************************************************
skipping: no hosts matched

PLAY [k8s_cluster:etcd] ***********************************************************************************************
Monday 05 September 2022  13:55:39 +0000 (0:00:00.055)       0:00:00.153 ******
Monday 05 September 2022  13:55:39 +0000 (0:00:00.040)       0:00:00.193 ******
Monday 05 September 2022  13:55:39 +0000 (0:00:00.045)       0:00:00.239 ******
Monday 05 September 2022  13:55:39 +0000 (0:00:00.044)       0:00:00.283 ******
Monday 05 September 2022  13:55:39 +0000 (0:00:00.041)       0:00:00.325 ******
Monday 05 September 2022  13:55:39 +0000 (0:00:00.046)       0:00:00.372 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.044)       0:00:00.416 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.051)       0:00:00.468 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.024)       0:00:00.493 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.023)       0:00:00.516 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.044)       0:00:00.560 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.043)       0:00:00.603 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.048)       0:00:00.652 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.045)       0:00:00.697 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.020)       0:00:00.718 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.041)       0:00:00.760 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.048)       0:00:00.808 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.043)       0:00:00.851 ******
Monday 05 September 2022  13:55:40 +0000 (0:00:00.040)       0:00:00.892 ******
Monday 05 September 2022  13:55:41 +0000 (0:00:01.430)       0:00:02.323 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node2] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node3] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  13:55:42 +0000 (0:00:00.062)       0:00:02.385 ******
Monday 05 September 2022  13:55:42 +0000 (0:00:00.053)       0:00:02.439 ******
Monday 05 September 2022  13:55:42 +0000 (0:00:00.024)       0:00:02.463 ******
Monday 05 September 2022  13:55:42 +0000 (0:00:00.076)       0:00:02.540 ******
Monday 05 September 2022  13:55:42 +0000 (0:00:00.026)       0:00:02.566 ******
Monday 05 September 2022  13:55:42 +0000 (0:00:00.040)       0:00:02.607 ******
[WARNING]: raw module does not support the environment keyword
[WARNING]: raw module does not support the environment keyword
[WARNING]: raw module does not support the environment keyword

TASK [bootstrap-os : Fetch /etc/os-release] ***************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:55:42 +0000 (0:00:00.116)       0:00:02.723 ******
Monday 05 September 2022  13:55:42 +0000 (0:00:00.041)       0:00:02.764 ******
Monday 05 September 2022  13:55:42 +0000 (0:00:00.045)       0:00:02.809 ******
included: /kubespray/roles/bootstrap-os/tasks/bootstrap-redhat.yml for node1, node2, node3
Monday 05 September 2022  13:55:42 +0000 (0:00:00.069)       0:00:02.878 ******

TASK [bootstrap-os : Gather host facts to get ansible_distribution_version ansible_distribution_major_version] ********
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:55:43 +0000 (0:00:00.582)       0:00:03.461 ******

TASK [bootstrap-os : Add proxy to yum.conf or dnf.conf if http_proxy is defined] **************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:55:43 +0000 (0:00:00.369)       0:00:03.831 ******
Monday 05 September 2022  13:55:43 +0000 (0:00:00.047)       0:00:03.879 ******

TASK [bootstrap-os : Check RHEL subscription-manager status] **********************************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  13:55:49 +0000 (0:00:06.486)       0:00:10.365 ******
Monday 05 September 2022  13:55:50 +0000 (0:00:00.050)       0:00:10.415 ******
Monday 05 September 2022  13:55:50 +0000 (0:00:00.043)       0:00:10.459 ******
Monday 05 September 2022  13:55:50 +0000 (0:00:00.046)       0:00:10.505 ******

TASK [bootstrap-os : Enable RHEL 8 repos] *****************************************************************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  13:56:08 +0000 (0:00:18.051)       0:00:28.557 ******

TASK [bootstrap-os : Check presence of fastestmirror.conf] ************************************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  13:56:08 +0000 (0:00:00.377)       0:00:28.934 ******
Monday 05 September 2022  13:56:08 +0000 (0:00:00.048)       0:00:28.983 ******

TASK [bootstrap-os : Install libselinux python package] ***************************************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  13:56:13 +0000 (0:00:04.725)       0:00:33.708 ******
Monday 05 September 2022  13:56:13 +0000 (0:00:00.048)       0:00:33.757 ******
Monday 05 September 2022  13:56:13 +0000 (0:00:00.050)       0:00:33.807 ******
Monday 05 September 2022  13:56:13 +0000 (0:00:00.047)       0:00:33.854 ******
Monday 05 September 2022  13:56:13 +0000 (0:00:00.044)       0:00:33.899 ******
Monday 05 September 2022  13:56:13 +0000 (0:00:00.046)       0:00:33.946 ******
Monday 05 September 2022  13:56:13 +0000 (0:00:00.044)       0:00:33.990 ******

TASK [bootstrap-os : Create remote_tmp for it is used by another module] **********************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  13:56:13 +0000 (0:00:00.316)       0:00:34.306 ******

TASK [bootstrap-os : Gather host facts to get ansible_os_family] ******************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:14 +0000 (0:00:00.339)       0:00:34.646 ******

TASK [bootstrap-os : Assign inventory name to unconfigured hostnames (non-CoreOS, non-Flatcar, Suse and ClearLinux, non-Fedora)] ***
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  13:56:14 +0000 (0:00:00.694)       0:00:35.340 ******
Monday 05 September 2022  13:56:15 +0000 (0:00:00.048)       0:00:35.388 ******
Monday 05 September 2022  13:56:15 +0000 (0:00:00.047)       0:00:35.436 ******
Monday 05 September 2022  13:56:15 +0000 (0:00:00.047)       0:00:35.483 ******

TASK [bootstrap-os : Ensure bash_completion.d folder exists] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]

PLAY [Gather facts] ***************************************************************************************************
Monday 05 September 2022  13:56:15 +0000 (0:00:00.246)       0:00:35.730 ******

TASK [Gather minimal facts] *******************************************************************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  13:56:15 +0000 (0:00:00.360)       0:00:36.090 ******

TASK [Gather necessary facts (network)] *******************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:16 +0000 (0:00:00.360)       0:00:36.451 ******

TASK [Gather necessary facts (hardware)] ******************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]

PLAY [k8s_cluster:etcd] ***********************************************************************************************
Monday 05 September 2022  13:56:16 +0000 (0:00:00.532)       0:00:36.983 ******
Monday 05 September 2022  13:56:16 +0000 (0:00:00.059)       0:00:37.042 ******
Monday 05 September 2022  13:56:16 +0000 (0:00:00.072)       0:00:37.115 ******
Monday 05 September 2022  13:56:16 +0000 (0:00:00.046)       0:00:37.162 ******
Monday 05 September 2022  13:56:16 +0000 (0:00:00.046)       0:00:37.208 ******
Monday 05 September 2022  13:56:16 +0000 (0:00:00.047)       0:00:37.255 ******
Monday 05 September 2022  13:56:16 +0000 (0:00:00.044)       0:00:37.300 ******
Monday 05 September 2022  13:56:16 +0000 (0:00:00.054)       0:00:37.354 ******
Monday 05 September 2022  13:56:16 +0000 (0:00:00.023)       0:00:37.378 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.023)       0:00:37.401 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.073)       0:00:37.475 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.046)       0:00:37.521 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.046)       0:00:37.568 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.050)       0:00:37.618 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.021)       0:00:37.639 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.048)       0:00:37.688 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.052)       0:00:37.740 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.064)       0:00:37.805 ******
Monday 05 September 2022  13:56:17 +0000 (0:00:00.082)       0:00:37.887 ******
Monday 05 September 2022  13:56:19 +0000 (0:00:01.598)       0:00:39.486 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node3] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node2] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  13:56:19 +0000 (0:00:00.052)       0:00:39.539 ******
Monday 05 September 2022  13:56:19 +0000 (0:00:00.084)       0:00:39.624 ******

TASK [kubespray-defaults : create fallback_ips_base] ******************************************************************
ok: [node1]
Monday 05 September 2022  13:56:19 +0000 (0:00:00.038)       0:00:39.663 ******

TASK [kubespray-defaults : set fallback_ips] **************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:19 +0000 (0:00:00.077)       0:00:39.740 ******
Monday 05 September 2022  13:56:19 +0000 (0:00:00.024)       0:00:39.765 ******
Monday 05 September 2022  13:56:19 +0000 (0:00:00.050)       0:00:39.815 ******

TASK [adduser : User | Create User Group] *****************************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:19 +0000 (0:00:00.415)       0:00:40.230 ******

TASK [adduser : User | Create User] ***********************************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:20 +0000 (0:00:00.480)       0:00:40.711 ******

TASK [kubernetes/preinstall : Remove swapfile from /etc/fstab] ********************************************************
ok: [node1] => (item=swap)
ok: [node3] => (item=swap)
ok: [node2] => (item=swap)
ok: [node1] => (item=none)
ok: [node3] => (item=none)
ok: [node2] => (item=none)
Monday 05 September 2022  13:56:20 +0000 (0:00:00.590)       0:00:41.301 ******

TASK [kubernetes/preinstall : check swap] *****************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:21 +0000 (0:00:00.252)       0:00:41.554 ******
Monday 05 September 2022  13:56:21 +0000 (0:00:00.054)       0:00:41.608 ******
Monday 05 September 2022  13:56:21 +0000 (0:00:00.057)       0:00:41.665 ******

TASK [kubernetes/preinstall : Stop if either kube_control_plane or kube_node group is empty] **************************
ok: [node1] => (item=kube_control_plane) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": "kube_control_plane",
    "msg": "All assertions passed"
}
ok: [node1] => (item=kube_node) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": "kube_node",
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:21 +0000 (0:00:00.058)       0:00:41.723 ******

TASK [kubernetes/preinstall : Stop if etcd group is empty in external etcd mode] **************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:21 +0000 (0:00:00.042)       0:00:41.766 ******

TASK [kubernetes/preinstall : Stop if non systemd OS type] ************************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:21 +0000 (0:00:00.077)       0:00:41.843 ******

TASK [kubernetes/preinstall : Stop if unknown OS] *********************************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:21 +0000 (0:00:00.071)       0:00:41.915 ******

TASK [kubernetes/preinstall : Stop if unknown network plugin] *********************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:21 +0000 (0:00:00.072)       0:00:41.988 ******
Monday 05 September 2022  13:56:21 +0000 (0:00:00.051)       0:00:42.039 ******

TASK [kubernetes/preinstall : Stop if supported Calico versions] ******************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:21 +0000 (0:00:00.071)       0:00:42.111 ******

TASK [kubernetes/preinstall : Stop if unsupported version of Kubernetes] **********************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:21 +0000 (0:00:00.071)       0:00:42.183 ******

TASK [kubernetes/preinstall : Stop if known booleans are set as strings (Use JSON format on CLI: -e "{'key': true }")] ***
ok: [node1] => (item={'name': 'download_run_once', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "download_run_once",
        "value": false
    },
    "msg": "All assertions passed"
}
ok: [node1] => (item={'name': 'deploy_netchecker', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "deploy_netchecker",
        "value": false
    },
    "msg": "All assertions passed"
}
ok: [node1] => (item={'name': 'download_always_pull', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "download_always_pull",
        "value": false
    },
    "msg": "All assertions passed"
}
ok: [node1] => (item={'name': 'helm_enabled', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "helm_enabled",
        "value": false
    },
    "msg": "All assertions passed"
}
ok: [node1] => (item={'name': 'openstack_lbaas_enabled', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "openstack_lbaas_enabled",
        "value": false
    },
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:21 +0000 (0:00:00.112)       0:00:42.296 ******

TASK [kubernetes/preinstall : Stop if even number of etcd hosts] ******************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:21 +0000 (0:00:00.069)       0:00:42.365 ******

TASK [kubernetes/preinstall : Stop if memory is too small for masters] ************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:22 +0000 (0:00:00.056)       0:00:42.422 ******

TASK [kubernetes/preinstall : Stop if memory is too small for nodes] **************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:22 +0000 (0:00:00.063)       0:00:42.486 ******

TASK [kubernetes/preinstall : Stop when dynamic_kubelet_configuration enabled for kubernetes >= 1.22] *****************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:22 +0000 (0:00:00.059)       0:00:42.545 ******
Monday 05 September 2022  13:56:22 +0000 (0:00:00.057)       0:00:42.603 ******

TASK [kubernetes/preinstall : Stop if ip var does not match local ips] ************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:22 +0000 (0:00:00.064)       0:00:42.668 ******

TASK [kubernetes/preinstall : Stop if access_ip is not pingable] ******************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:56:22 +0000 (0:00:00.263)       0:00:42.931 ******
Monday 05 September 2022  13:56:22 +0000 (0:00:00.098)       0:00:43.029 ******
Monday 05 September 2022  13:56:22 +0000 (0:00:00.085)       0:00:43.114 ******

TASK [kubernetes/preinstall : Stop if RBAC and anonymous-auth are not enabled when insecure port is disabled] *********
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:22 +0000 (0:00:00.096)       0:00:43.211 ******
Monday 05 September 2022  13:56:22 +0000 (0:00:00.075)       0:00:43.286 ******

TASK [kubernetes/preinstall : Stop if bad hostname] *******************************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:23 +0000 (0:00:00.106)       0:00:43.393 ******
Monday 05 September 2022  13:56:23 +0000 (0:00:00.073)       0:00:43.467 ******

TASK [kubernetes/preinstall : Get current calico cluster version] *****************************************************
ok: [node1]
Monday 05 September 2022  13:56:26 +0000 (0:00:03.751)       0:00:47.218 ******
Monday 05 September 2022  13:56:26 +0000 (0:00:00.026)       0:00:47.245 ******
Monday 05 September 2022  13:56:26 +0000 (0:00:00.026)       0:00:47.271 ******
Monday 05 September 2022  13:56:26 +0000 (0:00:00.055)       0:00:47.327 ******

TASK [kubernetes/preinstall : Check that kube_service_addresses is a network range] ***********************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:26 +0000 (0:00:00.053)       0:00:47.381 ******

TASK [kubernetes/preinstall : Check that kube_pods_subnet is a network range] *****************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:27 +0000 (0:00:00.056)       0:00:47.438 ******

TASK [kubernetes/preinstall : Check that kube_pods_subnet does not collide with kube_service_addresses] ***************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:27 +0000 (0:00:00.066)       0:00:47.504 ******

TASK [kubernetes/preinstall : Stop if unknown dns mode] ***************************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:27 +0000 (0:00:00.046)       0:00:47.550 ******

TASK [kubernetes/preinstall : Stop if unknown kube proxy mode] ********************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:27 +0000 (0:00:00.034)       0:00:47.585 ******

TASK [kubernetes/preinstall : Stop if unknown cert_management] ********************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:27 +0000 (0:00:00.032)       0:00:47.617 ******

TASK [kubernetes/preinstall : Stop if unknown resolvconf_mode] ********************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:27 +0000 (0:00:00.032)       0:00:47.650 ******

TASK [kubernetes/preinstall : Stop if etcd deployment type is not host or docker] *************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:27 +0000 (0:00:00.055)       0:00:47.706 ******

TASK [kubernetes/preinstall : Stop if etcd deployment type is not host when container_manager != docker] **************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:27 +0000 (0:00:00.057)       0:00:47.763 ******
Monday 05 September 2022  13:56:27 +0000 (0:00:00.045)       0:00:47.809 ******
Monday 05 September 2022  13:56:27 +0000 (0:00:00.049)       0:00:47.859 ******
Monday 05 September 2022  13:56:27 +0000 (0:00:00.091)       0:00:47.950 ******
Monday 05 September 2022  13:56:27 +0000 (0:00:00.063)       0:00:48.013 ******
Monday 05 September 2022  13:56:27 +0000 (0:00:00.049)       0:00:48.063 ******

TASK [kubernetes/preinstall : Ensure minimum containerd version] ******************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  13:56:27 +0000 (0:00:00.034)       0:00:48.098 ******
Monday 05 September 2022  13:56:27 +0000 (0:00:00.053)       0:00:48.151 ******
Monday 05 September 2022  13:56:27 +0000 (0:00:00.048)       0:00:48.200 ******

TASK [kubernetes/preinstall : check if booted with ostree] ************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:28 +0000 (0:00:00.244)       0:00:48.444 ******

TASK [kubernetes/preinstall : set is_fedora_coreos] *******************************************************************
ok: [node3]
ok: [node2]
ok: [node1]
Monday 05 September 2022  13:56:28 +0000 (0:00:00.376)       0:00:48.821 ******

TASK [kubernetes/preinstall : set is_fedora_coreos] *******************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:28 +0000 (0:00:00.062)       0:00:48.884 ******

TASK [kubernetes/preinstall : check resolvconf] ***********************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:28 +0000 (0:00:00.233)       0:00:49.117 ******

TASK [kubernetes/preinstall : check existence of /etc/resolvconf/resolv.conf.d] ***************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:28 +0000 (0:00:00.219)       0:00:49.336 ******

TASK [kubernetes/preinstall : check status of /etc/resolv.conf] *******************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:29 +0000 (0:00:00.239)       0:00:49.576 ******

TASK [kubernetes/preinstall : get content of /etc/resolv.conf] ********************************************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  13:56:29 +0000 (0:00:00.371)       0:00:49.947 ******

TASK [kubernetes/preinstall : get currently configured nameservers] ***************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:29 +0000 (0:00:00.112)       0:00:50.060 ******

TASK [kubernetes/preinstall : check systemd-resolved] *****************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:29 +0000 (0:00:00.240)       0:00:50.300 ******

TASK [kubernetes/preinstall : set dns facts] **************************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:29 +0000 (0:00:00.069)       0:00:50.369 ******

TASK [kubernetes/preinstall : check if kubelet is configured] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:30 +0000 (0:00:00.250)       0:00:50.619 ******

TASK [kubernetes/preinstall : check if early DNS configuration stage] *************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:30 +0000 (0:00:00.062)       0:00:50.682 ******

TASK [kubernetes/preinstall : target resolv.conf files] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:30 +0000 (0:00:00.065)       0:00:50.748 ******
Monday 05 September 2022  13:56:30 +0000 (0:00:00.044)       0:00:50.792 ******

TASK [kubernetes/preinstall : check if /etc/dhclient.conf exists] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:30 +0000 (0:00:00.226)       0:00:51.018 ******
Monday 05 September 2022  13:56:30 +0000 (0:00:00.053)       0:00:51.071 ******

TASK [kubernetes/preinstall : check if /etc/dhcp/dhclient.conf exists] ************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:30 +0000 (0:00:00.233)       0:00:51.305 ******

TASK [kubernetes/preinstall : target dhclient conf file for /etc/dhcp/dhclient.conf] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:30 +0000 (0:00:00.062)       0:00:51.367 ******

TASK [kubernetes/preinstall : target dhclient hook file for Red Hat family] *******************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:31 +0000 (0:00:00.062)       0:00:51.430 ******
Monday 05 September 2022  13:56:31 +0000 (0:00:00.047)       0:00:51.477 ******

TASK [kubernetes/preinstall : generate search domains to resolvconf] **************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:31 +0000 (0:00:00.064)       0:00:51.542 ******

TASK [kubernetes/preinstall : pick coredns cluster IP or default resolver] ********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:31 +0000 (0:00:00.095)       0:00:51.637 ******

TASK [kubernetes/preinstall : generate nameservers to resolvconf] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:31 +0000 (0:00:00.063)       0:00:51.701 ******

TASK [kubernetes/preinstall : gather os specific variables] ***********************************************************
ok: [node1] => (item=/kubespray/roles/kubernetes/preinstall/vars/../vars/redhat.yml)
ok: [node3] => (item=/kubespray/roles/kubernetes/preinstall/vars/../vars/redhat.yml)
ok: [node2] => (item=/kubespray/roles/kubernetes/preinstall/vars/../vars/redhat.yml)
Monday 05 September 2022  13:56:31 +0000 (0:00:00.084)       0:00:51.786 ******
Monday 05 September 2022  13:56:31 +0000 (0:00:00.049)       0:00:51.835 ******

TASK [kubernetes/preinstall : check /usr readonly] ********************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:31 +0000 (0:00:00.245)       0:00:52.081 ******
Monday 05 September 2022  13:56:31 +0000 (0:00:00.069)       0:00:52.150 ******
Monday 05 September 2022  13:56:31 +0000 (0:00:00.048)       0:00:52.199 ******
Monday 05 September 2022  13:56:31 +0000 (0:00:00.056)       0:00:52.255 ******

TASK [kubernetes/preinstall : Create kubernetes directories] **********************************************************
ok: [node1] => (item=/etc/kubernetes)
ok: [node3] => (item=/etc/kubernetes)
ok: [node2] => (item=/etc/kubernetes)
ok: [node1] => (item=/etc/kubernetes/ssl)
ok: [node3] => (item=/etc/kubernetes/ssl)
ok: [node2] => (item=/etc/kubernetes/ssl)
ok: [node1] => (item=/etc/kubernetes/manifests)
ok: [node3] => (item=/etc/kubernetes/manifests)
ok: [node2] => (item=/etc/kubernetes/manifests)
ok: [node1] => (item=/usr/local/bin/kubernetes-scripts)
ok: [node3] => (item=/usr/local/bin/kubernetes-scripts)
ok: [node2] => (item=/usr/local/bin/kubernetes-scripts)
ok: [node1] => (item=/usr/libexec/kubernetes/kubelet-plugins/volume/exec)
ok: [node3] => (item=/usr/libexec/kubernetes/kubelet-plugins/volume/exec)
ok: [node2] => (item=/usr/libexec/kubernetes/kubelet-plugins/volume/exec)
Monday 05 September 2022  13:56:33 +0000 (0:00:01.156)       0:00:53.411 ******

TASK [kubernetes/preinstall : Create other directories] ***************************************************************
ok: [node1] => (item=/usr/local/bin)
ok: [node2] => (item=/usr/local/bin)
ok: [node3] => (item=/usr/local/bin)
Monday 05 September 2022  13:56:33 +0000 (0:00:00.262)       0:00:53.674 ******

TASK [kubernetes/preinstall : Check if kubernetes kubeadm compat cert dir exists] *************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:33 +0000 (0:00:00.250)       0:00:53.925 ******
Monday 05 September 2022  13:56:33 +0000 (0:00:00.077)       0:00:54.002 ******

TASK [kubernetes/preinstall : Create cni directories] *****************************************************************
ok: [node1] => (item=/etc/cni/net.d)
ok: [node3] => (item=/etc/cni/net.d)
ok: [node2] => (item=/etc/cni/net.d)
ok: [node2] => (item=/opt/cni/bin)
ok: [node1] => (item=/opt/cni/bin)
ok: [node3] => (item=/opt/cni/bin)
ok: [node3] => (item=/var/lib/calico)
ok: [node1] => (item=/var/lib/calico)
ok: [node2] => (item=/var/lib/calico)
Monday 05 September 2022  13:56:34 +0000 (0:00:00.687)       0:00:54.690 ******
Monday 05 September 2022  13:56:34 +0000 (0:00:00.059)       0:00:54.750 ******
Monday 05 September 2022  13:56:34 +0000 (0:00:00.050)       0:00:54.801 ******

TASK [kubernetes/preinstall : Add domain/search/nameservers/options to resolv.conf] ***********************************
changed: [node2]
changed: [node1]
changed: [node3]
Monday 05 September 2022  13:56:34 +0000 (0:00:00.388)       0:00:55.189 ******

TASK [kubernetes/preinstall : Remove search/domain/nameserver options before block] ***********************************
ok: [node2] => (item=['/etc/resolv.conf', 'search '])
ok: [node1] => (item=['/etc/resolv.conf', 'search '])
ok: [node3] => (item=['/etc/resolv.conf', 'search '])
ok: [node2] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node1] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node3] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node2] => (item=['/etc/resolv.conf', 'domain '])
ok: [node1] => (item=['/etc/resolv.conf', 'domain '])
ok: [node3] => (item=['/etc/resolv.conf', 'domain '])
ok: [node2] => (item=['/etc/resolv.conf', 'options '])
ok: [node1] => (item=['/etc/resolv.conf', 'options '])
ok: [node3] => (item=['/etc/resolv.conf', 'options '])
Monday 05 September 2022  13:56:35 +0000 (0:00:01.060)       0:00:56.250 ******

TASK [kubernetes/preinstall : Remove search/domain/nameserver options after block] ************************************
changed: [node1] => (item=['/etc/resolv.conf', 'search '])
changed: [node2] => (item=['/etc/resolv.conf', 'search '])
changed: [node3] => (item=['/etc/resolv.conf', 'search '])
changed: [node1] => (item=['/etc/resolv.conf', 'nameserver '])
changed: [node3] => (item=['/etc/resolv.conf', 'nameserver '])
changed: [node2] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node1] => (item=['/etc/resolv.conf', 'domain '])
ok: [node3] => (item=['/etc/resolv.conf', 'domain '])
ok: [node2] => (item=['/etc/resolv.conf', 'domain '])
ok: [node1] => (item=['/etc/resolv.conf', 'options '])
ok: [node3] => (item=['/etc/resolv.conf', 'options '])
ok: [node2] => (item=['/etc/resolv.conf', 'options '])
Monday 05 September 2022  13:56:36 +0000 (0:00:00.948)       0:00:57.199 ******
Monday 05 September 2022  13:56:36 +0000 (0:00:00.063)       0:00:57.262 ******
Monday 05 September 2022  13:56:36 +0000 (0:00:00.056)       0:00:57.319 ******
Monday 05 September 2022  13:56:36 +0000 (0:00:00.052)       0:00:57.372 ******

TASK [kubernetes/preinstall : NetworkManager | Check if host has NetworkManager] **************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:37 +0000 (0:00:00.234)       0:00:57.606 ******

TASK [kubernetes/preinstall : NetworkManager | Ensure NetworkManager conf.d dir] **************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:37 +0000 (0:00:00.247)       0:00:57.854 ******

TASK [kubernetes/preinstall : NetworkManager | Prevent NetworkManager from managing Calico interfaces (cali*/tunl*/vxlan.calico)] ***
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:38 +0000 (0:00:00.832)       0:00:58.686 ******

TASK [kubernetes/preinstall : NetworkManager | Prevent NetworkManager from managing K8S interfaces (kube-ipvs0/nodelocaldns)] ***
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:38 +0000 (0:00:00.536)       0:00:59.223 ******
Monday 05 September 2022  13:56:38 +0000 (0:00:00.054)       0:00:59.277 ******
Monday 05 September 2022  13:56:38 +0000 (0:00:00.051)       0:00:59.329 ******
Monday 05 September 2022  13:56:38 +0000 (0:00:00.052)       0:00:59.382 ******
Monday 05 September 2022  13:56:39 +0000 (0:00:00.050)       0:00:59.432 ******
Monday 05 September 2022  13:56:39 +0000 (0:00:00.057)       0:00:59.490 ******

TASK [kubernetes/preinstall : Remove legacy docker repo file] *********************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:39 +0000 (0:00:00.245)       0:00:59.736 ******
Monday 05 September 2022  13:56:39 +0000 (0:00:00.053)       0:00:59.789 ******
Monday 05 September 2022  13:56:39 +0000 (0:00:00.056)       0:00:59.845 ******

TASK [kubernetes/preinstall : Update common_required_pkgs with ipvsadm when kube_proxy_mode is ipvs] ******************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:39 +0000 (0:00:00.064)       0:00:59.910 ******

TASK [kubernetes/preinstall : Install packages requirements] **********************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:46 +0000 (0:00:06.575)       0:01:06.486 ******
Monday 05 September 2022  13:56:46 +0000 (0:00:00.067)       0:01:06.553 ******

TASK [kubernetes/preinstall : Confirm selinux deployed] ***************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:46 +0000 (0:00:00.241)       0:01:06.795 ******

TASK [kubernetes/preinstall : Set selinux policy] *********************************************************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  13:56:46 +0000 (0:00:00.572)       0:01:07.367 ******
Monday 05 September 2022  13:56:47 +0000 (0:00:00.067)       0:01:07.434 ******

TASK [kubernetes/preinstall : Stat sysctl file configuration] *********************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:47 +0000 (0:00:00.241)       0:01:07.676 ******

TASK [kubernetes/preinstall : Change sysctl file path to link source if linked] ***************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:47 +0000 (0:00:00.067)       0:01:07.744 ******

TASK [kubernetes/preinstall : Make sure sysctl file path folder exists] ***********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:47 +0000 (0:00:00.292)       0:01:08.036 ******

TASK [kubernetes/preinstall : Enable ip forwarding] *******************************************************************
ok: [node3]
ok: [node2]
ok: [node1]
Monday 05 September 2022  13:56:47 +0000 (0:00:00.306)       0:01:08.343 ******
Monday 05 September 2022  13:56:48 +0000 (0:00:00.051)       0:01:08.394 ******

TASK [kubernetes/preinstall : Check if we need to set fs.may_detach_mounts] *******************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:48 +0000 (0:00:00.246)       0:01:08.640 ******
Monday 05 September 2022  13:56:48 +0000 (0:00:00.052)       0:01:08.693 ******

TASK [kubernetes/preinstall : Ensure kube-bench parameters are set] ***************************************************
ok: [node1] => (item={'name': 'vm.overcommit_memory', 'value': 1})
ok: [node3] => (item={'name': 'vm.overcommit_memory', 'value': 1})
ok: [node2] => (item={'name': 'vm.overcommit_memory', 'value': 1})
ok: [node1] => (item={'name': 'kernel.panic', 'value': 10})
ok: [node3] => (item={'name': 'kernel.panic', 'value': 10})
ok: [node2] => (item={'name': 'kernel.panic', 'value': 10})
ok: [node1] => (item={'name': 'kernel.panic_on_oops', 'value': 1})
ok: [node3] => (item={'name': 'kernel.panic_on_oops', 'value': 1})
ok: [node2] => (item={'name': 'kernel.panic_on_oops', 'value': 1})
Monday 05 September 2022  13:56:48 +0000 (0:00:00.664)       0:01:09.358 ******

TASK [kubernetes/preinstall : Check dummy module] *********************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:49 +0000 (0:00:00.284)       0:01:09.642 ******

TASK [kubernetes/preinstall : Hosts | create list from inventory] *****************************************************
ok: [node1]
Monday 05 September 2022  13:56:49 +0000 (0:00:00.080)       0:01:09.723 ******

TASK [kubernetes/preinstall : Hosts | populate inventory into hosts file] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:49 +0000 (0:00:00.245)       0:01:09.968 ******
Monday 05 September 2022  13:56:49 +0000 (0:00:00.054)       0:01:10.023 ******

TASK [kubernetes/preinstall : Hosts | Retrieve hosts file content] ****************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:56:49 +0000 (0:00:00.245)       0:01:10.268 ******

TASK [kubernetes/preinstall : Hosts | Extract existing entries for localhost from hosts file] *************************
ok: [node1] => (item=127.0.0.1 localhost rhel01)
ok: [node1] => (item=127.0.0.1 localhost.localdomain localhost)
ok: [node1] => (item=127.0.0.1 localhost4.localdomain4 localhost4 localhost localhost.localdomain)
ok: [node1] => (item=::1 localhost rhel01)
ok: [node1] => (item=::1 localhost.localdomain localhost)
ok: [node1] => (item=::1 localhost6.localdomain6 localhost6 localhost6.localdomain)
ok: [node2] => (item=127.0.0.1 localhost rhel02)
ok: [node3] => (item=127.0.0.1 localhost rhel03)
ok: [node2] => (item=127.0.0.1 localhost.localdomain localhost)
ok: [node2] => (item=127.0.0.1 localhost4.localdomain4 localhost4 localhost localhost.localdomain)
ok: [node3] => (item=127.0.0.1 localhost.localdomain localhost)
ok: [node3] => (item=127.0.0.1 localhost4.localdomain4 localhost4 localhost localhost.localdomain)
ok: [node2] => (item=::1 localhost rhel02)
ok: [node2] => (item=::1 localhost.localdomain localhost)
ok: [node2] => (item=::1 localhost6.localdomain6 localhost6 localhost6.localdomain)
ok: [node3] => (item=::1 localhost rhel03)
ok: [node3] => (item=::1 localhost.localdomain localhost)
ok: [node3] => (item=::1 localhost6.localdomain6 localhost6 localhost6.localdomain)
Monday 05 September 2022  13:56:50 +0000 (0:00:00.610)       0:01:10.879 ******

TASK [kubernetes/preinstall : Hosts | Update target hosts file entries dict with required entries] ********************
ok: [node1] => (item={'key': '127.0.0.1', 'value': {'expected': ['localhost', 'localhost.localdomain']}})
ok: [node1] => (item={'key': '::1', 'value': {'expected': ['localhost6', 'localhost6.localdomain'], 'unexpected': ['localhost', 'localhost.localdomain']}})
ok: [node2] => (item={'key': '127.0.0.1', 'value': {'expected': ['localhost', 'localhost.localdomain']}})
ok: [node2] => (item={'key': '::1', 'value': {'expected': ['localhost6', 'localhost6.localdomain'], 'unexpected': ['localhost', 'localhost.localdomain']}})
ok: [node3] => (item={'key': '127.0.0.1', 'value': {'expected': ['localhost', 'localhost.localdomain']}})
ok: [node3] => (item={'key': '::1', 'value': {'expected': ['localhost6', 'localhost6.localdomain'], 'unexpected': ['localhost', 'localhost.localdomain']}})
Monday 05 September 2022  13:56:50 +0000 (0:00:00.113)       0:01:10.992 ******

TASK [kubernetes/preinstall : Hosts | Update (if necessary) hosts file] ***********************************************
ok: [node1] => (item={'key': '127.0.0.1', 'value': ['localhost4.localdomain4', 'localhost4', 'localhost', 'localhost.localdomain']})
ok: [node3] => (item={'key': '127.0.0.1', 'value': ['localhost4.localdomain4', 'localhost4', 'localhost', 'localhost.localdomain']})
ok: [node2] => (item={'key': '127.0.0.1', 'value': ['localhost4.localdomain4', 'localhost4', 'localhost', 'localhost.localdomain']})
ok: [node1] => (item={'key': '::1', 'value': ['localhost6.localdomain6', 'localhost6', 'localhost6.localdomain']})
ok: [node3] => (item={'key': '::1', 'value': ['localhost6.localdomain6', 'localhost6', 'localhost6.localdomain']})
ok: [node2] => (item={'key': '::1', 'value': ['localhost6.localdomain6', 'localhost6', 'localhost6.localdomain']})
Monday 05 September 2022  13:56:51 +0000 (0:00:00.468)       0:01:11.461 ******

TASK [kubernetes/preinstall : Update facts] ***************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:51 +0000 (0:00:00.348)       0:01:11.809 ******

TASK [kubernetes/preinstall : Configure dhclient to supersede search/domain/nameservers] ******************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:56:51 +0000 (0:00:00.259)       0:01:12.069 ******
Monday 05 September 2022  13:56:51 +0000 (0:00:00.064)       0:01:12.133 ******

TASK [kubernetes/preinstall : Configure dhclient hooks for resolv.conf (RH-only)] *************************************
changed: [node3]
changed: [node2]
changed: [node1]
Monday 05 September 2022  13:56:52 +0000 (0:00:00.684)       0:01:12.818 ******
Monday 05 September 2022  13:56:52 +0000 (0:00:00.050)       0:01:12.868 ******
Monday 05 September 2022  13:56:52 +0000 (0:00:00.049)       0:01:12.918 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | propagate resolvconf to k8s components] *************************
changed: [node2]
changed: [node1]
changed: [node3]
Monday 05 September 2022  13:56:52 +0000 (0:00:00.229)       0:01:13.147 ******
Monday 05 September 2022  13:56:52 +0000 (0:00:00.054)       0:01:13.202 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | kube-apiserver configured] **************************************
ok: [node1]
Monday 05 September 2022  13:56:52 +0000 (0:00:00.181)       0:01:13.384 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | kube-controller configured] *************************************
ok: [node1]
Monday 05 September 2022  13:56:53 +0000 (0:00:00.185)       0:01:13.569 ******
Monday 05 September 2022  13:56:53 +0000 (0:00:00.048)       0:01:13.618 ******
Monday 05 September 2022  13:56:53 +0000 (0:00:00.052)       0:01:13.670 ******
Monday 05 September 2022  13:56:53 +0000 (0:00:00.050)       0:01:13.721 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | restart kube-apiserver crio/containerd] *************************
changed: [node1]
Monday 05 September 2022  13:56:53 +0000 (0:00:00.199)       0:01:13.921 ******
Monday 05 September 2022  13:56:53 +0000 (0:00:00.053)       0:01:13.974 ******

TASK [kubernetes/preinstall : Check if we are running inside a Azure VM] **********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:53 +0000 (0:00:00.233)       0:01:14.208 ******
Monday 05 September 2022  13:56:53 +0000 (0:00:00.051)       0:01:14.259 ******
Monday 05 September 2022  13:56:53 +0000 (0:00:00.056)       0:01:14.315 ******
Monday 05 September 2022  13:56:53 +0000 (0:00:00.050)       0:01:14.366 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.054)       0:01:14.420 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.045)       0:01:14.466 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.053)       0:01:14.520 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.054)       0:01:14.574 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.053)       0:01:14.627 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.057)       0:01:14.684 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.051)       0:01:14.736 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.059)       0:01:14.796 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.058)       0:01:14.854 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.078)       0:01:14.933 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.055)       0:01:14.988 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.049)       0:01:15.038 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.050)       0:01:15.088 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.066)       0:01:15.155 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.058)       0:01:15.213 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.053)       0:01:15.267 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.052)       0:01:15.320 ******
Monday 05 September 2022  13:56:54 +0000 (0:00:00.059)       0:01:15.379 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.046)       0:01:15.425 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.071)       0:01:15.497 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.053)       0:01:15.550 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.051)       0:01:15.602 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.055)       0:01:15.658 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.059)       0:01:15.717 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.051)       0:01:15.769 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.055)       0:01:15.825 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.055)       0:01:15.880 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.057)       0:01:15.938 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.053)       0:01:15.991 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.053)       0:01:16.045 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.066)       0:01:16.112 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.050)       0:01:16.162 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.059)       0:01:16.222 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.057)       0:01:16.280 ******
Monday 05 September 2022  13:56:55 +0000 (0:00:00.076)       0:01:16.356 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.054)       0:01:16.410 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.054)       0:01:16.464 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.067)       0:01:16.531 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.063)       0:01:16.594 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.045)       0:01:16.640 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.098)       0:01:16.738 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.054)       0:01:16.792 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.055)       0:01:16.848 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.051)       0:01:16.899 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.053)       0:01:16.952 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.058)       0:01:17.010 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.053)       0:01:17.064 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.053)       0:01:17.118 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.059)       0:01:17.177 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.075)       0:01:17.253 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.056)       0:01:17.309 ******
Monday 05 September 2022  13:56:56 +0000 (0:00:00.050)       0:01:17.359 ******
Monday 05 September 2022  13:56:57 +0000 (0:00:00.063)       0:01:17.423 ******
Monday 05 September 2022  13:56:57 +0000 (0:00:00.076)       0:01:17.499 ******
Monday 05 September 2022  13:56:57 +0000 (0:00:00.048)       0:01:17.547 ******
Monday 05 September 2022  13:56:57 +0000 (0:00:00.074)       0:01:17.622 ******
Monday 05 September 2022  13:56:57 +0000 (0:00:00.077)       0:01:17.699 ******
Monday 05 September 2022  13:56:57 +0000 (0:00:00.053)       0:01:17.753 ******
Monday 05 September 2022  13:56:57 +0000 (0:00:00.054)       0:01:17.808 ******
Monday 05 September 2022  13:56:57 +0000 (0:00:00.058)       0:01:17.866 ******

TASK [container-engine/containerd-common : containerd-common | check if fedora coreos] ********************************
ok: [node2]
ok: [node3]
ok: [node1]
Monday 05 September 2022  13:56:57 +0000 (0:00:00.279)       0:01:18.145 ******

TASK [container-engine/containerd-common : containerd-common | set is_ostree] *****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:57 +0000 (0:00:00.078)       0:01:18.223 ******
Monday 05 September 2022  13:56:57 +0000 (0:00:00.062)       0:01:18.286 ******

TASK [container-engine/runc : runc | set is_ostree] *******************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:56:57 +0000 (0:00:00.070)       0:01:18.357 ******

TASK [container-engine/runc : runc | Uninstall runc package managed by package manager] *******************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  13:57:04 +0000 (0:00:06.569)       0:01:24.927 ******
included: /kubespray/roles/container-engine/runc/tasks/../../../download/tasks/download_file.yml for node1, node2, node3
Monday 05 September 2022  13:57:04 +0000 (0:00:00.080)       0:01:25.007 ******

TASK [container-engine/runc : download_file | Starting download of file] **********************************************
ok: [node1] => {
    "msg": "https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64"
}
ok: [node2] => {
    "msg": "https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64"
}
ok: [node3] => {
    "msg": "https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64"
}
Monday 05 September 2022  13:57:05 +0000 (0:00:00.788)       0:01:25.796 ******

TASK [container-engine/runc : download_file | Set pathname of cached file] ********************************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  13:57:06 +0000 (0:00:00.814)       0:01:26.611 ******

TASK [container-engine/runc : download_file | Create dest directory on node] ******************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:57:07 +0000 (0:00:01.138)       0:01:27.749 ******
Monday 05 September 2022  13:57:07 +0000 (0:00:00.039)       0:01:27.789 ******
Monday 05 September 2022  13:57:07 +0000 (0:00:00.042)       0:01:27.832 ******

TASK [container-engine/runc : download_file | Download item] **********************************************************
changed: [node1 -> 192.168.88.211]
changed: [node3 -> 192.168.88.213]
changed: [node2 -> 192.168.88.212]
Monday 05 September 2022  13:57:23 +0000 (0:00:16.062)       0:01:43.894 ******
Monday 05 September 2022  13:57:23 +0000 (0:00:00.073)       0:01:43.968 ******
Monday 05 September 2022  13:57:23 +0000 (0:00:00.068)       0:01:44.037 ******
Monday 05 September 2022  13:57:23 +0000 (0:00:00.061)       0:01:44.098 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  13:57:23 +0000 (0:00:00.086)       0:01:44.185 ******
Monday 05 September 2022  13:57:24 +0000 (0:00:00.832)       0:01:45.017 ******

TASK [container-engine/runc : Copy runc binary from download dir] *****************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:57:25 +0000 (0:00:01.098)       0:01:46.115 ******

TASK [container-engine/runc : runc | Remove orphaned binary] **********************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:57:25 +0000 (0:00:00.258)       0:01:46.374 ******
included: /kubespray/roles/container-engine/crictl/tasks/crictl.yml for node1, node3, node2
Monday 05 September 2022  13:57:26 +0000 (0:00:00.069)       0:01:46.443 ******
included: /kubespray/roles/container-engine/crictl/tasks/../../../download/tasks/download_file.yml for node1, node2, node3
Monday 05 September 2022  13:57:26 +0000 (0:00:00.084)       0:01:46.528 ******

TASK [container-engine/crictl : download_file | Starting download of file] ********************************************
ok: [node2] => {
    "msg": "https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz"
}
ok: [node3] => {
    "msg": "https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz"
}
ok: [node1] => {
    "msg": "https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  13:57:26 +0000 (0:00:00.722)       0:01:47.250 ******

TASK [container-engine/crictl : download_file | Set pathname of cached file] ******************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:57:27 +0000 (0:00:00.918)       0:01:48.169 ******

TASK [container-engine/crictl : download_file | Create dest directory on node] ****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:57:28 +0000 (0:00:01.165)       0:01:49.335 ******
Monday 05 September 2022  13:57:28 +0000 (0:00:00.041)       0:01:49.376 ******
Monday 05 September 2022  13:57:29 +0000 (0:00:00.041)       0:01:49.417 ******

TASK [container-engine/crictl : download_file | Download item] ********************************************************
changed: [node1 -> 192.168.88.211]
changed: [node2 -> 192.168.88.212]
changed: [node3 -> 192.168.88.213]
Monday 05 September 2022  13:57:51 +0000 (0:00:22.399)       0:02:11.817 ******
Monday 05 September 2022  13:57:51 +0000 (0:00:00.064)       0:02:11.881 ******
Monday 05 September 2022  13:57:51 +0000 (0:00:00.072)       0:02:11.954 ******
Monday 05 September 2022  13:57:51 +0000 (0:00:00.064)       0:02:12.019 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  13:57:51 +0000 (0:00:00.079)       0:02:12.098 ******

TASK [container-engine/crictl : extract_file | Unpacking archive] *****************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:57:54 +0000 (0:00:03.147)       0:02:15.246 ******

TASK [container-engine/crictl : Install crictl config] ****************************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:57:55 +0000 (0:00:00.632)       0:02:15.879 ******

TASK [container-engine/crictl : Copy crictl binary from download dir] *************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:57:55 +0000 (0:00:00.507)       0:02:16.387 ******

TASK [container-engine/crictl : Set fact crictl_installed] ************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:57:56 +0000 (0:00:00.087)       0:02:16.475 ******
included: /kubespray/roles/container-engine/nerdctl/tasks/../../../download/tasks/download_file.yml for node1, node2, node3
Monday 05 September 2022  13:57:56 +0000 (0:00:00.072)       0:02:16.547 ******

TASK [container-engine/nerdctl : download_file | Starting download of file] *******************************************
ok: [node1] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
ok: [node2] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
ok: [node3] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  13:57:56 +0000 (0:00:00.765)       0:02:17.313 ******

TASK [container-engine/nerdctl : download_file | Set pathname of cached file] *****************************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  13:57:57 +0000 (0:00:00.951)       0:02:18.264 ******

TASK [container-engine/nerdctl : download_file | Create dest directory on node] ***************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:57:59 +0000 (0:00:01.239)       0:02:19.504 ******
Monday 05 September 2022  13:57:59 +0000 (0:00:00.036)       0:02:19.540 ******
Monday 05 September 2022  13:57:59 +0000 (0:00:00.040)       0:02:19.581 ******

TASK [container-engine/nerdctl : download_file | Download item] *******************************************************
changed: [node2 -> 192.168.88.212]
changed: [node3 -> 192.168.88.213]
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  13:58:15 +0000 (0:00:16.436)       0:02:36.017 ******
Monday 05 September 2022  13:58:15 +0000 (0:00:00.076)       0:02:36.094 ******
Monday 05 September 2022  13:58:15 +0000 (0:00:00.064)       0:02:36.158 ******
Monday 05 September 2022  13:58:15 +0000 (0:00:00.061)       0:02:36.220 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  13:58:15 +0000 (0:00:00.080)       0:02:36.300 ******

TASK [container-engine/nerdctl : extract_file | Unpacking archive] ****************************************************
changed: [node3]
changed: [node1]
changed: [node2]
Monday 05 September 2022  13:58:19 +0000 (0:00:03.295)       0:02:39.596 ******

TASK [container-engine/nerdctl : nerdctl | Copy nerdctl binary from download dir] *************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:58:19 +0000 (0:00:00.451)       0:02:40.047 ******
Monday 05 September 2022  13:58:19 +0000 (0:00:00.072)       0:02:40.120 ******

TASK [container-engine/containerd : containerd | Remove any package manager controlled containerd package] ************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:58:27 +0000 (0:00:07.980)       0:02:48.100 ******

TASK [container-engine/containerd : containerd | Remove containerd repository] ****************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:58:27 +0000 (0:00:00.261)       0:02:48.361 ******
Monday 05 September 2022  13:58:28 +0000 (0:00:00.077)       0:02:48.439 ******
included: /kubespray/roles/container-engine/containerd/tasks/../../../download/tasks/download_file.yml for node1, node2, node3
Monday 05 September 2022  13:58:28 +0000 (0:00:00.074)       0:02:48.513 ******

TASK [container-engine/containerd : download_file | Starting download of file] ****************************************
ok: [node1] => {
    "msg": "https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz"
}
ok: [node2] => {
    "msg": "https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz"
}
ok: [node3] => {
    "msg": "https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz"
}
Monday 05 September 2022  13:58:28 +0000 (0:00:00.780)       0:02:49.294 ******

TASK [container-engine/containerd : download_file | Set pathname of cached file] **************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  13:58:29 +0000 (0:00:00.736)       0:02:50.031 ******

TASK [container-engine/containerd : download_file | Create dest directory on node] ************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  13:58:30 +0000 (0:00:01.210)       0:02:51.242 ******
Monday 05 September 2022  13:58:30 +0000 (0:00:00.043)       0:02:51.285 ******
Monday 05 September 2022  13:58:30 +0000 (0:00:00.039)       0:02:51.324 ******

TASK [container-engine/containerd : download_file | Download item] ****************************************************
changed: [node2 -> 192.168.88.212]
changed: [node3 -> 192.168.88.213]
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  13:58:57 +0000 (0:00:27.022)       0:03:18.347 ******
Monday 05 September 2022  13:58:58 +0000 (0:00:00.074)       0:03:18.421 ******
Monday 05 September 2022  13:58:58 +0000 (0:00:00.071)       0:03:18.492 ******
Monday 05 September 2022  13:58:58 +0000 (0:00:00.070)       0:03:18.563 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  13:58:58 +0000 (0:00:00.078)       0:03:18.641 ******
Monday 05 September 2022  13:58:59 +0000 (0:00:00.871)       0:03:19.512 ******

TASK [container-engine/containerd : containerd | Unpack containerd archive] *******************************************
changed: [node2]
changed: [node1]
changed: [node3]
Monday 05 September 2022  13:59:02 +0000 (0:00:02.938)       0:03:22.451 ******

TASK [container-engine/containerd : containerd | Remove orphaned binary] **********************************************
ok: [node1] => (item=containerd)
ok: [node2] => (item=containerd)
ok: [node3] => (item=containerd)
ok: [node1] => (item=containerd-shim)
ok: [node3] => (item=containerd-shim)
ok: [node2] => (item=containerd-shim)
ok: [node1] => (item=containerd-shim-runc-v1)
ok: [node3] => (item=containerd-shim-runc-v1)
ok: [node2] => (item=containerd-shim-runc-v1)
ok: [node1] => (item=containerd-shim-runc-v2)
ok: [node3] => (item=containerd-shim-runc-v2)
ok: [node2] => (item=containerd-shim-runc-v2)
ok: [node1] => (item=ctr)
ok: [node3] => (item=ctr)
ok: [node2] => (item=ctr)
Monday 05 September 2022  13:59:03 +0000 (0:00:01.200)       0:03:23.652 ******

TASK [container-engine/containerd : containerd | Generate systemd service for containerd] *****************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:59:03 +0000 (0:00:00.626)       0:03:24.279 ******

TASK [container-engine/containerd : containerd | Ensure containerd directories exist] *********************************
changed: [node1] => (item=/etc/systemd/system/containerd.service.d)
changed: [node3] => (item=/etc/systemd/system/containerd.service.d)
changed: [node2] => (item=/etc/systemd/system/containerd.service.d)
changed: [node1] => (item=/etc/containerd)
changed: [node3] => (item=/etc/containerd)
changed: [node2] => (item=/etc/containerd)
changed: [node1] => (item=/var/lib/containerd)
changed: [node3] => (item=/var/lib/containerd)
changed: [node2] => (item=/var/lib/containerd)
changed: [node1] => (item=/run/containerd)
changed: [node3] => (item=/run/containerd)
changed: [node2] => (item=/run/containerd)
Monday 05 September 2022  13:59:04 +0000 (0:00:00.925)       0:03:25.204 ******
Monday 05 September 2022  13:59:04 +0000 (0:00:00.063)       0:03:25.267 ******

TASK [container-engine/containerd : containerd | Copy containerd config file] *****************************************
changed: [node1]
changed: [node3]
changed: [node2]
[WARNING]: flush_handlers task does not support when conditional
Monday 05 September 2022  13:59:05 +0000 (0:00:00.651)       0:03:25.919 ******

RUNNING HANDLER [container-engine/containerd : restart containerd] ****************************************************
changed: [node3]
changed: [node2]
changed: [node1]
Monday 05 September 2022  13:59:05 +0000 (0:00:00.240)       0:03:26.160 ******

RUNNING HANDLER [container-engine/containerd : Containerd | restart containerd] ***************************************
changed: [node2]
changed: [node1]
changed: [node3]
Monday 05 September 2022  13:59:06 +0000 (0:00:00.974)       0:03:27.134 ******

RUNNING HANDLER [container-engine/containerd : Containerd | wait for containerd] **************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  13:59:07 +0000 (0:00:00.305)       0:03:27.439 ******

RUNNING HANDLER [container-engine/crictl : Get crictl completion] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:59:07 +0000 (0:00:00.280)       0:03:27.720 ******

RUNNING HANDLER [container-engine/crictl : Install crictl completion] *************************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  13:59:08 +0000 (0:00:00.671)       0:03:28.392 ******

RUNNING HANDLER [container-engine/nerdctl : Get nerdctl completion] ***************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:59:08 +0000 (0:00:00.285)       0:03:28.678 ******

RUNNING HANDLER [container-engine/nerdctl : Install nerdctl completion] ***********************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:59:08 +0000 (0:00:00.618)       0:03:29.296 ******

TASK [container-engine/containerd : containerd | Ensure containerd is started and enabled] ****************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  13:59:09 +0000 (0:00:00.622)       0:03:29.919 ******
Monday 05 September 2022  13:59:09 +0000 (0:00:00.079)       0:03:29.999 ******
Monday 05 September 2022  13:59:09 +0000 (0:00:00.057)       0:03:30.057 ******
Monday 05 September 2022  13:59:09 +0000 (0:00:00.055)       0:03:30.113 ******
Monday 05 September 2022  13:59:09 +0000 (0:00:00.057)       0:03:30.170 ******
Monday 05 September 2022  13:59:09 +0000 (0:00:00.055)       0:03:30.226 ******
Monday 05 September 2022  13:59:09 +0000 (0:00:00.055)       0:03:30.281 ******
Monday 05 September 2022  13:59:09 +0000 (0:00:00.054)       0:03:30.336 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.055)       0:03:30.392 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.090)       0:03:30.482 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.057)       0:03:30.540 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.054)       0:03:30.595 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.058)       0:03:30.653 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.058)       0:03:30.711 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.055)       0:03:30.767 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.056)       0:03:30.824 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.064)       0:03:30.888 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.064)       0:03:30.952 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.056)       0:03:31.009 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.059)       0:03:31.069 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.107)       0:03:31.176 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.065)       0:03:31.242 ******
Monday 05 September 2022  13:59:10 +0000 (0:00:00.102)       0:03:31.345 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.066)       0:03:31.411 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.045)       0:03:31.456 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.060)       0:03:31.517 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.058)       0:03:31.575 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.060)       0:03:31.636 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.058)       0:03:31.694 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.061)       0:03:31.756 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.058)       0:03:31.814 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.063)       0:03:31.878 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.064)       0:03:31.942 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.059)       0:03:32.001 ******

TASK [download : prep_download | Set a few facts] *********************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:59:11 +0000 (0:00:00.071)       0:03:32.072 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.059)       0:03:32.132 ******

TASK [download : prep_download | Set image pull/info command for containerd] ******************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:59:11 +0000 (0:00:00.073)       0:03:32.205 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.057)       0:03:32.262 ******
Monday 05 September 2022  13:59:11 +0000 (0:00:00.063)       0:03:32.325 ******

TASK [download : prep_download | Set image pull/info command for containerd on localhost] *****************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:59:12 +0000 (0:00:00.068)       0:03:32.394 ******
Monday 05 September 2022  13:59:12 +0000 (0:00:00.084)       0:03:32.478 ******
Monday 05 September 2022  13:59:12 +0000 (0:00:00.038)       0:03:32.517 ******
Monday 05 September 2022  13:59:12 +0000 (0:00:00.038)       0:03:32.555 ******
Monday 05 September 2022  13:59:12 +0000 (0:00:00.057)       0:03:32.613 ******
Monday 05 September 2022  13:59:12 +0000 (0:00:00.057)       0:03:32.670 ******

TASK [download : prep_download | Register docker images info] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:59:12 +0000 (0:00:00.281)       0:03:32.952 ******

TASK [download : prep_download | Create staging directory on remote node] *********************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:59:12 +0000 (0:00:00.311)       0:03:33.263 ******
Monday 05 September 2022  13:59:12 +0000 (0:00:00.026)       0:03:33.290 ******
Monday 05 September 2022  13:59:12 +0000 (0:00:00.061)       0:03:33.352 ******
included: /kubespray/roles/container-engine/nerdctl/tasks/../../../download/tasks/download_file.yml for node1, node2, node3
Monday 05 September 2022  13:59:13 +0000 (0:00:00.077)       0:03:33.429 ******

TASK [container-engine/nerdctl : download_file | Starting download of file] *******************************************
ok: [node2] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
ok: [node1] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
ok: [node3] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  13:59:13 +0000 (0:00:00.924)       0:03:34.353 ******

TASK [container-engine/nerdctl : download_file | Set pathname of cached file] *****************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  13:59:14 +0000 (0:00:00.845)       0:03:35.198 ******

TASK [container-engine/nerdctl : download_file | Create dest directory on node] ***************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  13:59:16 +0000 (0:00:01.337)       0:03:36.536 ******
Monday 05 September 2022  13:59:16 +0000 (0:00:00.037)       0:03:36.574 ******
Monday 05 September 2022  13:59:16 +0000 (0:00:00.036)       0:03:36.610 ******

TASK [container-engine/nerdctl : download_file | Download item] *******************************************************
ok: [node1 -> 192.168.88.211]
ok: [node3 -> 192.168.88.213]
ok: [node2 -> 192.168.88.212]
Monday 05 September 2022  13:59:18 +0000 (0:00:02.777)       0:03:39.387 ******
Monday 05 September 2022  13:59:19 +0000 (0:00:00.051)       0:03:39.439 ******
Monday 05 September 2022  13:59:19 +0000 (0:00:00.060)       0:03:39.500 ******
Monday 05 September 2022  13:59:19 +0000 (0:00:00.058)       0:03:39.559 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  13:59:19 +0000 (0:00:00.084)       0:03:39.644 ******

TASK [container-engine/nerdctl : extract_file | Unpacking archive] ****************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:59:22 +0000 (0:00:02.870)       0:03:42.514 ******

TASK [container-engine/nerdctl : nerdctl | Copy nerdctl binary from download dir] *************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:59:22 +0000 (0:00:00.418)       0:03:42.932 ******
included: /kubespray/roles/download/tasks/prep_kubeadm_images.yml for node1
Monday 05 September 2022  13:59:22 +0000 (0:00:00.076)       0:03:43.008 ******
Monday 05 September 2022  13:59:23 +0000 (0:00:00.408)       0:03:43.417 ******
included: /kubespray/roles/download/tasks/download_file.yml for node1
Monday 05 September 2022  13:59:23 +0000 (0:00:00.402)       0:03:43.820 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubeadm"
}
Monday 05 September 2022  13:59:23 +0000 (0:00:00.388)       0:03:44.208 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
Monday 05 September 2022  13:59:24 +0000 (0:00:00.423)       0:03:44.632 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node1]
Monday 05 September 2022  13:59:24 +0000 (0:00:00.679)       0:03:45.311 ******
Monday 05 September 2022  13:59:24 +0000 (0:00:00.031)       0:03:45.342 ******
Monday 05 September 2022  13:59:24 +0000 (0:00:00.034)       0:03:45.377 ******

TASK [download : download_file | Download item] ***********************************************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  13:59:33 +0000 (0:00:08.100)       0:03:53.478 ******
Monday 05 September 2022  13:59:33 +0000 (0:00:00.022)       0:03:53.500 ******
Monday 05 September 2022  13:59:33 +0000 (0:00:00.022)       0:03:53.523 ******
Monday 05 September 2022  13:59:33 +0000 (0:00:00.024)       0:03:53.547 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1
Monday 05 September 2022  13:59:33 +0000 (0:00:00.034)       0:03:53.581 ******
Monday 05 September 2022  13:59:33 +0000 (0:00:00.468)       0:03:54.050 ******
[WARNING]: noop task does not support when conditional

TASK [download : prep_kubeadm_images | Create kubeadm config] *********************************************************
changed: [node1]
Monday 05 September 2022  13:59:34 +0000 (0:00:00.511)       0:03:54.562 ******

TASK [download : prep_kubeadm_images | Copy kubeadm binary from download dir to system path] **************************
changed: [node1]
Monday 05 September 2022  13:59:34 +0000 (0:00:00.396)       0:03:54.958 ******

TASK [download : prep_kubeadm_images | Set kubeadm binary permissions] ************************************************
ok: [node1]
Monday 05 September 2022  13:59:34 +0000 (0:00:00.184)       0:03:55.142 ******

TASK [download : prep_kubeadm_images | Generate list of required images] **********************************************
ok: [node1]
Monday 05 September 2022  13:59:34 +0000 (0:00:00.208)       0:03:55.351 ******

TASK [download : prep_kubeadm_images | Parse list of images] **********************************************************
ok: [node1] => (item=k8s.gcr.io/kube-apiserver:v1.22.8)
ok: [node1] => (item=k8s.gcr.io/kube-controller-manager:v1.22.8)
ok: [node1] => (item=k8s.gcr.io/kube-scheduler:v1.22.8)
ok: [node1] => (item=k8s.gcr.io/kube-proxy:v1.22.8)
Monday 05 September 2022  13:59:35 +0000 (0:00:00.121)       0:03:55.472 ******

TASK [download : prep_kubeadm_images | Convert list of images to dict for later use] **********************************
ok: [node1]
Monday 05 September 2022  13:59:35 +0000 (0:00:00.035)       0:03:55.507 ******
included: /kubespray/roles/download/tasks/download_file.yml for node2, node3, node1 => (item={'key': 'cni', 'value': {'enabled': True, 'file': True, 'version': 'v1.0.1', 'dest': '/tmp/releases/cni-plugins-linux-amd64-v1.0.1.tgz', 'sha256': '5238fbb2767cbf6aae736ad97a7aa29167525dcd405196dfbc064672a730d3cf', 'url': 'https://github.com/containernetworking/plugins/releases/download/v1.0.1/cni-plugins-linux-amd64-v1.0.1.tgz', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node2, node3, node1 => (item={'key': 'kubeadm', 'value': {'enabled': True, 'file': True, 'version': 'v1.22.8', 'dest': '/tmp/releases/kubeadm-v1.22.8-amd64', 'sha256': 'fc10b4e5b66c9bfa6dc297bbb4a93f58051a6069c969905ef23c19680d8d49dc', 'url': 'https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubeadm', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node2, node3, node1 => (item={'key': 'kubelet', 'value': {'enabled': True, 'file': True, 'version': 'v1.22.8', 'dest': '/tmp/releases/kubelet-v1.22.8-amd64', 'sha256': '2e6d1774f18c4d4527c3b9197a64ea5705edcf1b547c77b3e683458d771f3ce7', 'url': 'https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubelet', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node2, node3, node1 => (item={'key': 'crictl', 'value': {'file': True, 'enabled': True, 'version': 'v1.22.0', 'dest': '/tmp/releases/crictl-v1.22.0-linux-amd64.tar.gz', 'sha256': '45e0556c42616af60ebe93bf4691056338b3ea0001c0201a6a8ff8b1dbc0652a', 'url': 'https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz', 'unarchive': True, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node2, node3, node1 => (item={'key': 'runc', 'value': {'file': True, 'enabled': True, 'version': 'v1.0.3', 'dest': '/tmp/releases/runc', 'sha256': '5d4c0e5a4e8d6ccbb9c6696bb239f31cfab8d94b15801bafe09aaee600714f61', 'url': 'https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node2, node3, node1 => (item={'key': 'containerd', 'value': {'enabled': True, 'file': True, 'version': '1.5.8', 'dest': '/tmp/releases/containerd-1.5.8-linux-amd64.tar.gz', 'sha256': 'feeda3f563edf0294e33b6c4b89bd7dbe0ee182ca61a2f9b8c3de2766bcbc99b', 'url': 'https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node2, node3, node1 => (item={'key': 'nerdctl', 'value': {'file': True, 'enabled': True, 'version': '0.15.0', 'dest': '/tmp/releases/nerdctl-0.15.0-linux-amd64.tar.gz', 'sha256': '1371da3f6bd461f331946654f6dd3ef2ef4b9da0dd7bc5f78ed1166f32ad5adc', 'url': 'https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz', 'unarchive': True, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node2, node3, node1 => (item={'key': 'calicoctl', 'value': {'enabled': True, 'file': True, 'version': 'v3.20.3', 'dest': '/tmp/releases/calicoctl', 'sha256': '29bec97b1dfc135b830b0cbfd3dfe216f00e97e9e6ef08e620d81d4a09db6393', 'url': 'https://github.com/projectcalico/calicoctl/releases/download/v3.20.3/calicoctl-linux-amd64', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'calico_node', 'value': {'enabled': True, 'container': True, 'repo': 'quay.io/calico/node', 'tag': 'v3.20.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'calico_cni', 'value': {'enabled': True, 'container': True, 'repo': 'quay.io/calico/cni', 'tag': 'v3.20.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'calico_flexvol', 'value': {'enabled': True, 'container': True, 'repo': 'quay.io/calico/pod2daemon-flexvol', 'tag': 'v3.20.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'calico_policy', 'value': {'enabled': True, 'container': True, 'repo': 'quay.io/calico/kube-controllers', 'tag': 'v3.20.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'pod_infra', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/pause', 'tag': '3.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'nginx', 'value': {'enabled': True, 'container': True, 'repo': 'docker.io/library/nginx', 'tag': '1.21.4', 'sha256': '', 'groups': ['kube_node']}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'nodelocaldns', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/dns/k8s-dns-node-cache', 'tag': '1.21.1', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'kubeadm_kube-apiserver', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/kube-apiserver', 'tag': 'v1.22.8', 'groups': 'k8s_cluster'}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'kubeadm_kube-controller-manager', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/kube-controller-manager', 'tag': 'v1.22.8', 'groups': 'k8s_cluster'}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'kubeadm_kube-scheduler', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/kube-scheduler', 'tag': 'v1.22.8', 'groups': 'k8s_cluster'}})
included: /kubespray/roles/download/tasks/download_container.yml for node2, node3, node1 => (item={'key': 'kubeadm_kube-proxy', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/kube-proxy', 'tag': 'v1.22.8', 'groups': 'k8s_cluster'}})
included: /kubespray/roles/download/tasks/download_file.yml for node1 => (item={'key': 'etcd', 'value': {'container': False, 'file': True, 'enabled': True, 'version': 'v3.5.0', 'dest': '/tmp/releases/etcd-v3.5.0-linux-amd64.tar.gz', 'repo': 'quay.io/coreos/etcd', 'tag': 'v3.5.0', 'sha256': '864baa0437f8368e0713d44b83afe21dce1fb4ee7dae4ca0f9dd5f0df22d01c4', 'url': 'https://github.com/coreos/etcd/releases/download/v3.5.0/etcd-v3.5.0-linux-amd64.tar.gz', 'unarchive': True, 'owner': 'root', 'mode': '0755', 'groups': ['etcd']}})
included: /kubespray/roles/download/tasks/download_file.yml for node1 => (item={'key': 'kubectl', 'value': {'enabled': True, 'file': True, 'version': 'v1.22.8', 'dest': '/tmp/releases/kubectl-v1.22.8-amd64', 'sha256': '761bf1f648056eeef753f84c8365afe4305795c5f605cd9be6a715483fe7ca6b', 'url': 'https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubectl', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['kube_control_plane']}})
included: /kubespray/roles/download/tasks/download_file.yml for node1 => (item={'key': 'calico_crds', 'value': {'file': True, 'enabled': True, 'version': 'v3.20.3', 'dest': '/tmp/releases/calico-v3.20.3-kdd-crds/v3.20.3.tar.gz', 'sha256': '2a3a5cbe05c60fa2fc850252c4eecfa36dd6629191ed805eea31f9b5c740bc4c', 'url': 'https://github.com/projectcalico/calico/archive/v3.20.3.tar.gz', 'unarchive': True, 'unarchive_extra_opts': ['--strip=6', '--wildcards', '*/_includes/charts/calico/crds/kdd/'], 'owner': 'root', 'mode': '0755', 'groups': ['kube_control_plane']}})
included: /kubespray/roles/download/tasks/download_container.yml for node1 => (item={'key': 'coredns', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/coredns/coredns', 'tag': 'v1.8.0', 'sha256': '', 'groups': ['kube_control_plane']}})
included: /kubespray/roles/download/tasks/download_container.yml for node1 => (item={'key': 'dnsautoscaler', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/cpa/cluster-proportional-autoscaler-amd64', 'tag': '1.8.5', 'sha256': '', 'groups': ['kube_control_plane']}})
Monday 05 September 2022  13:59:38 +0000 (0:00:03.164)       0:03:58.672 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://github.com/containernetworking/plugins/releases/download/v1.0.1/cni-plugins-linux-amd64-v1.0.1.tgz"
}
ok: [node2] => {
    "msg": "https://github.com/containernetworking/plugins/releases/download/v1.0.1/cni-plugins-linux-amd64-v1.0.1.tgz"
}
ok: [node3] => {
    "msg": "https://github.com/containernetworking/plugins/releases/download/v1.0.1/cni-plugins-linux-amd64-v1.0.1.tgz"
}
Monday 05 September 2022  13:59:38 +0000 (0:00:00.066)       0:03:58.739 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  13:59:38 +0000 (0:00:00.075)       0:03:58.814 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  13:59:38 +0000 (0:00:00.263)       0:03:59.078 ******
Monday 05 September 2022  13:59:38 +0000 (0:00:00.033)       0:03:59.111 ******
Monday 05 September 2022  13:59:38 +0000 (0:00:00.033)       0:03:59.145 ******

TASK [download : download_file | Download item] ***********************************************************************
changed: [node2 -> 192.168.88.212]
changed: [node1 -> 192.168.88.211]
changed: [node3 -> 192.168.88.213]
Monday 05 September 2022  14:00:02 +0000 (0:00:24.006)       0:04:23.151 ******
Monday 05 September 2022  14:00:02 +0000 (0:00:00.053)       0:04:23.205 ******
Monday 05 September 2022  14:00:02 +0000 (0:00:00.053)       0:04:23.258 ******
Monday 05 September 2022  14:00:02 +0000 (0:00:00.052)       0:04:23.311 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  14:00:02 +0000 (0:00:00.076)       0:04:23.388 ******
Monday 05 September 2022  14:00:03 +0000 (0:00:00.051)       0:04:23.439 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubeadm"
}
ok: [node2] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubeadm"
}
ok: [node3] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubeadm"
}
Monday 05 September 2022  14:00:03 +0000 (0:00:00.070)       0:04:23.510 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:00:03 +0000 (0:00:00.063)       0:04:23.573 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:00:03 +0000 (0:00:00.270)       0:04:23.844 ******
Monday 05 September 2022  14:00:03 +0000 (0:00:00.032)       0:04:23.876 ******
Monday 05 September 2022  14:00:03 +0000 (0:00:00.034)       0:04:23.911 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node1 -> 192.168.88.211]
changed: [node2 -> 192.168.88.212]
changed: [node3 -> 192.168.88.213]
Monday 05 September 2022  14:00:15 +0000 (0:00:11.664)       0:04:35.576 ******
Monday 05 September 2022  14:00:15 +0000 (0:00:00.055)       0:04:35.631 ******
Monday 05 September 2022  14:00:15 +0000 (0:00:00.054)       0:04:35.685 ******
Monday 05 September 2022  14:00:15 +0000 (0:00:00.056)       0:04:35.742 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  14:00:15 +0000 (0:00:00.079)       0:04:35.822 ******
Monday 05 September 2022  14:00:15 +0000 (0:00:00.060)       0:04:35.882 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubelet"
}
ok: [node2] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubelet"
}
ok: [node3] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubelet"
}
Monday 05 September 2022  14:00:15 +0000 (0:00:00.080)       0:04:35.962 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:00:15 +0000 (0:00:00.069)       0:04:36.031 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:00:15 +0000 (0:00:00.259)       0:04:36.291 ******
Monday 05 September 2022  14:00:15 +0000 (0:00:00.032)       0:04:36.324 ******
Monday 05 September 2022  14:00:15 +0000 (0:00:00.035)       0:04:36.359 ******

TASK [download : download_file | Download item] ***********************************************************************
changed: [node1 -> 192.168.88.211]
changed: [node3 -> 192.168.88.213]
changed: [node2 -> 192.168.88.212]
Monday 05 September 2022  14:00:51 +0000 (0:00:35.881)       0:05:12.241 ******
Monday 05 September 2022  14:00:51 +0000 (0:00:00.058)       0:05:12.299 ******
Monday 05 September 2022  14:00:51 +0000 (0:00:00.060)       0:05:12.360 ******
Monday 05 September 2022  14:00:52 +0000 (0:00:00.054)       0:05:12.415 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  14:00:52 +0000 (0:00:00.094)       0:05:12.509 ******
Monday 05 September 2022  14:00:52 +0000 (0:00:00.058)       0:05:12.568 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz"
}
ok: [node2] => {
    "msg": "https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz"
}
ok: [node3] => {
    "msg": "https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  14:00:52 +0000 (0:00:00.068)       0:05:12.636 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:00:52 +0000 (0:00:00.076)       0:05:12.713 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:00:52 +0000 (0:00:00.265)       0:05:12.978 ******
Monday 05 September 2022  14:00:52 +0000 (0:00:00.036)       0:05:13.015 ******
Monday 05 September 2022  14:00:52 +0000 (0:00:00.043)       0:05:13.058 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node1 -> 192.168.88.211]
ok: [node3 -> 192.168.88.213]
ok: [node2 -> 192.168.88.212]
Monday 05 September 2022  14:00:53 +0000 (0:00:00.436)       0:05:13.495 ******
Monday 05 September 2022  14:00:53 +0000 (0:00:00.061)       0:05:13.556 ******
Monday 05 September 2022  14:00:53 +0000 (0:00:00.053)       0:05:13.609 ******
Monday 05 September 2022  14:00:53 +0000 (0:00:00.056)       0:05:13.666 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  14:00:53 +0000 (0:00:00.080)       0:05:13.746 ******

TASK [download : extract_file | Unpacking archive] ********************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:00:54 +0000 (0:00:01.133)       0:05:14.880 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64"
}
ok: [node3] => {
    "msg": "https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64"
}
ok: [node2] => {
    "msg": "https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64"
}
Monday 05 September 2022  14:00:54 +0000 (0:00:00.065)       0:05:14.946 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:00:54 +0000 (0:00:00.065)       0:05:15.011 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:00:54 +0000 (0:00:00.272)       0:05:15.284 ******
Monday 05 September 2022  14:00:54 +0000 (0:00:00.033)       0:05:15.317 ******
Monday 05 September 2022  14:00:54 +0000 (0:00:00.034)       0:05:15.352 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node1 -> 192.168.88.211]
ok: [node2 -> 192.168.88.212]
ok: [node3 -> 192.168.88.213]
Monday 05 September 2022  14:00:55 +0000 (0:00:00.403)       0:05:15.756 ******
Monday 05 September 2022  14:00:55 +0000 (0:00:00.060)       0:05:15.816 ******
Monday 05 September 2022  14:00:55 +0000 (0:00:00.058)       0:05:15.875 ******
Monday 05 September 2022  14:00:55 +0000 (0:00:00.053)       0:05:15.928 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  14:00:55 +0000 (0:00:00.085)       0:05:16.014 ******
Monday 05 September 2022  14:00:55 +0000 (0:00:00.060)       0:05:16.075 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz"
}
ok: [node2] => {
    "msg": "https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz"
}
ok: [node3] => {
    "msg": "https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz"
}
Monday 05 September 2022  14:00:55 +0000 (0:00:00.082)       0:05:16.157 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:00:55 +0000 (0:00:00.075)       0:05:16.233 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:00:56 +0000 (0:00:00.261)       0:05:16.495 ******
Monday 05 September 2022  14:00:56 +0000 (0:00:00.035)       0:05:16.530 ******
Monday 05 September 2022  14:00:56 +0000 (0:00:00.034)       0:05:16.564 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node1 -> 192.168.88.211]
ok: [node2 -> 192.168.88.212]
ok: [node3 -> 192.168.88.213]
Monday 05 September 2022  14:00:56 +0000 (0:00:00.434)       0:05:16.999 ******
Monday 05 September 2022  14:00:56 +0000 (0:00:00.058)       0:05:17.058 ******
Monday 05 September 2022  14:00:56 +0000 (0:00:00.057)       0:05:17.115 ******
Monday 05 September 2022  14:00:56 +0000 (0:00:00.058)       0:05:17.173 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  14:00:56 +0000 (0:00:00.087)       0:05:17.261 ******
Monday 05 September 2022  14:00:56 +0000 (0:00:00.063)       0:05:17.324 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
ok: [node3] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
ok: [node2] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  14:00:57 +0000 (0:00:00.067)       0:05:17.391 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:00:57 +0000 (0:00:00.117)       0:05:17.509 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:00:57 +0000 (0:00:00.295)       0:05:17.804 ******
Monday 05 September 2022  14:00:57 +0000 (0:00:00.036)       0:05:17.840 ******
Monday 05 September 2022  14:00:57 +0000 (0:00:00.037)       0:05:17.878 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node1 -> 192.168.88.211]
ok: [node2 -> 192.168.88.212]
ok: [node3 -> 192.168.88.213]
Monday 05 September 2022  14:00:57 +0000 (0:00:00.462)       0:05:18.340 ******
Monday 05 September 2022  14:00:58 +0000 (0:00:00.055)       0:05:18.396 ******
Monday 05 September 2022  14:00:58 +0000 (0:00:00.050)       0:05:18.447 ******
Monday 05 September 2022  14:00:58 +0000 (0:00:00.052)       0:05:18.500 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  14:00:58 +0000 (0:00:00.081)       0:05:18.581 ******

TASK [download : extract_file | Unpacking archive] ********************************************************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  14:00:59 +0000 (0:00:00.969)       0:05:19.550 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://github.com/projectcalico/calicoctl/releases/download/v3.20.3/calicoctl-linux-amd64"
}
ok: [node2] => {
    "msg": "https://github.com/projectcalico/calicoctl/releases/download/v3.20.3/calicoctl-linux-amd64"
}
ok: [node3] => {
    "msg": "https://github.com/projectcalico/calicoctl/releases/download/v3.20.3/calicoctl-linux-amd64"
}
Monday 05 September 2022  14:00:59 +0000 (0:00:00.068)       0:05:19.618 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:00:59 +0000 (0:00:00.102)       0:05:19.721 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  14:00:59 +0000 (0:00:00.315)       0:05:20.037 ******
Monday 05 September 2022  14:00:59 +0000 (0:00:00.032)       0:05:20.069 ******
Monday 05 September 2022  14:00:59 +0000 (0:00:00.033)       0:05:20.103 ******

TASK [download : download_file | Download item] ***********************************************************************
changed: [node1 -> 192.168.88.211]
changed: [node3 -> 192.168.88.213]
changed: [node2 -> 192.168.88.212]
Monday 05 September 2022  14:01:24 +0000 (0:00:24.713)       0:05:44.816 ******
Monday 05 September 2022  14:01:24 +0000 (0:00:00.055)       0:05:44.872 ******
Monday 05 September 2022  14:01:24 +0000 (0:00:00.059)       0:05:44.931 ******
Monday 05 September 2022  14:01:24 +0000 (0:00:00.051)       0:05:44.982 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1, node2, node3
Monday 05 September 2022  14:01:24 +0000 (0:00:00.077)       0:05:45.060 ******
Monday 05 September 2022  14:01:24 +0000 (0:00:00.055)       0:05:45.115 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:24 +0000 (0:00:00.060)       0:05:45.176 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "quay.io/calico/node"
}
ok: [node2] => {
    "msg": "quay.io/calico/node"
}
ok: [node3] => {
    "msg": "quay.io/calico/node"
}
Monday 05 September 2022  14:01:24 +0000 (0:00:00.063)       0:05:45.239 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:24 +0000 (0:00:00.072)       0:05:45.311 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:25 +0000 (0:00:00.073)       0:05:45.385 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:25 +0000 (0:00:00.067)       0:05:45.453 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:25 +0000 (0:00:00.065)       0:05:45.518 ******
Monday 05 September 2022  14:01:25 +0000 (0:00:00.054)       0:05:45.573 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:25 +0000 (0:00:00.080)       0:05:45.653 ******
Monday 05 September 2022  14:01:25 +0000 (0:00:00.049)       0:05:45.703 ******
Monday 05 September 2022  14:01:25 +0000 (0:00:00.058)       0:05:45.761 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:01:25 +0000 (0:00:00.064)       0:05:45.826 ******
Monday 05 September 2022  14:01:25 +0000 (0:00:00.051)       0:05:45.878 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:01:25 +0000 (0:00:00.092)       0:05:45.970 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:01:25 +0000 (0:00:00.260)       0:05:46.230 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:25 +0000 (0:00:00.072)       0:05:46.303 ******
Monday 05 September 2022  14:01:25 +0000 (0:00:00.055)       0:05:46.359 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull quay.io/calico/node:v3.20.3 required is: True"
}
ok: [node2] => {
    "msg": "Pull quay.io/calico/node:v3.20.3 required is: True"
}
ok: [node3] => {
    "msg": "Pull quay.io/calico/node:v3.20.3 required is: True"
}
Monday 05 September 2022  14:01:26 +0000 (0:00:00.072)       0:05:46.432 ******
Monday 05 September 2022  14:01:26 +0000 (0:00:00.079)       0:05:46.511 ******
Monday 05 September 2022  14:01:26 +0000 (0:00:00.054)       0:05:46.566 ******
Monday 05 September 2022  14:01:26 +0000 (0:00:00.066)       0:05:46.632 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node3 -> 192.168.88.213]
changed: [node1 -> 192.168.88.211]
changed: [node2 -> 192.168.88.212]
Monday 05 September 2022  14:01:57 +0000 (0:00:31.106)       0:06:17.739 ******
Monday 05 September 2022  14:01:57 +0000 (0:00:00.035)       0:06:17.774 ******
Monday 05 September 2022  14:01:57 +0000 (0:00:00.062)       0:06:17.837 ******
Monday 05 September 2022  14:01:57 +0000 (0:00:00.070)       0:06:17.908 ******
Monday 05 September 2022  14:01:57 +0000 (0:00:00.082)       0:06:17.991 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:01:57 +0000 (0:00:00.251)       0:06:18.242 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:57 +0000 (0:00:00.066)       0:06:18.309 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "quay.io/calico/cni"
}
ok: [node2] => {
    "msg": "quay.io/calico/cni"
}
ok: [node3] => {
    "msg": "quay.io/calico/cni"
}
Monday 05 September 2022  14:01:57 +0000 (0:00:00.064)       0:06:18.373 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:58 +0000 (0:00:00.071)       0:06:18.444 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:58 +0000 (0:00:00.067)       0:06:18.512 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:58 +0000 (0:00:00.074)       0:06:18.587 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:58 +0000 (0:00:00.076)       0:06:18.663 ******
Monday 05 September 2022  14:01:58 +0000 (0:00:00.049)       0:06:18.713 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:58 +0000 (0:00:00.077)       0:06:18.791 ******
Monday 05 September 2022  14:01:58 +0000 (0:00:00.049)       0:06:18.841 ******
Monday 05 September 2022  14:01:58 +0000 (0:00:00.052)       0:06:18.893 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:58 +0000 (0:00:00.074)       0:06:18.967 ******
Monday 05 September 2022  14:01:58 +0000 (0:00:00.061)       0:06:19.029 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:01:58 +0000 (0:00:00.088)       0:06:19.117 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:59 +0000 (0:00:00.276)       0:06:19.394 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:01:59 +0000 (0:00:00.068)       0:06:19.463 ******
Monday 05 September 2022  14:01:59 +0000 (0:00:00.058)       0:06:19.521 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull quay.io/calico/cni:v3.20.3 required is: True"
}
ok: [node2] => {
    "msg": "Pull quay.io/calico/cni:v3.20.3 required is: True"
}
ok: [node3] => {
    "msg": "Pull quay.io/calico/cni:v3.20.3 required is: True"
}
Monday 05 September 2022  14:01:59 +0000 (0:00:00.068)       0:06:19.590 ******
Monday 05 September 2022  14:01:59 +0000 (0:00:00.060)       0:06:19.651 ******
Monday 05 September 2022  14:01:59 +0000 (0:00:00.057)       0:06:19.709 ******
Monday 05 September 2022  14:01:59 +0000 (0:00:00.063)       0:06:19.773 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node3 -> 192.168.88.213]
changed: [node2 -> 192.168.88.212]
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:02:24 +0000 (0:00:25.062)       0:06:44.835 ******
Monday 05 September 2022  14:02:24 +0000 (0:00:00.031)       0:06:44.867 ******
Monday 05 September 2022  14:02:24 +0000 (0:00:00.063)       0:06:44.930 ******
Monday 05 September 2022  14:02:24 +0000 (0:00:00.070)       0:06:45.001 ******
Monday 05 September 2022  14:02:24 +0000 (0:00:00.059)       0:06:45.060 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:02:25 +0000 (0:00:00.387)       0:06:45.447 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:25 +0000 (0:00:00.086)       0:06:45.534 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "quay.io/calico/pod2daemon-flexvol"
}
ok: [node3] => {
    "msg": "quay.io/calico/pod2daemon-flexvol"
}
ok: [node2] => {
    "msg": "quay.io/calico/pod2daemon-flexvol"
}
Monday 05 September 2022  14:02:25 +0000 (0:00:00.083)       0:06:45.618 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:25 +0000 (0:00:00.085)       0:06:45.703 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:25 +0000 (0:00:00.067)       0:06:45.770 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:25 +0000 (0:00:00.068)       0:06:45.839 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:25 +0000 (0:00:00.065)       0:06:45.904 ******
Monday 05 September 2022  14:02:25 +0000 (0:00:00.065)       0:06:45.970 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:25 +0000 (0:00:00.095)       0:06:46.065 ******
Monday 05 September 2022  14:02:25 +0000 (0:00:00.067)       0:06:46.133 ******
Monday 05 September 2022  14:02:25 +0000 (0:00:00.057)       0:06:46.190 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:25 +0000 (0:00:00.075)       0:06:46.265 ******
Monday 05 September 2022  14:02:25 +0000 (0:00:00.059)       0:06:46.325 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:02:26 +0000 (0:00:00.100)       0:06:46.426 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:26 +0000 (0:00:00.311)       0:06:46.738 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:26 +0000 (0:00:00.074)       0:06:46.812 ******
Monday 05 September 2022  14:02:26 +0000 (0:00:00.056)       0:06:46.869 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull quay.io/calico/pod2daemon-flexvol:v3.20.3 required is: True"
}
ok: [node2] => {
    "msg": "Pull quay.io/calico/pod2daemon-flexvol:v3.20.3 required is: True"
}
ok: [node3] => {
    "msg": "Pull quay.io/calico/pod2daemon-flexvol:v3.20.3 required is: True"
}
Monday 05 September 2022  14:02:26 +0000 (0:00:00.074)       0:06:46.943 ******
Monday 05 September 2022  14:02:26 +0000 (0:00:00.063)       0:06:47.007 ******
Monday 05 September 2022  14:02:26 +0000 (0:00:00.053)       0:06:47.060 ******
Monday 05 September 2022  14:02:26 +0000 (0:00:00.060)       0:06:47.121 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
changed: [node3 -> 192.168.88.213]
changed: [node2 -> 192.168.88.212]
Monday 05 September 2022  14:02:44 +0000 (0:00:18.190)       0:07:05.311 ******
Monday 05 September 2022  14:02:44 +0000 (0:00:00.029)       0:07:05.341 ******
Monday 05 September 2022  14:02:45 +0000 (0:00:00.063)       0:07:05.405 ******
Monday 05 September 2022  14:02:45 +0000 (0:00:00.055)       0:07:05.460 ******
Monday 05 September 2022  14:02:45 +0000 (0:00:00.057)       0:07:05.518 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:02:45 +0000 (0:00:00.248)       0:07:05.767 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:45 +0000 (0:00:00.064)       0:07:05.832 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "quay.io/calico/kube-controllers"
}
ok: [node2] => {
    "msg": "quay.io/calico/kube-controllers"
}
ok: [node3] => {
    "msg": "quay.io/calico/kube-controllers"
}
Monday 05 September 2022  14:02:45 +0000 (0:00:00.078)       0:07:05.910 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:45 +0000 (0:00:00.061)       0:07:05.972 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:45 +0000 (0:00:00.072)       0:07:06.045 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:45 +0000 (0:00:00.071)       0:07:06.116 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:45 +0000 (0:00:00.084)       0:07:06.201 ******
Monday 05 September 2022  14:02:45 +0000 (0:00:00.060)       0:07:06.262 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:45 +0000 (0:00:00.067)       0:07:06.329 ******
Monday 05 September 2022  14:02:45 +0000 (0:00:00.054)       0:07:06.383 ******
Monday 05 September 2022  14:02:46 +0000 (0:00:00.060)       0:07:06.444 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:46 +0000 (0:00:00.067)       0:07:06.511 ******
Monday 05 September 2022  14:02:46 +0000 (0:00:00.053)       0:07:06.564 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:02:46 +0000 (0:00:00.094)       0:07:06.658 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:02:46 +0000 (0:00:00.333)       0:07:06.992 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:02:46 +0000 (0:00:00.093)       0:07:07.085 ******
Monday 05 September 2022  14:02:46 +0000 (0:00:00.078)       0:07:07.164 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull quay.io/calico/kube-controllers:v3.20.3 required is: True"
}
ok: [node2] => {
    "msg": "Pull quay.io/calico/kube-controllers:v3.20.3 required is: True"
}
ok: [node3] => {
    "msg": "Pull quay.io/calico/kube-controllers:v3.20.3 required is: True"
}
Monday 05 September 2022  14:02:46 +0000 (0:00:00.069)       0:07:07.233 ******
Monday 05 September 2022  14:02:46 +0000 (0:00:00.064)       0:07:07.297 ******
Monday 05 September 2022  14:02:46 +0000 (0:00:00.053)       0:07:07.351 ******
Monday 05 September 2022  14:02:47 +0000 (0:00:00.083)       0:07:07.435 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
changed: [node2 -> 192.168.88.212]
changed: [node3 -> 192.168.88.213]
Monday 05 September 2022  14:03:11 +0000 (0:00:24.260)       0:07:31.695 ******
Monday 05 September 2022  14:03:11 +0000 (0:00:00.040)       0:07:31.735 ******
Monday 05 September 2022  14:03:11 +0000 (0:00:00.061)       0:07:31.796 ******
Monday 05 September 2022  14:03:11 +0000 (0:00:00.076)       0:07:31.873 ******
Monday 05 September 2022  14:03:11 +0000 (0:00:00.055)       0:07:31.929 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:11 +0000 (0:00:00.253)       0:07:32.182 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:11 +0000 (0:00:00.069)       0:07:32.251 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "k8s.gcr.io/pause"
}
ok: [node2] => {
    "msg": "k8s.gcr.io/pause"
}
ok: [node3] => {
    "msg": "k8s.gcr.io/pause"
}
Monday 05 September 2022  14:03:11 +0000 (0:00:00.065)       0:07:32.316 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:12 +0000 (0:00:00.089)       0:07:32.406 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:03:12 +0000 (0:00:00.124)       0:07:32.531 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:03:12 +0000 (0:00:00.084)       0:07:32.615 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:12 +0000 (0:00:00.065)       0:07:32.680 ******
Monday 05 September 2022  14:03:12 +0000 (0:00:00.053)       0:07:32.734 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:12 +0000 (0:00:00.064)       0:07:32.799 ******
Monday 05 September 2022  14:03:12 +0000 (0:00:00.049)       0:07:32.848 ******
Monday 05 September 2022  14:03:12 +0000 (0:00:00.070)       0:07:32.919 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:12 +0000 (0:00:00.099)       0:07:33.018 ******
Monday 05 September 2022  14:03:12 +0000 (0:00:00.054)       0:07:33.073 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:03:12 +0000 (0:00:00.087)       0:07:33.160 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:03:13 +0000 (0:00:00.315)       0:07:33.476 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:03:13 +0000 (0:00:00.068)       0:07:33.544 ******
Monday 05 September 2022  14:03:13 +0000 (0:00:00.057)       0:07:33.602 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull k8s.gcr.io/pause:3.3 required is: True"
}
ok: [node3] => {
    "msg": "Pull k8s.gcr.io/pause:3.3 required is: True"
}
ok: [node2] => {
    "msg": "Pull k8s.gcr.io/pause:3.3 required is: True"
}
Monday 05 September 2022  14:03:13 +0000 (0:00:00.062)       0:07:33.664 ******
Monday 05 September 2022  14:03:13 +0000 (0:00:00.058)       0:07:33.723 ******
Monday 05 September 2022  14:03:13 +0000 (0:00:00.064)       0:07:33.788 ******
Monday 05 September 2022  14:03:13 +0000 (0:00:00.062)       0:07:33.850 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
changed: [node2 -> 192.168.88.212]
changed: [node3 -> 192.168.88.213]
Monday 05 September 2022  14:03:20 +0000 (0:00:07.067)       0:07:40.918 ******
Monday 05 September 2022  14:03:20 +0000 (0:00:00.037)       0:07:40.955 ******
Monday 05 September 2022  14:03:20 +0000 (0:00:00.087)       0:07:41.042 ******
Monday 05 September 2022  14:03:20 +0000 (0:00:00.100)       0:07:41.143 ******
Monday 05 September 2022  14:03:20 +0000 (0:00:00.063)       0:07:41.207 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:03:21 +0000 (0:00:00.263)       0:07:41.470 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:03:21 +0000 (0:00:00.063)       0:07:41.533 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "docker.io/library/nginx"
}
ok: [node2] => {
    "msg": "docker.io/library/nginx"
}
ok: [node3] => {
    "msg": "docker.io/library/nginx"
}
Monday 05 September 2022  14:03:21 +0000 (0:00:00.068)       0:07:41.602 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:21 +0000 (0:00:00.070)       0:07:41.673 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:21 +0000 (0:00:00.072)       0:07:41.745 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:21 +0000 (0:00:00.063)       0:07:41.808 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:21 +0000 (0:00:00.060)       0:07:41.869 ******
Monday 05 September 2022  14:03:21 +0000 (0:00:00.056)       0:07:41.925 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:21 +0000 (0:00:00.070)       0:07:41.995 ******
Monday 05 September 2022  14:03:21 +0000 (0:00:00.053)       0:07:42.049 ******
Monday 05 September 2022  14:03:21 +0000 (0:00:00.058)       0:07:42.107 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:21 +0000 (0:00:00.066)       0:07:42.174 ******
Monday 05 September 2022  14:03:21 +0000 (0:00:00.050)       0:07:42.225 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:03:21 +0000 (0:00:00.088)       0:07:42.313 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:03:22 +0000 (0:00:00.332)       0:07:42.645 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:03:22 +0000 (0:00:00.070)       0:07:42.716 ******
Monday 05 September 2022  14:03:22 +0000 (0:00:00.060)       0:07:42.776 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull docker.io/library/nginx:1.21.4 required is: True"
}
ok: [node2] => {
    "msg": "Pull docker.io/library/nginx:1.21.4 required is: True"
}
ok: [node3] => {
    "msg": "Pull docker.io/library/nginx:1.21.4 required is: True"
}
Monday 05 September 2022  14:03:22 +0000 (0:00:00.074)       0:07:42.851 ******
Monday 05 September 2022  14:03:22 +0000 (0:00:00.060)       0:07:42.912 ******
Monday 05 September 2022  14:03:22 +0000 (0:00:00.052)       0:07:42.964 ******
Monday 05 September 2022  14:03:22 +0000 (0:00:00.069)       0:07:43.034 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
changed: [node2 -> 192.168.88.212]
changed: [node3 -> 192.168.88.213]
Monday 05 September 2022  14:03:51 +0000 (0:00:29.033)       0:08:12.067 ******
Monday 05 September 2022  14:03:51 +0000 (0:00:00.031)       0:08:12.098 ******
Monday 05 September 2022  14:03:51 +0000 (0:00:00.064)       0:08:12.163 ******
Monday 05 September 2022  14:03:51 +0000 (0:00:00.055)       0:08:12.218 ******
Monday 05 September 2022  14:03:51 +0000 (0:00:00.056)       0:08:12.275 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:03:52 +0000 (0:00:00.243)       0:08:12.518 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:52 +0000 (0:00:00.073)       0:08:12.592 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "k8s.gcr.io/dns/k8s-dns-node-cache"
}
ok: [node2] => {
    "msg": "k8s.gcr.io/dns/k8s-dns-node-cache"
}
ok: [node3] => {
    "msg": "k8s.gcr.io/dns/k8s-dns-node-cache"
}
Monday 05 September 2022  14:03:52 +0000 (0:00:00.077)       0:08:12.669 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:52 +0000 (0:00:00.073)       0:08:12.743 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:52 +0000 (0:00:00.064)       0:08:12.807 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:52 +0000 (0:00:00.072)       0:08:12.879 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:52 +0000 (0:00:00.076)       0:08:12.955 ******
Monday 05 September 2022  14:03:52 +0000 (0:00:00.055)       0:08:13.011 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:03:52 +0000 (0:00:00.184)       0:08:13.195 ******
Monday 05 September 2022  14:03:52 +0000 (0:00:00.052)       0:08:13.248 ******
Monday 05 September 2022  14:03:52 +0000 (0:00:00.056)       0:08:13.305 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:52 +0000 (0:00:00.071)       0:08:13.376 ******
Monday 05 September 2022  14:03:53 +0000 (0:00:00.053)       0:08:13.429 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:03:53 +0000 (0:00:00.096)       0:08:13.526 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:03:53 +0000 (0:00:00.354)       0:08:13.880 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:03:53 +0000 (0:00:00.070)       0:08:13.951 ******
Monday 05 September 2022  14:03:53 +0000 (0:00:00.056)       0:08:14.008 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull k8s.gcr.io/dns/k8s-dns-node-cache:1.21.1 required is: True"
}
ok: [node2] => {
    "msg": "Pull k8s.gcr.io/dns/k8s-dns-node-cache:1.21.1 required is: True"
}
ok: [node3] => {
    "msg": "Pull k8s.gcr.io/dns/k8s-dns-node-cache:1.21.1 required is: True"
}
Monday 05 September 2022  14:03:53 +0000 (0:00:00.076)       0:08:14.084 ******
Monday 05 September 2022  14:03:53 +0000 (0:00:00.054)       0:08:14.139 ******
Monday 05 September 2022  14:03:53 +0000 (0:00:00.058)       0:08:14.197 ******
Monday 05 September 2022  14:03:53 +0000 (0:00:00.061)       0:08:14.259 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
changed: [node3 -> 192.168.88.213]
changed: [node2 -> 192.168.88.212]
Monday 05 September 2022  14:04:12 +0000 (0:00:18.872)       0:08:33.131 ******
Monday 05 September 2022  14:04:12 +0000 (0:00:00.031)       0:08:33.163 ******
Monday 05 September 2022  14:04:12 +0000 (0:00:00.057)       0:08:33.221 ******
Monday 05 September 2022  14:04:12 +0000 (0:00:00.055)       0:08:33.276 ******
Monday 05 September 2022  14:04:12 +0000 (0:00:00.059)       0:08:33.335 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:13 +0000 (0:00:00.260)       0:08:33.596 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:13 +0000 (0:00:00.068)       0:08:33.664 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "k8s.gcr.io/kube-apiserver"
}
ok: [node2] => {
    "msg": "k8s.gcr.io/kube-apiserver"
}
ok: [node3] => {
    "msg": "k8s.gcr.io/kube-apiserver"
}
Monday 05 September 2022  14:04:13 +0000 (0:00:00.068)       0:08:33.732 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:13 +0000 (0:00:00.077)       0:08:33.810 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:13 +0000 (0:00:00.080)       0:08:33.891 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:13 +0000 (0:00:00.062)       0:08:33.953 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:13 +0000 (0:00:00.068)       0:08:34.022 ******
Monday 05 September 2022  14:04:13 +0000 (0:00:00.051)       0:08:34.073 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:13 +0000 (0:00:00.072)       0:08:34.146 ******
Monday 05 September 2022  14:04:13 +0000 (0:00:00.051)       0:08:34.197 ******
Monday 05 September 2022  14:04:13 +0000 (0:00:00.064)       0:08:34.262 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:13 +0000 (0:00:00.076)       0:08:34.339 ******
Monday 05 September 2022  14:04:14 +0000 (0:00:00.054)       0:08:34.393 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:04:14 +0000 (0:00:00.094)       0:08:34.488 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:04:14 +0000 (0:00:00.410)       0:08:34.898 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:14 +0000 (0:00:00.072)       0:08:34.971 ******
Monday 05 September 2022  14:04:14 +0000 (0:00:00.058)       0:08:35.029 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull k8s.gcr.io/kube-apiserver:v1.22.8 required is: True"
}
ok: [node2] => {
    "msg": "Pull k8s.gcr.io/kube-apiserver:v1.22.8 required is: True"
}
ok: [node3] => {
    "msg": "Pull k8s.gcr.io/kube-apiserver:v1.22.8 required is: True"
}
Monday 05 September 2022  14:04:14 +0000 (0:00:00.081)       0:08:35.110 ******
Monday 05 September 2022  14:04:14 +0000 (0:00:00.060)       0:08:35.170 ******
Monday 05 September 2022  14:04:14 +0000 (0:00:00.053)       0:08:35.224 ******
Monday 05 September 2022  14:04:14 +0000 (0:00:00.057)       0:08:35.281 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
changed: [node3 -> 192.168.88.213]
changed: [node2 -> 192.168.88.212]
Monday 05 September 2022  14:04:30 +0000 (0:00:15.106)       0:08:50.388 ******
Monday 05 September 2022  14:04:30 +0000 (0:00:00.037)       0:08:50.425 ******
Monday 05 September 2022  14:04:30 +0000 (0:00:00.064)       0:08:50.490 ******
Monday 05 September 2022  14:04:30 +0000 (0:00:00.060)       0:08:50.551 ******
Monday 05 September 2022  14:04:30 +0000 (0:00:00.056)       0:08:50.608 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:30 +0000 (0:00:00.254)       0:08:50.863 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:30 +0000 (0:00:00.069)       0:08:50.933 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "k8s.gcr.io/kube-controller-manager"
}
ok: [node2] => {
    "msg": "k8s.gcr.io/kube-controller-manager"
}
ok: [node3] => {
    "msg": "k8s.gcr.io/kube-controller-manager"
}
Monday 05 September 2022  14:04:30 +0000 (0:00:00.077)       0:08:51.010 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:30 +0000 (0:00:00.070)       0:08:51.080 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:30 +0000 (0:00:00.066)       0:08:51.146 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:30 +0000 (0:00:00.069)       0:08:51.216 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:30 +0000 (0:00:00.069)       0:08:51.285 ******
Monday 05 September 2022  14:04:30 +0000 (0:00:00.056)       0:08:51.341 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:31 +0000 (0:00:00.066)       0:08:51.408 ******
Monday 05 September 2022  14:04:31 +0000 (0:00:00.055)       0:08:51.463 ******
Monday 05 September 2022  14:04:31 +0000 (0:00:00.064)       0:08:51.528 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:31 +0000 (0:00:00.079)       0:08:51.608 ******
Monday 05 September 2022  14:04:31 +0000 (0:00:00.056)       0:08:51.664 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:04:31 +0000 (0:00:00.096)       0:08:51.761 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  14:04:31 +0000 (0:00:00.429)       0:08:52.190 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:31 +0000 (0:00:00.074)       0:08:52.264 ******
Monday 05 September 2022  14:04:31 +0000 (0:00:00.060)       0:08:52.325 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull k8s.gcr.io/kube-controller-manager:v1.22.8 required is: True"
}
ok: [node2] => {
    "msg": "Pull k8s.gcr.io/kube-controller-manager:v1.22.8 required is: True"
}
ok: [node3] => {
    "msg": "Pull k8s.gcr.io/kube-controller-manager:v1.22.8 required is: True"
}
Monday 05 September 2022  14:04:32 +0000 (0:00:00.083)       0:08:52.408 ******
Monday 05 September 2022  14:04:32 +0000 (0:00:00.084)       0:08:52.493 ******
Monday 05 September 2022  14:04:32 +0000 (0:00:00.057)       0:08:52.551 ******
Monday 05 September 2022  14:04:32 +0000 (0:00:00.059)       0:08:52.610 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node3 -> 192.168.88.213]
changed: [node1 -> 192.168.88.211]
changed: [node2 -> 192.168.88.212]
Monday 05 September 2022  14:04:46 +0000 (0:00:14.625)       0:09:07.236 ******
Monday 05 September 2022  14:04:46 +0000 (0:00:00.030)       0:09:07.266 ******
Monday 05 September 2022  14:04:46 +0000 (0:00:00.060)       0:09:07.326 ******
Monday 05 September 2022  14:04:47 +0000 (0:00:00.062)       0:09:07.388 ******
Monday 05 September 2022  14:04:47 +0000 (0:00:00.095)       0:09:07.484 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:04:47 +0000 (0:00:00.260)       0:09:07.745 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:47 +0000 (0:00:00.118)       0:09:07.863 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "k8s.gcr.io/kube-scheduler"
}
ok: [node3] => {
    "msg": "k8s.gcr.io/kube-scheduler"
}
ok: [node2] => {
    "msg": "k8s.gcr.io/kube-scheduler"
}
Monday 05 September 2022  14:04:47 +0000 (0:00:00.072)       0:09:07.936 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:47 +0000 (0:00:00.070)       0:09:08.007 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:04:47 +0000 (0:00:00.078)       0:09:08.085 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:04:47 +0000 (0:00:00.063)       0:09:08.149 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:47 +0000 (0:00:00.076)       0:09:08.226 ******
Monday 05 September 2022  14:04:47 +0000 (0:00:00.057)       0:09:08.283 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:47 +0000 (0:00:00.068)       0:09:08.351 ******
Monday 05 September 2022  14:04:48 +0000 (0:00:00.061)       0:09:08.413 ******
Monday 05 September 2022  14:04:48 +0000 (0:00:00.063)       0:09:08.476 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:48 +0000 (0:00:00.073)       0:09:08.550 ******
Monday 05 September 2022  14:04:48 +0000 (0:00:00.060)       0:09:08.610 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node2, node3
Monday 05 September 2022  14:04:48 +0000 (0:00:00.102)       0:09:08.713 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:04:48 +0000 (0:00:00.426)       0:09:09.140 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:04:48 +0000 (0:00:00.071)       0:09:09.211 ******
Monday 05 September 2022  14:04:48 +0000 (0:00:00.060)       0:09:09.272 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull k8s.gcr.io/kube-scheduler:v1.22.8 required is: True"
}
ok: [node2] => {
    "msg": "Pull k8s.gcr.io/kube-scheduler:v1.22.8 required is: True"
}
ok: [node3] => {
    "msg": "Pull k8s.gcr.io/kube-scheduler:v1.22.8 required is: True"
}
Monday 05 September 2022  14:04:48 +0000 (0:00:00.076)       0:09:09.349 ******
Monday 05 September 2022  14:04:49 +0000 (0:00:00.061)       0:09:09.410 ******
Monday 05 September 2022  14:04:49 +0000 (0:00:00.057)       0:09:09.468 ******
Monday 05 September 2022  14:04:49 +0000 (0:00:00.064)       0:09:09.532 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
changed: [node3 -> 192.168.88.213]
changed: [node2 -> 192.168.88.212]
Monday 05 September 2022  14:05:00 +0000 (0:00:11.092)       0:09:20.624 ******
Monday 05 September 2022  14:05:00 +0000 (0:00:00.038)       0:09:20.663 ******
Monday 05 September 2022  14:05:00 +0000 (0:00:00.075)       0:09:20.738 ******
Monday 05 September 2022  14:05:00 +0000 (0:00:00.058)       0:09:20.796 ******
Monday 05 September 2022  14:05:00 +0000 (0:00:00.057)       0:09:20.854 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:05:00 +0000 (0:00:00.261)       0:09:21.116 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:05:00 +0000 (0:00:00.082)       0:09:21.198 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "k8s.gcr.io/kube-proxy"
}
ok: [node2] => {
    "msg": "k8s.gcr.io/kube-proxy"
}
ok: [node3] => {
    "msg": "k8s.gcr.io/kube-proxy"
}
Monday 05 September 2022  14:05:00 +0000 (0:00:00.072)       0:09:21.270 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:05:00 +0000 (0:00:00.073)       0:09:21.344 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:05:01 +0000 (0:00:00.075)       0:09:21.420 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:05:01 +0000 (0:00:00.082)       0:09:21.502 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:05:01 +0000 (0:00:00.087)       0:09:21.589 ******
Monday 05 September 2022  14:05:01 +0000 (0:00:00.064)       0:09:21.654 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:05:01 +0000 (0:00:00.082)       0:09:21.737 ******
Monday 05 September 2022  14:05:01 +0000 (0:00:00.062)       0:09:21.799 ******
Monday 05 September 2022  14:05:01 +0000 (0:00:00.069)       0:09:21.868 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:05:01 +0000 (0:00:00.095)       0:09:21.964 ******
Monday 05 September 2022  14:05:01 +0000 (0:00:00.070)       0:09:22.035 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1, node3, node2
Monday 05 September 2022  14:05:01 +0000 (0:00:00.154)       0:09:22.189 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:05:02 +0000 (0:00:00.625)       0:09:22.815 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:05:02 +0000 (0:00:00.098)       0:09:22.913 ******
Monday 05 September 2022  14:05:02 +0000 (0:00:00.067)       0:09:22.981 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull k8s.gcr.io/kube-proxy:v1.22.8 required is: True"
}
ok: [node2] => {
    "msg": "Pull k8s.gcr.io/kube-proxy:v1.22.8 required is: True"
}
ok: [node3] => {
    "msg": "Pull k8s.gcr.io/kube-proxy:v1.22.8 required is: True"
}
Monday 05 September 2022  14:05:02 +0000 (0:00:00.077)       0:09:23.058 ******
Monday 05 September 2022  14:05:02 +0000 (0:00:00.061)       0:09:23.120 ******
Monday 05 September 2022  14:05:02 +0000 (0:00:00.065)       0:09:23.186 ******
Monday 05 September 2022  14:05:02 +0000 (0:00:00.063)       0:09:23.249 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
changed: [node2 -> 192.168.88.212]
changed: [node3 -> 192.168.88.213]
Monday 05 September 2022  14:05:12 +0000 (0:00:09.843)       0:09:33.093 ******
Monday 05 September 2022  14:05:12 +0000 (0:00:00.030)       0:09:33.123 ******
Monday 05 September 2022  14:05:12 +0000 (0:00:00.061)       0:09:33.185 ******
Monday 05 September 2022  14:05:12 +0000 (0:00:00.064)       0:09:33.249 ******
Monday 05 September 2022  14:05:12 +0000 (0:00:00.062)       0:09:33.312 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:05:13 +0000 (0:00:00.265)       0:09:33.577 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://github.com/coreos/etcd/releases/download/v3.5.0/etcd-v3.5.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  14:05:13 +0000 (0:00:00.037)       0:09:33.614 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
Monday 05 September 2022  14:05:13 +0000 (0:00:00.034)       0:09:33.649 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node1]
Monday 05 September 2022  14:05:13 +0000 (0:00:00.232)       0:09:33.882 ******
Monday 05 September 2022  14:05:13 +0000 (0:00:00.035)       0:09:33.918 ******
Monday 05 September 2022  14:05:13 +0000 (0:00:00.033)       0:09:33.952 ******

TASK [download : download_file | Download item] ***********************************************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:05:31 +0000 (0:00:18.038)       0:09:51.990 ******
Monday 05 September 2022  14:05:31 +0000 (0:00:00.028)       0:09:52.018 ******
Monday 05 September 2022  14:05:31 +0000 (0:00:00.036)       0:09:52.055 ******
Monday 05 September 2022  14:05:31 +0000 (0:00:00.036)       0:09:52.091 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1
Monday 05 September 2022  14:05:31 +0000 (0:00:00.051)       0:09:52.143 ******

TASK [download : extract_file | Unpacking archive] ********************************************************************
changed: [node1]
Monday 05 September 2022  14:05:33 +0000 (0:00:01.490)       0:09:53.634 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubectl"
}
Monday 05 September 2022  14:05:33 +0000 (0:00:00.032)       0:09:53.666 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
Monday 05 September 2022  14:05:33 +0000 (0:00:00.031)       0:09:53.698 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node1]
Monday 05 September 2022  14:05:33 +0000 (0:00:00.200)       0:09:53.898 ******
Monday 05 September 2022  14:05:33 +0000 (0:00:00.030)       0:09:53.929 ******
Monday 05 September 2022  14:05:33 +0000 (0:00:00.032)       0:09:53.961 ******

TASK [download : download_file | Download item] ***********************************************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:05:40 +0000 (0:00:06.956)       0:10:00.918 ******
Monday 05 September 2022  14:05:40 +0000 (0:00:00.024)       0:10:00.942 ******
Monday 05 September 2022  14:05:40 +0000 (0:00:00.055)       0:10:00.998 ******
Monday 05 September 2022  14:05:40 +0000 (0:00:00.024)       0:10:01.023 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1
Monday 05 September 2022  14:05:40 +0000 (0:00:00.036)       0:10:01.059 ******
Monday 05 September 2022  14:05:40 +0000 (0:00:00.026)       0:10:01.085 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node1] => {
    "msg": "https://github.com/projectcalico/calico/archive/v3.20.3.tar.gz"
}
Monday 05 September 2022  14:05:40 +0000 (0:00:00.032)       0:10:01.118 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node1]
Monday 05 September 2022  14:05:40 +0000 (0:00:00.032)       0:10:01.150 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
changed: [node1]
Monday 05 September 2022  14:05:40 +0000 (0:00:00.187)       0:10:01.338 ******
Monday 05 September 2022  14:05:40 +0000 (0:00:00.032)       0:10:01.371 ******
Monday 05 September 2022  14:05:41 +0000 (0:00:00.033)       0:10:01.404 ******

TASK [download : download_file | Download item] ***********************************************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:05:52 +0000 (0:00:11.386)       0:10:12.791 ******
Monday 05 September 2022  14:05:52 +0000 (0:00:00.023)       0:10:12.814 ******
Monday 05 September 2022  14:05:52 +0000 (0:00:00.025)       0:10:12.839 ******
Monday 05 September 2022  14:05:52 +0000 (0:00:00.023)       0:10:12.863 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node1
Monday 05 September 2022  14:05:52 +0000 (0:00:00.034)       0:10:12.898 ******

TASK [download : extract_file | Unpacking archive] ********************************************************************
changed: [node1]
Monday 05 September 2022  14:05:53 +0000 (0:00:00.931)       0:10:13.830 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
Monday 05 September 2022  14:05:53 +0000 (0:00:00.029)       0:10:13.859 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "k8s.gcr.io/coredns/coredns"
}
Monday 05 September 2022  14:05:53 +0000 (0:00:00.032)       0:10:13.891 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
Monday 05 September 2022  14:05:53 +0000 (0:00:00.036)       0:10:13.928 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
Monday 05 September 2022  14:05:53 +0000 (0:00:00.036)       0:10:13.964 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
Monday 05 September 2022  14:05:53 +0000 (0:00:00.032)       0:10:13.997 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
Monday 05 September 2022  14:05:53 +0000 (0:00:00.030)       0:10:14.028 ******
Monday 05 September 2022  14:05:53 +0000 (0:00:00.023)       0:10:14.051 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
Monday 05 September 2022  14:05:53 +0000 (0:00:00.031)       0:10:14.083 ******
Monday 05 September 2022  14:05:53 +0000 (0:00:00.022)       0:10:14.106 ******
Monday 05 September 2022  14:05:53 +0000 (0:00:00.025)       0:10:14.132 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
Monday 05 September 2022  14:05:53 +0000 (0:00:00.035)       0:10:14.167 ******
Monday 05 September 2022  14:05:53 +0000 (0:00:00.023)       0:10:14.191 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1
Monday 05 September 2022  14:05:53 +0000 (0:00:00.041)       0:10:14.232 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node1]
Monday 05 September 2022  14:05:54 +0000 (0:00:00.382)       0:10:14.614 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
Monday 05 September 2022  14:05:54 +0000 (0:00:00.034)       0:10:14.649 ******
Monday 05 September 2022  14:05:54 +0000 (0:00:00.025)       0:10:14.674 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull k8s.gcr.io/coredns/coredns:v1.8.0 required is: True"
}
Monday 05 September 2022  14:05:54 +0000 (0:00:00.035)       0:10:14.710 ******
Monday 05 September 2022  14:05:54 +0000 (0:00:00.024)       0:10:14.734 ******
Monday 05 September 2022  14:05:54 +0000 (0:00:00.022)       0:10:14.757 ******
Monday 05 September 2022  14:05:54 +0000 (0:00:00.030)       0:10:14.788 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:06:02 +0000 (0:00:07.653)       0:10:22.442 ******
Monday 05 September 2022  14:06:02 +0000 (0:00:00.040)       0:10:22.482 ******
Monday 05 September 2022  14:06:02 +0000 (0:00:00.024)       0:10:22.507 ******
Monday 05 September 2022  14:06:02 +0000 (0:00:00.024)       0:10:22.531 ******
Monday 05 September 2022  14:06:02 +0000 (0:00:00.023)       0:10:22.555 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]
Monday 05 September 2022  14:06:02 +0000 (0:00:00.190)       0:10:22.746 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node1]
Monday 05 September 2022  14:06:02 +0000 (0:00:00.043)       0:10:22.789 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node1] => {
    "msg": "k8s.gcr.io/cpa/cluster-proportional-autoscaler-amd64"
}
Monday 05 September 2022  14:06:02 +0000 (0:00:00.055)       0:10:22.845 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node1]
Monday 05 September 2022  14:06:02 +0000 (0:00:00.035)       0:10:22.880 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node1]
Monday 05 September 2022  14:06:02 +0000 (0:00:00.034)       0:10:22.915 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node1]
Monday 05 September 2022  14:06:02 +0000 (0:00:00.034)       0:10:22.950 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node1]
Monday 05 September 2022  14:06:02 +0000 (0:00:00.032)       0:10:22.982 ******
Monday 05 September 2022  14:06:02 +0000 (0:00:00.024)       0:10:23.006 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node1]
Monday 05 September 2022  14:06:02 +0000 (0:00:00.038)       0:10:23.045 ******
Monday 05 September 2022  14:06:02 +0000 (0:00:00.027)       0:10:23.072 ******
Monday 05 September 2022  14:06:02 +0000 (0:00:00.024)       0:10:23.096 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node1]
Monday 05 September 2022  14:06:02 +0000 (0:00:00.035)       0:10:23.131 ******
Monday 05 September 2022  14:06:02 +0000 (0:00:00.022)       0:10:23.154 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node1
Monday 05 September 2022  14:06:02 +0000 (0:00:00.053)       0:10:23.207 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node1]
Monday 05 September 2022  14:06:03 +0000 (0:00:00.370)       0:10:23.578 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node1]
Monday 05 September 2022  14:06:03 +0000 (0:00:00.032)       0:10:23.610 ******
Monday 05 September 2022  14:06:03 +0000 (0:00:00.023)       0:10:23.633 ******

TASK [download : debug] ***********************************************************************************************
ok: [node1] => {
    "msg": "Pull k8s.gcr.io/cpa/cluster-proportional-autoscaler-amd64:1.8.5 required is: True"
}
Monday 05 September 2022  14:06:03 +0000 (0:00:00.036)       0:10:23.670 ******
Monday 05 September 2022  14:06:03 +0000 (0:00:00.022)       0:10:23.692 ******
Monday 05 September 2022  14:06:03 +0000 (0:00:00.023)       0:10:23.715 ******
Monday 05 September 2022  14:06:03 +0000 (0:00:00.029)       0:10:23.745 ******

TASK [download : download_container | Download image if required] *****************************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:06:10 +0000 (0:00:07.192)       0:10:30.938 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.031)       0:10:30.969 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.025)       0:10:30.995 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.022)       0:10:31.018 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.024)       0:10:31.042 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node1]

PLAY [etcd] ***********************************************************************************************************
Monday 05 September 2022  14:06:10 +0000 (0:00:00.207)       0:10:31.249 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.022)       0:10:31.272 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.022)       0:10:31.294 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.026)       0:10:31.321 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.023)       0:10:31.344 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.021)       0:10:31.365 ******
Monday 05 September 2022  14:06:10 +0000 (0:00:00.022)       0:10:31.388 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.025)       0:10:31.414 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.032)       0:10:31.446 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.034)       0:10:31.480 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.030)       0:10:31.511 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.031)       0:10:31.543 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.031)       0:10:31.574 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.042)       0:10:31.616 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.032)       0:10:31.648 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.036)       0:10:31.685 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.029)       0:10:31.714 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.021)       0:10:31.736 ******
Monday 05 September 2022  14:06:11 +0000 (0:00:00.023)       0:10:31.760 ******
Monday 05 September 2022  14:06:12 +0000 (0:00:00.871)       0:10:32.631 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  14:06:12 +0000 (0:00:00.029)       0:10:32.660 ******
Monday 05 September 2022  14:06:12 +0000 (0:00:00.057)       0:10:32.718 ******
Monday 05 September 2022  14:06:12 +0000 (0:00:00.033)       0:10:32.751 ******
Monday 05 September 2022  14:06:12 +0000 (0:00:00.025)       0:10:32.776 ******
Monday 05 September 2022  14:06:12 +0000 (0:00:00.025)       0:10:32.801 ******
Monday 05 September 2022  14:06:12 +0000 (0:00:00.023)       0:10:32.825 ******

TASK [adduser : User | Create User Group] *****************************************************************************
changed: [node1]
Monday 05 September 2022  14:06:12 +0000 (0:00:00.420)       0:10:33.246 ******

TASK [adduser : User | Create User] ***********************************************************************************
changed: [node1]
Monday 05 September 2022  14:06:13 +0000 (0:00:00.435)       0:10:33.681 ******

TASK [adduser : User | Create User Group] *****************************************************************************
ok: [node1]
Monday 05 September 2022  14:06:13 +0000 (0:00:00.189)       0:10:33.870 ******

TASK [adduser : User | Create User] ***********************************************************************************
ok: [node1]
Monday 05 September 2022  14:06:13 +0000 (0:00:00.226)       0:10:34.097 ******
included: /kubespray/roles/etcd/tasks/check_certs.yml for node1
Monday 05 September 2022  14:06:13 +0000 (0:00:00.043)       0:10:34.140 ******

TASK [etcd : Check_certs | Register certs that have already been generated on first etcd node] ************************
ok: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:06:13 +0000 (0:00:00.244)       0:10:34.384 ******

TASK [etcd : Check_certs | Set default value for 'sync_certs', 'gen_certs' and 'etcd_secret_changed' to false] ********
ok: [node1]
Monday 05 September 2022  14:06:14 +0000 (0:00:00.031)       0:10:34.416 ******

TASK [etcd : Check certs | Register ca and etcd admin/member certs on etcd hosts] *************************************
ok: [node1] => (item=ca.pem)
ok: [node1] => (item=member-node1.pem)
ok: [node1] => (item=member-node1-key.pem)
ok: [node1] => (item=admin-node1.pem)
ok: [node1] => (item=admin-node1-key.pem)
Monday 05 September 2022  14:06:14 +0000 (0:00:00.828)       0:10:35.245 ******

TASK [etcd : Check certs | Register ca and etcd node certs on kubernetes hosts] ***************************************
ok: [node1] => (item=ca.pem)
ok: [node1] => (item=node-node1.pem)
ok: [node1] => (item=node-node1-key.pem)
Monday 05 September 2022  14:06:15 +0000 (0:00:00.501)       0:10:35.746 ******

TASK [etcd : Check_certs | Set 'gen_certs' to true if expected certificates are not on the first etcd node] ***********
ok: [node1] => (item=/etc/ssl/etcd/ssl/ca.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/admin-node1.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/admin-node1-key.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/member-node1.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/member-node1-key.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node1.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node1-key.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node2.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node2-key.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node3.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node3-key.pem)
Monday 05 September 2022  14:06:15 +0000 (0:00:00.192)       0:10:35.939 ******

TASK [etcd : Check_certs | Set 'gen_master_certs' object to track whether member and admin certs exist on first etcd node] ***
ok: [node1]
Monday 05 September 2022  14:06:15 +0000 (0:00:00.035)       0:10:35.975 ******

TASK [etcd : Check_certs | Set 'gen_node_certs' object to track whether node certs exist on first etcd node] **********
ok: [node1]
Monday 05 September 2022  14:06:15 +0000 (0:00:00.036)       0:10:36.012 ******

TASK [etcd : Check_certs | Set 'etcd_member_requires_sync' to true if ca or member/admin cert and key don't exist on etcd member or checksum doesn't match] ***
ok: [node1]
Monday 05 September 2022  14:06:15 +0000 (0:00:00.047)       0:10:36.060 ******
Monday 05 September 2022  14:06:15 +0000 (0:00:00.024)       0:10:36.084 ******

TASK [etcd : Check_certs | Set 'sync_certs' to true] ******************************************************************
ok: [node1]
Monday 05 September 2022  14:06:15 +0000 (0:00:00.033)       0:10:36.117 ******
included: /kubespray/roles/etcd/tasks/gen_certs_script.yml for node1
Monday 05 September 2022  14:06:15 +0000 (0:00:00.042)       0:10:36.160 ******

TASK [etcd : Gen_certs | create etcd cert dir] ************************************************************************
changed: [node1]
Monday 05 September 2022  14:06:15 +0000 (0:00:00.187)       0:10:36.347 ******

TASK [etcd : Gen_certs | create etcd script dir (on node1)] ***********************************************************
changed: [node1]
Monday 05 September 2022  14:06:16 +0000 (0:00:00.188)       0:10:36.535 ******

TASK [etcd : Gen_certs | write openssl config] ************************************************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:06:16 +0000 (0:00:00.506)       0:10:37.041 ******

TASK [etcd : Gen_certs | copy certs generation script] ****************************************************************
changed: [node1]
Monday 05 September 2022  14:06:17 +0000 (0:00:00.527)       0:10:37.568 ******

TASK [etcd : Gen_certs | run cert generation script] ******************************************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:06:17 +0000 (0:00:00.706)       0:10:38.275 ******
Monday 05 September 2022  14:06:17 +0000 (0:00:00.094)       0:10:38.369 ******
Monday 05 September 2022  14:06:18 +0000 (0:00:00.106)       0:10:38.476 ******
Monday 05 September 2022  14:06:18 +0000 (0:00:00.087)       0:10:38.564 ******
Monday 05 September 2022  14:06:18 +0000 (0:00:00.082)       0:10:38.646 ******

TASK [etcd : Gen_certs | Set cert names per node] *********************************************************************
ok: [node1]
Monday 05 September 2022  14:06:18 +0000 (0:00:00.037)       0:10:38.684 ******
Monday 05 September 2022  14:06:18 +0000 (0:00:00.055)       0:10:38.739 ******
Monday 05 September 2022  14:06:18 +0000 (0:00:00.030)       0:10:38.770 ******
Monday 05 September 2022  14:06:18 +0000 (0:00:00.026)       0:10:38.796 ******

TASK [etcd : Gen_certs | check certificate permissions] ***************************************************************
changed: [node1]
Monday 05 September 2022  14:06:18 +0000 (0:00:00.189)       0:10:38.986 ******
included: /kubespray/roles/etcd/tasks/upd_ca_trust.yml for node1
Monday 05 September 2022  14:06:18 +0000 (0:00:00.036)       0:10:39.022 ******

TASK [etcd : Gen_certs | target ca-certificate store file] ************************************************************
ok: [node1]
Monday 05 September 2022  14:06:18 +0000 (0:00:00.032)       0:10:39.055 ******

TASK [etcd : Gen_certs | add CA to trusted CA dir] ********************************************************************
changed: [node1]
Monday 05 September 2022  14:06:18 +0000 (0:00:00.262)       0:10:39.317 ******
Monday 05 September 2022  14:06:18 +0000 (0:00:00.023)       0:10:39.341 ******

TASK [etcd : Gen_certs | update ca-certificates (RedHat)] *************************************************************
changed: [node1]
Monday 05 September 2022  14:06:19 +0000 (0:00:00.390)       0:10:39.731 ******
Monday 05 September 2022  14:06:19 +0000 (0:00:00.026)       0:10:39.758 ******

TASK [etcd : Gen_certs | Get etcd certificate serials] ****************************************************************
ok: [node1]
Monday 05 September 2022  14:06:19 +0000 (0:00:00.186)       0:10:39.945 ******

TASK [etcd : Set etcd_client_cert_serial] *****************************************************************************
ok: [node1]
Monday 05 September 2022  14:06:19 +0000 (0:00:00.034)       0:10:39.980 ******
included: /kubespray/roles/etcd/tasks/install_host.yml for node1
Monday 05 September 2022  14:06:19 +0000 (0:00:00.038)       0:10:40.018 ******

TASK [etcd : install | Copy etcd and etcdctl binary from download dir] ************************************************
changed: [node1] => (item=etcd)
changed: [node1] => (item=etcdctl)
Monday 05 September 2022  14:06:20 +0000 (0:00:00.617)       0:10:40.636 ******
included: /kubespray/roles/etcd/tasks/configure.yml for node1
Monday 05 September 2022  14:06:20 +0000 (0:00:00.050)       0:10:40.687 ******

TASK [etcd : Configure | Check if etcd cluster is healthy] ************************************************************
ok: [node1]
Monday 05 September 2022  14:06:25 +0000 (0:00:05.204)       0:10:45.891 ******
Monday 05 September 2022  14:06:25 +0000 (0:00:00.024)       0:10:45.916 ******
included: /kubespray/roles/etcd/tasks/refresh_config.yml for node1
Monday 05 September 2022  14:06:25 +0000 (0:00:00.034)       0:10:45.950 ******

TASK [etcd : Refresh config | Create etcd config file] ****************************************************************
changed: [node1]
Monday 05 September 2022  14:06:26 +0000 (0:00:00.489)       0:10:46.439 ******
Monday 05 September 2022  14:06:26 +0000 (0:00:00.023)       0:10:46.463 ******

TASK [etcd : Configure | Copy etcd.service systemd file] **************************************************************
changed: [node1]
Monday 05 September 2022  14:06:26 +0000 (0:00:00.475)       0:10:46.938 ******
Monday 05 September 2022  14:06:26 +0000 (0:00:00.026)       0:10:46.964 ******

TASK [etcd : Configure | reload systemd] ******************************************************************************
ok: [node1]
Monday 05 September 2022  14:06:26 +0000 (0:00:00.352)       0:10:47.317 ******

TASK [etcd : Configure | Ensure etcd is running] **********************************************************************
changed: [node1]
Monday 05 September 2022  14:06:30 +0000 (0:00:03.436)       0:10:50.753 ******
Monday 05 September 2022  14:06:30 +0000 (0:00:00.030)       0:10:50.784 ******

TASK [etcd : Configure | Wait for etcd cluster to be healthy] *********************************************************
ok: [node1]
Monday 05 September 2022  14:06:30 +0000 (0:00:00.256)       0:10:51.040 ******
Monday 05 September 2022  14:06:30 +0000 (0:00:00.027)       0:10:51.067 ******

TASK [etcd : Configure | Check if member is in etcd cluster] **********************************************************
ok: [node1]
Monday 05 September 2022  14:06:30 +0000 (0:00:00.222)       0:10:51.290 ******
Monday 05 September 2022  14:06:30 +0000 (0:00:00.039)       0:10:51.330 ******
Monday 05 September 2022  14:06:30 +0000 (0:00:00.034)       0:10:51.364 ******
Monday 05 September 2022  14:06:31 +0000 (0:00:00.029)       0:10:51.394 ******
included: /kubespray/roles/etcd/tasks/refresh_config.yml for node1
Monday 05 September 2022  14:06:31 +0000 (0:00:00.037)       0:10:51.432 ******

TASK [etcd : Refresh config | Create etcd config file] ****************************************************************
changed: [node1]
Monday 05 September 2022  14:06:31 +0000 (0:00:00.499)       0:10:51.932 ******
Monday 05 September 2022  14:06:31 +0000 (0:00:00.024)       0:10:51.956 ******
Monday 05 September 2022  14:06:31 +0000 (0:00:00.024)       0:10:51.980 ******
Monday 05 September 2022  14:06:31 +0000 (0:00:00.025)       0:10:52.006 ******
included: /kubespray/roles/etcd/tasks/refresh_config.yml for node1
Monday 05 September 2022  14:06:31 +0000 (0:00:00.041)       0:10:52.047 ******

TASK [etcd : Refresh config | Create etcd config file] ****************************************************************
ok: [node1]
Monday 05 September 2022  14:06:32 +0000 (0:00:00.450)       0:10:52.498 ******
Monday 05 September 2022  14:06:32 +0000 (0:00:00.023)       0:10:52.522 ******

RUNNING HANDLER [etcd : restart etcd] *********************************************************************************
changed: [node1]
Monday 05 September 2022  14:06:32 +0000 (0:00:00.175)       0:10:52.697 ******

RUNNING HANDLER [etcd : Backup etcd data] *****************************************************************************
changed: [node1]
Monday 05 September 2022  14:06:32 +0000 (0:00:00.192)       0:10:52.890 ******

RUNNING HANDLER [etcd : Refresh Time Fact] ****************************************************************************
ok: [node1]
Monday 05 September 2022  14:06:32 +0000 (0:00:00.440)       0:10:53.330 ******

RUNNING HANDLER [etcd : Set Backup Directory] *************************************************************************
ok: [node1]
Monday 05 September 2022  14:06:32 +0000 (0:00:00.032)       0:10:53.362 ******

RUNNING HANDLER [etcd : Create Backup Directory] **********************************************************************
changed: [node1]
Monday 05 September 2022  14:06:33 +0000 (0:00:00.181)       0:10:53.544 ******

RUNNING HANDLER [etcd : Stat etcd v2 data directory] ******************************************************************
ok: [node1]
Monday 05 September 2022  14:06:33 +0000 (0:00:00.176)       0:10:53.721 ******

RUNNING HANDLER [etcd : Backup etcd v2 data] **************************************************************************
changed: [node1]
Monday 05 September 2022  14:06:33 +0000 (0:00:00.200)       0:10:53.922 ******

RUNNING HANDLER [etcd : Backup etcd v3 data] **************************************************************************
changed: [node1]
Monday 05 September 2022  14:06:33 +0000 (0:00:00.210)       0:10:54.133 ******
Monday 05 September 2022  14:06:33 +0000 (0:00:00.025)       0:10:54.158 ******

RUNNING HANDLER [etcd : etcd | reload systemd] ************************************************************************
ok: [node1]
Monday 05 September 2022  14:06:34 +0000 (0:00:00.336)       0:10:54.495 ******

RUNNING HANDLER [etcd : reload etcd] **********************************************************************************
changed: [node1]
Monday 05 September 2022  14:06:36 +0000 (0:00:02.563)       0:10:57.058 ******

RUNNING HANDLER [etcd : wait for etcd up] *****************************************************************************
ok: [node1]
Monday 05 September 2022  14:06:37 +0000 (0:00:00.555)       0:10:57.613 ******

RUNNING HANDLER [etcd : set etcd_secret_changed] **********************************************************************
ok: [node1]

PLAY [k8s_cluster] ****************************************************************************************************
Monday 05 September 2022  14:06:37 +0000 (0:00:00.056)       0:10:57.669 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.045)       0:10:57.715 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.050)       0:10:57.766 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.058)       0:10:57.824 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.051)       0:10:57.876 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.047)       0:10:57.923 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.049)       0:10:57.973 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.054)       0:10:58.027 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.025)       0:10:58.052 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.023)       0:10:58.076 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.045)       0:10:58.121 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.056)       0:10:58.178 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.048)       0:10:58.226 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.049)       0:10:58.276 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.022)       0:10:58.299 ******
Monday 05 September 2022  14:06:37 +0000 (0:00:00.048)       0:10:58.348 ******
Monday 05 September 2022  14:06:38 +0000 (0:00:00.050)       0:10:58.399 ******
Monday 05 September 2022  14:06:38 +0000 (0:00:00.048)       0:10:58.447 ******
Monday 05 September 2022  14:06:38 +0000 (0:00:00.047)       0:10:58.495 ******
Monday 05 September 2022  14:06:39 +0000 (0:00:01.667)       0:11:00.163 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node2] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node3] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  14:06:39 +0000 (0:00:00.066)       0:11:00.229 ******
Monday 05 September 2022  14:06:39 +0000 (0:00:00.059)       0:11:00.288 ******
Monday 05 September 2022  14:06:39 +0000 (0:00:00.026)       0:11:00.314 ******
Monday 05 September 2022  14:06:39 +0000 (0:00:00.056)       0:11:00.371 ******
Monday 05 September 2022  14:06:40 +0000 (0:00:00.026)       0:11:00.397 ******
Monday 05 September 2022  14:06:40 +0000 (0:00:00.058)       0:11:00.456 ******

TASK [adduser : User | Create User Group] *****************************************************************************
ok: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:06:40 +0000 (0:00:00.516)       0:11:00.972 ******

TASK [adduser : User | Create User] ***********************************************************************************
ok: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:06:41 +0000 (0:00:00.545)       0:11:01.518 ******

TASK [adduser : User | Create User Group] *****************************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:06:41 +0000 (0:00:00.279)       0:11:01.797 ******

TASK [adduser : User | Create User] ***********************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:41 +0000 (0:00:00.339)       0:11:02.137 ******
included: /kubespray/roles/etcd/tasks/check_certs.yml for node1, node3, node2
Monday 05 September 2022  14:06:41 +0000 (0:00:00.102)       0:11:02.239 ******

TASK [etcd : Check_certs | Register certs that have already been generated on first etcd node] ************************
ok: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:06:42 +0000 (0:00:00.202)       0:11:02.442 ******

TASK [etcd : Check_certs | Set default value for 'sync_certs', 'gen_certs' and 'etcd_secret_changed' to false] ********
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:06:42 +0000 (0:00:00.095)       0:11:02.537 ******

TASK [etcd : Check certs | Register ca and etcd admin/member certs on etcd hosts] *************************************
ok: [node1] => (item=ca.pem)
ok: [node1] => (item=member-node1.pem)
ok: [node1] => (item=member-node1-key.pem)
ok: [node1] => (item=admin-node1.pem)
ok: [node1] => (item=admin-node1-key.pem)
Monday 05 September 2022  14:06:43 +0000 (0:00:00.857)       0:11:03.395 ******

TASK [etcd : Check certs | Register ca and etcd node certs on kubernetes hosts] ***************************************
ok: [node1] => (item=ca.pem)
ok: [node3] => (item=ca.pem)
ok: [node2] => (item=ca.pem)
ok: [node1] => (item=node-node1.pem)
ok: [node2] => (item=node-node2.pem)
ok: [node3] => (item=node-node3.pem)
ok: [node1] => (item=node-node1-key.pem)
ok: [node2] => (item=node-node2-key.pem)
ok: [node3] => (item=node-node3-key.pem)
Monday 05 September 2022  14:06:43 +0000 (0:00:00.658)       0:11:04.053 ******

TASK [etcd : Check_certs | Set 'gen_certs' to true if expected certificates are not on the first etcd node] ***********
ok: [node1] => (item=/etc/ssl/etcd/ssl/ca.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/admin-node1.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/admin-node1-key.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/member-node1.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/member-node1-key.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node1.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node1-key.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node2.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node2-key.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node3.pem)
ok: [node1] => (item=/etc/ssl/etcd/ssl/node-node3-key.pem)
Monday 05 September 2022  14:06:43 +0000 (0:00:00.332)       0:11:04.386 ******

TASK [etcd : Check_certs | Set 'gen_master_certs' object to track whether member and admin certs exist on first etcd node] ***
ok: [node1]
Monday 05 September 2022  14:06:44 +0000 (0:00:00.043)       0:11:04.429 ******

TASK [etcd : Check_certs | Set 'gen_node_certs' object to track whether node certs exist on first etcd node] **********
ok: [node1]
Monday 05 September 2022  14:06:44 +0000 (0:00:00.037)       0:11:04.466 ******
Monday 05 September 2022  14:06:44 +0000 (0:00:00.063)       0:11:04.529 ******

TASK [etcd : Check_certs | Set 'kubernetes_host_requires_sync' to true if ca or node cert and key don't exist on kubernetes host or checksum doesn't match] ***
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:06:44 +0000 (0:00:00.079)       0:11:04.609 ******

TASK [etcd : Check_certs | Set 'sync_certs' to true] ******************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:44 +0000 (0:00:00.064)       0:11:04.673 ******
included: /kubespray/roles/etcd/tasks/gen_certs_script.yml for node1, node2, node3
Monday 05 September 2022  14:06:44 +0000 (0:00:00.076)       0:11:04.749 ******

TASK [etcd : Gen_certs | create etcd cert dir] ************************************************************************
ok: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:06:44 +0000 (0:00:00.340)       0:11:05.089 ******

TASK [etcd : Gen_certs | create etcd script dir (on node1)] ***********************************************************
ok: [node1]
Monday 05 September 2022  14:06:44 +0000 (0:00:00.201)       0:11:05.291 ******

TASK [etcd : Gen_certs | write openssl config] ************************************************************************
ok: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:06:45 +0000 (0:00:00.443)       0:11:05.734 ******

TASK [etcd : Gen_certs | copy certs generation script] ****************************************************************
ok: [node1]
Monday 05 September 2022  14:06:45 +0000 (0:00:00.415)       0:11:06.150 ******

TASK [etcd : Gen_certs | run cert generation script] ******************************************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:06:46 +0000 (0:00:00.888)       0:11:07.038 ******
Monday 05 September 2022  14:06:46 +0000 (0:00:00.209)       0:11:07.247 ******
Monday 05 September 2022  14:06:47 +0000 (0:00:00.159)       0:11:07.407 ******
Monday 05 September 2022  14:06:47 +0000 (0:00:00.197)       0:11:07.604 ******
Monday 05 September 2022  14:06:47 +0000 (0:00:00.141)       0:11:07.746 ******

TASK [etcd : Gen_certs | Set cert names per node] *********************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:47 +0000 (0:00:00.110)       0:11:07.856 ******

TASK [etcd : Check_certs | Set 'sync_certs' to true on nodes] *********************************************************
ok: [node2] => (item=ca.pem)
ok: [node2] => (item=node-node2.pem)
ok: [node3] => (item=ca.pem)
ok: [node2] => (item=node-node2-key.pem)
ok: [node3] => (item=node-node3.pem)
ok: [node3] => (item=node-node3-key.pem)
Monday 05 September 2022  14:06:47 +0000 (0:00:00.138)       0:11:07.995 ******

TASK [etcd : Gen_certs | Gather node certs] ***************************************************************************
changed: [node2 -> 192.168.88.211]
changed: [node3 -> 192.168.88.211]
Monday 05 September 2022  14:06:47 +0000 (0:00:00.299)       0:11:08.294 ******

TASK [etcd : Gen_certs | Copy certs on nodes] *************************************************************************
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:48 +0000 (0:00:00.229)       0:11:08.524 ******

TASK [etcd : Gen_certs | check certificate permissions] ***************************************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:06:48 +0000 (0:00:00.256)       0:11:08.780 ******
included: /kubespray/roles/etcd/tasks/upd_ca_trust.yml for node1, node2, node3
Monday 05 September 2022  14:06:48 +0000 (0:00:00.078)       0:11:08.859 ******

TASK [etcd : Gen_certs | target ca-certificate store file] ************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:06:48 +0000 (0:00:00.060)       0:11:08.919 ******

TASK [etcd : Gen_certs | add CA to trusted CA dir] ********************************************************************
ok: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:06:48 +0000 (0:00:00.352)       0:11:09.272 ******
Monday 05 September 2022  14:06:48 +0000 (0:00:00.056)       0:11:09.328 ******

TASK [etcd : Gen_certs | update ca-certificates (RedHat)] *************************************************************
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:06:49 +0000 (0:00:00.442)       0:11:09.771 ******
Monday 05 September 2022  14:06:49 +0000 (0:00:00.053)       0:11:09.824 ******

TASK [etcd : Gen_certs | Get etcd certificate serials] ****************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:49 +0000 (0:00:00.253)       0:11:10.078 ******

TASK [etcd : Set etcd_client_cert_serial] *****************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:49 +0000 (0:00:00.066)       0:11:10.144 ******
included: /kubespray/roles/etcd/tasks/install_host.yml for node1
Monday 05 September 2022  14:06:49 +0000 (0:00:00.079)       0:11:10.223 ******
Monday 05 September 2022  14:06:49 +0000 (0:00:00.033)       0:11:10.256 ******
included: /kubespray/roles/etcd/tasks/configure.yml for node1
Monday 05 September 2022  14:06:49 +0000 (0:00:00.086)       0:11:10.343 ******
Monday 05 September 2022  14:06:49 +0000 (0:00:00.031)       0:11:10.374 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.033)       0:11:10.407 ******
included: /kubespray/roles/etcd/tasks/refresh_config.yml for node1
Monday 05 September 2022  14:06:50 +0000 (0:00:00.032)       0:11:10.440 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.028)       0:11:10.469 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.026)       0:11:10.496 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.024)       0:11:10.521 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.025)       0:11:10.547 ******

TASK [etcd : Configure | reload systemd] ******************************************************************************
ok: [node1]
Monday 05 September 2022  14:06:50 +0000 (0:00:00.358)       0:11:10.905 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.025)       0:11:10.930 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.026)       0:11:10.956 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.027)       0:11:10.984 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.027)       0:11:11.011 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.022)       0:11:11.034 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.024)       0:11:11.059 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.033)       0:11:11.093 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.030)       0:11:11.123 ******
included: /kubespray/roles/etcd/tasks/refresh_config.yml for node1
Monday 05 September 2022  14:06:50 +0000 (0:00:00.083)       0:11:11.207 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.022)       0:11:11.229 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.023)       0:11:11.253 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.053)       0:11:11.306 ******
Monday 05 September 2022  14:06:50 +0000 (0:00:00.059)       0:11:11.366 ******
included: /kubespray/roles/etcd/tasks/refresh_config.yml for node1
Monday 05 September 2022  14:06:51 +0000 (0:00:00.078)       0:11:11.444 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.023)       0:11:11.468 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.026)       0:11:11.495 ******

RUNNING HANDLER [etcd : set etcd_secret_changed] **********************************************************************
ok: [node1]

PLAY [k8s_cluster] ****************************************************************************************************
Monday 05 September 2022  14:06:51 +0000 (0:00:00.062)       0:11:11.557 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.046)       0:11:11.604 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.048)       0:11:11.653 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.051)       0:11:11.705 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.050)       0:11:11.756 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.056)       0:11:11.812 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.056)       0:11:11.869 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.050)       0:11:11.920 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.024)       0:11:11.944 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.026)       0:11:11.971 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.044)       0:11:12.015 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.048)       0:11:12.064 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.047)       0:11:12.112 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.055)       0:11:12.167 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.022)       0:11:12.190 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.048)       0:11:12.239 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.048)       0:11:12.288 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.048)       0:11:12.336 ******
Monday 05 September 2022  14:06:51 +0000 (0:00:00.052)       0:11:12.388 ******
Monday 05 September 2022  14:06:53 +0000 (0:00:01.637)       0:11:14.026 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node2] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node3] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  14:06:53 +0000 (0:00:00.060)       0:11:14.086 ******
Monday 05 September 2022  14:06:53 +0000 (0:00:00.053)       0:11:14.139 ******
Monday 05 September 2022  14:06:53 +0000 (0:00:00.022)       0:11:14.162 ******
Monday 05 September 2022  14:06:53 +0000 (0:00:00.055)       0:11:14.217 ******
Monday 05 September 2022  14:06:53 +0000 (0:00:00.023)       0:11:14.241 ******
Monday 05 September 2022  14:06:53 +0000 (0:00:00.049)       0:11:14.290 ******
Monday 05 September 2022  14:06:53 +0000 (0:00:00.049)       0:11:14.340 ******
Monday 05 September 2022  14:06:54 +0000 (0:00:00.051)       0:11:14.391 ******
Monday 05 September 2022  14:06:54 +0000 (0:00:00.047)       0:11:14.438 ******
Monday 05 September 2022  14:06:54 +0000 (0:00:00.050)       0:11:14.489 ******

TASK [kubernetes/node : set kubelet_cgroup_driver_detected fact for containerd] ***************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:54 +0000 (0:00:00.057)       0:11:14.547 ******

TASK [kubernetes/node : set kubelet_cgroup_driver] ********************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:54 +0000 (0:00:00.058)       0:11:14.605 ******
Monday 05 September 2022  14:06:54 +0000 (0:00:00.049)       0:11:14.655 ******
Monday 05 September 2022  14:06:54 +0000 (0:00:00.051)       0:11:14.707 ******
Monday 05 September 2022  14:06:54 +0000 (0:00:00.057)       0:11:14.765 ******

TASK [kubernetes/node : Pre-upgrade | check if kubelet container exists] **********************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:06:54 +0000 (0:00:00.270)       0:11:15.035 ******
Monday 05 September 2022  14:06:54 +0000 (0:00:00.050)       0:11:15.086 ******
Monday 05 September 2022  14:06:54 +0000 (0:00:00.048)       0:11:15.135 ******
Monday 05 September 2022  14:06:54 +0000 (0:00:00.058)       0:11:15.193 ******

TASK [kubernetes/node : Ensure /var/lib/cni exists] *******************************************************************
changed: [node2]
changed: [node1]
changed: [node3]
Monday 05 September 2022  14:06:55 +0000 (0:00:00.273)       0:11:15.466 ******

TASK [kubernetes/node : install | Copy kubeadm binary from download dir] **********************************************
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:06:55 +0000 (0:00:00.456)       0:11:15.922 ******

TASK [kubernetes/node : install | Copy kubelet binary from download dir] **********************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:06:56 +0000 (0:00:00.713)       0:11:16.635 ******
Monday 05 September 2022  14:06:56 +0000 (0:00:00.050)       0:11:16.686 ******
Monday 05 September 2022  14:06:56 +0000 (0:00:00.050)       0:11:16.737 ******
Monday 05 September 2022  14:06:56 +0000 (0:00:00.054)       0:11:16.791 ******

TASK [kubernetes/node : haproxy | Cleanup potentially deployed haproxy] ***********************************************
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:56 +0000 (0:00:00.241)       0:11:17.033 ******

TASK [kubernetes/node : nginx-proxy | Make nginx directory] ***********************************************************
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:06:56 +0000 (0:00:00.243)       0:11:17.276 ******

TASK [kubernetes/node : nginx-proxy | Write nginx-proxy configuration] ************************************************
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:06:57 +0000 (0:00:00.576)       0:11:17.853 ******

TASK [kubernetes/node : nginx-proxy | Get checksum from config] *******************************************************
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:57 +0000 (0:00:00.286)       0:11:18.140 ******

TASK [kubernetes/node : nginx-proxy | Write static pod] ***************************************************************
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:06:58 +0000 (0:00:00.544)       0:11:18.684 ******
Monday 05 September 2022  14:06:58 +0000 (0:00:00.059)       0:11:18.744 ******
Monday 05 September 2022  14:06:58 +0000 (0:00:00.062)       0:11:18.806 ******
Monday 05 September 2022  14:06:58 +0000 (0:00:00.056)       0:11:18.862 ******
Monday 05 September 2022  14:06:58 +0000 (0:00:00.060)       0:11:18.923 ******
Monday 05 September 2022  14:06:58 +0000 (0:00:00.056)       0:11:18.979 ******

TASK [kubernetes/node : Ensure nodePort range is reserved] ************************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:06:58 +0000 (0:00:00.255)       0:11:19.235 ******

TASK [kubernetes/node : Verify if br_netfilter module exists] *********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:59 +0000 (0:00:00.250)       0:11:19.486 ******

TASK [kubernetes/node : Verify br_netfilter module path exists] *******************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:06:59 +0000 (0:00:00.253)       0:11:19.739 ******

TASK [kubernetes/node : Enable br_netfilter module] *******************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:06:59 +0000 (0:00:00.258)       0:11:19.998 ******

TASK [kubernetes/node : Persist br_netfilter module] ******************************************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:07:00 +0000 (0:00:00.673)       0:11:20.671 ******

TASK [kubernetes/node : Check if bridge-nf-call-iptables key exists] **************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:07:00 +0000 (0:00:00.252)       0:11:20.924 ******

TASK [kubernetes/node : Enable bridge-nf-call tables] *****************************************************************
changed: [node2] => (item=net.bridge.bridge-nf-call-iptables)
changed: [node1] => (item=net.bridge.bridge-nf-call-iptables)
changed: [node3] => (item=net.bridge.bridge-nf-call-iptables)
changed: [node1] => (item=net.bridge.bridge-nf-call-arptables)
changed: [node2] => (item=net.bridge.bridge-nf-call-arptables)
changed: [node3] => (item=net.bridge.bridge-nf-call-arptables)
changed: [node1] => (item=net.bridge.bridge-nf-call-ip6tables)
changed: [node2] => (item=net.bridge.bridge-nf-call-ip6tables)
changed: [node3] => (item=net.bridge.bridge-nf-call-ip6tables)
Monday 05 September 2022  14:07:01 +0000 (0:00:00.716)       0:11:21.641 ******

TASK [kubernetes/node : Modprobe Kernel Module for IPVS] **************************************************************
changed: [node1] => (item=ip_vs)
changed: [node3] => (item=ip_vs)
changed: [node2] => (item=ip_vs)
changed: [node1] => (item=ip_vs_rr)
changed: [node2] => (item=ip_vs_rr)
changed: [node3] => (item=ip_vs_rr)
changed: [node1] => (item=ip_vs_wrr)
changed: [node2] => (item=ip_vs_wrr)
changed: [node3] => (item=ip_vs_wrr)
changed: [node1] => (item=ip_vs_sh)
changed: [node3] => (item=ip_vs_sh)
changed: [node2] => (item=ip_vs_sh)
Monday 05 September 2022  14:07:03 +0000 (0:00:02.219)       0:11:23.861 ******

TASK [kubernetes/node : Modprobe nf_conntrack_ipv4] *******************************************************************
fatal: [node1]: FAILED! => {"changed": false, "msg": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64\n", "name": "nf_conntrack_ipv4", "params": "", "rc": 1, "state": "present", "stderr": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64\n", "stderr_lines": ["modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64"], "stdout": "", "stdout_lines": []}
...ignoring
fatal: [node2]: FAILED! => {"changed": false, "msg": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64\n", "name": "nf_conntrack_ipv4", "params": "", "rc": 1, "state": "present", "stderr": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64\n", "stderr_lines": ["modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64"], "stdout": "", "stdout_lines": []}
...ignoring
fatal: [node3]: FAILED! => {"changed": false, "msg": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64\n", "name": "nf_conntrack_ipv4", "params": "", "rc": 1, "state": "present", "stderr": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64\n", "stderr_lines": ["modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64"], "stdout": "", "stdout_lines": []}
...ignoring
Monday 05 September 2022  14:07:03 +0000 (0:00:00.246)       0:11:24.107 ******

TASK [kubernetes/node : Persist ip_vs modules] ************************************************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:07:04 +0000 (0:00:00.623)       0:11:24.731 ******
Monday 05 September 2022  14:07:04 +0000 (0:00:00.047)       0:11:24.778 ******
Monday 05 September 2022  14:07:04 +0000 (0:00:00.047)       0:11:24.826 ******
Monday 05 September 2022  14:07:04 +0000 (0:00:00.046)       0:11:24.873 ******
Monday 05 September 2022  14:07:04 +0000 (0:00:00.054)       0:11:24.927 ******
Monday 05 September 2022  14:07:04 +0000 (0:00:00.068)       0:11:24.995 ******

TASK [kubernetes/node : Set kubelet api version to v1beta1] ***********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:07:04 +0000 (0:00:00.065)       0:11:25.061 ******

TASK [kubernetes/node : Write kubelet environment config file (kubeadm)] **********************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:07:05 +0000 (0:00:00.635)       0:11:25.696 ******

TASK [kubernetes/node : Write kubelet config file] ********************************************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:07:05 +0000 (0:00:00.661)       0:11:26.358 ******

TASK [kubernetes/node : Write kubelet systemd init file] **************************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:07:06 +0000 (0:00:00.624)       0:11:26.983 ******

RUNNING HANDLER [kubernetes/node : Node | restart kubelet] ************************************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:07:06 +0000 (0:00:00.252)       0:11:27.236 ******

RUNNING HANDLER [kubernetes/node : Kubelet | reload systemd] **********************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:07:07 +0000 (0:00:00.459)       0:11:27.695 ******

RUNNING HANDLER [kubernetes/node : Kubelet | restart kubelet] *********************************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:07:07 +0000 (0:00:00.388)       0:11:28.084 ******

TASK [kubernetes/node : Enable kubelet] *******************************************************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:07:08 +0000 (0:00:00.521)       0:11:28.605 ******

RUNNING HANDLER [kubernetes/node : Kubelet | restart kubelet] *********************************************************
changed: [node1]
changed: [node2]
changed: [node3]

PLAY [kube_control_plane] *********************************************************************************************
Monday 05 September 2022  14:07:08 +0000 (0:00:00.434)       0:11:29.039 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.025)       0:11:29.065 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.029)       0:11:29.095 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.033)       0:11:29.128 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.032)       0:11:29.161 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.026)       0:11:29.187 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.030)       0:11:29.218 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.036)       0:11:29.254 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.037)       0:11:29.292 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.027)       0:11:29.319 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.029)       0:11:29.349 ******
Monday 05 September 2022  14:07:08 +0000 (0:00:00.030)       0:11:29.380 ******
Monday 05 September 2022  14:07:09 +0000 (0:00:00.031)       0:11:29.412 ******
Monday 05 September 2022  14:07:09 +0000 (0:00:00.033)       0:11:29.445 ******
Monday 05 September 2022  14:07:09 +0000 (0:00:00.030)       0:11:29.475 ******
Monday 05 September 2022  14:07:09 +0000 (0:00:00.028)       0:11:29.503 ******
Monday 05 September 2022  14:07:09 +0000 (0:00:00.020)       0:11:29.524 ******
Monday 05 September 2022  14:07:09 +0000 (0:00:00.022)       0:11:29.546 ******
Monday 05 September 2022  14:07:09 +0000 (0:00:00.021)       0:11:29.568 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:01.010)       0:11:30.579 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  14:07:10 +0000 (0:00:00.030)       0:11:30.610 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.057)       0:11:30.668 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.025)       0:11:30.693 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.026)       0:11:30.719 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.025)       0:11:30.745 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.031)       0:11:30.776 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.023)       0:11:30.800 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.023)       0:11:30.824 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.028)       0:11:30.853 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.022)       0:11:30.875 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.021)       0:11:30.897 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.027)       0:11:30.924 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.031)       0:11:30.955 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.045)       0:11:31.001 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.049)       0:11:31.050 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.027)       0:11:31.078 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.027)       0:11:31.105 ******
Monday 05 September 2022  14:07:10 +0000 (0:00:00.022)       0:11:31.127 ******

TASK [kubernetes/control-plane : Pre-upgrade | Delete master manifests if etcd secrets changed] ***********************
ok: [node1] => (item=kube-apiserver)
ok: [node1] => (item=kube-controller-manager)
ok: [node1] => (item=kube-scheduler)
Monday 05 September 2022  14:07:11 +0000 (0:00:00.505)       0:11:31.633 ******
Monday 05 September 2022  14:07:11 +0000 (0:00:00.041)       0:11:31.675 ******

TASK [kubernetes/control-plane : Check which kube-control nodes are already members of the cluster] *******************
fatal: [node1]: FAILED! => {"changed": false, "cmd": "/usr/local/bin/kubectl get nodes --selector=node-role.kubernetes.io/control-plane -o json", "msg": "[Errno 2] No such file or directory: b'/usr/local/bin/kubectl': b'/usr/local/bin/kubectl'", "rc": 2}
...ignoring
Monday 05 September 2022  14:07:11 +0000 (0:00:00.180)       0:11:31.855 ******
Monday 05 September 2022  14:07:11 +0000 (0:00:00.028)       0:11:31.884 ******

TASK [kubernetes/control-plane : Set fact first_kube_control_plane] ***************************************************
ok: [node1]
Monday 05 September 2022  14:07:11 +0000 (0:00:00.031)       0:11:31.915 ******
Monday 05 September 2022  14:07:11 +0000 (0:00:00.021)       0:11:31.937 ******
Monday 05 September 2022  14:07:11 +0000 (0:00:00.021)       0:11:31.959 ******

TASK [kubernetes/control-plane : Create kube-scheduler config] ********************************************************
changed: [node1]
Monday 05 September 2022  14:07:12 +0000 (0:00:00.484)       0:11:32.444 ******
Monday 05 September 2022  14:07:12 +0000 (0:00:00.030)       0:11:32.474 ******
Monday 05 September 2022  14:07:12 +0000 (0:00:00.027)       0:11:32.502 ******
Monday 05 September 2022  14:07:12 +0000 (0:00:00.027)       0:11:32.529 ******
Monday 05 September 2022  14:07:12 +0000 (0:00:00.034)       0:11:32.564 ******
Monday 05 September 2022  14:07:12 +0000 (0:00:00.025)       0:11:32.590 ******
Monday 05 September 2022  14:07:12 +0000 (0:00:00.021)       0:11:32.612 ******

TASK [kubernetes/control-plane : Install | Copy kubectl binary from download dir] *************************************
changed: [node1]
Monday 05 September 2022  14:07:12 +0000 (0:00:00.389)       0:11:33.001 ******

TASK [kubernetes/control-plane : Install kubectl bash completion] *****************************************************
changed: [node1]
Monday 05 September 2022  14:07:12 +0000 (0:00:00.232)       0:11:33.234 ******

TASK [kubernetes/control-plane : Set kubectl bash completion file permissions] ****************************************
changed: [node1]
Monday 05 September 2022  14:07:13 +0000 (0:00:00.192)       0:11:33.426 ******
Monday 05 September 2022  14:07:13 +0000 (0:00:00.021)       0:11:33.447 ******
Monday 05 September 2022  14:07:13 +0000 (0:00:00.024)       0:11:33.472 ******

TASK [kubernetes/control-plane : kubeadm | Check if kubeadm has already run] ******************************************
ok: [node1]
Monday 05 September 2022  14:07:13 +0000 (0:00:00.175)       0:11:33.648 ******
Monday 05 September 2022  14:07:13 +0000 (0:00:00.058)       0:11:33.706 ******
Monday 05 September 2022  14:07:13 +0000 (0:00:00.045)       0:11:33.752 ******

TASK [kubernetes/control-plane : kubeadm | aggregate all SANs] ********************************************************
ok: [node1]
Monday 05 September 2022  14:07:13 +0000 (0:00:00.086)       0:11:33.838 ******
Monday 05 September 2022  14:07:13 +0000 (0:00:00.023)       0:11:33.862 ******
Monday 05 September 2022  14:07:13 +0000 (0:00:00.023)       0:11:33.885 ******
Monday 05 September 2022  14:07:13 +0000 (0:00:00.021)       0:11:33.906 ******
Monday 05 September 2022  14:07:13 +0000 (0:00:00.021)       0:11:33.928 ******

TASK [kubernetes/control-plane : Set kubeadm api version to v1beta2] **************************************************
ok: [node1]
Monday 05 September 2022  14:07:13 +0000 (0:00:00.029)       0:11:33.957 ******

TASK [kubernetes/control-plane : kubeadm | Create kubeadm config] *****************************************************
changed: [node1]
Monday 05 September 2022  14:07:14 +0000 (0:00:00.596)       0:11:34.553 ******
Monday 05 September 2022  14:07:14 +0000 (0:00:00.023)       0:11:34.576 ******
Monday 05 September 2022  14:07:14 +0000 (0:00:00.037)       0:11:34.614 ******
Monday 05 September 2022  14:07:14 +0000 (0:00:00.023)       0:11:34.637 ******

TASK [kubernetes/control-plane : kubeadm | Initialize first master] ***************************************************
changed: [node1]
Monday 05 September 2022  14:07:35 +0000 (0:00:21.066)       0:11:55.704 ******
Monday 05 September 2022  14:07:36 +0000 (0:00:00.774)       0:11:56.478 ******
Monday 05 September 2022  14:07:36 +0000 (0:00:00.031)       0:11:56.510 ******

TASK [kubernetes/control-plane : Create kubeadm token for joining nodes with 24h expiration (default)] ****************
ok: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:07:36 +0000 (0:00:00.225)       0:11:56.735 ******

TASK [kubernetes/control-plane : Set kubeadm_token] *******************************************************************
ok: [node1]
Monday 05 September 2022  14:07:36 +0000 (0:00:00.036)       0:11:56.772 ******
included: /kubespray/roles/kubernetes/control-plane/tasks/kubeadm-secondary.yml for node1
Monday 05 September 2022  14:07:36 +0000 (0:00:00.037)       0:11:56.810 ******

TASK [kubernetes/control-plane : Set kubeadm_discovery_address] *******************************************************
ok: [node1]
Monday 05 September 2022  14:07:36 +0000 (0:00:00.040)       0:11:56.850 ******

TASK [kubernetes/control-plane : Upload certificates so they are fresh and not expired] *******************************
changed: [node1]
Monday 05 September 2022  14:07:36 +0000 (0:00:00.262)       0:11:57.112 ******

TASK [kubernetes/control-plane : Parse certificate key if not set] ****************************************************
ok: [node1]
Monday 05 September 2022  14:07:36 +0000 (0:00:00.051)       0:11:57.164 ******
Monday 05 September 2022  14:07:36 +0000 (0:00:00.027)       0:11:57.191 ******

TASK [kubernetes/control-plane : Wait for k8s apiserver] **************************************************************
ok: [node1]
Monday 05 September 2022  14:07:37 +0000 (0:00:00.388)       0:11:57.579 ******

TASK [kubernetes/control-plane : check already run] *******************************************************************
ok: [node1] => {
    "msg": false
}
Monday 05 September 2022  14:07:37 +0000 (0:00:00.035)       0:11:57.615 ******
Monday 05 September 2022  14:07:37 +0000 (0:00:00.023)       0:11:57.639 ******
Monday 05 September 2022  14:07:37 +0000 (0:00:00.028)       0:11:57.668 ******

TASK [kubernetes/control-plane : kubeadm | Remove taint for master with node role] ************************************
changed: [node1 -> 192.168.88.211] => (item=node-role.kubernetes.io/master:NoSchedule-)
changed: [node1 -> 192.168.88.211] => (item=node-role.kubernetes.io/control-plane:NoSchedule-)
Monday 05 September 2022  14:07:37 +0000 (0:00:00.570)       0:11:58.238 ******
Monday 05 September 2022  14:07:37 +0000 (0:00:00.023)       0:11:58.262 ******
included: /kubespray/roles/kubernetes/control-plane/tasks/kubeadm-fix-apiserver.yml for node1
Monday 05 September 2022  14:07:37 +0000 (0:00:00.036)       0:11:58.299 ******

TASK [kubernetes/control-plane : Update server field in component kubeconfigs] ****************************************
changed: [node1] => (item=admin.conf)
changed: [node1] => (item=controller-manager.conf)
changed: [node1] => (item=kubelet.conf)
changed: [node1] => (item=scheduler.conf)
Monday 05 September 2022  14:07:38 +0000 (0:00:00.764)       0:11:59.063 ******

TASK [kubernetes/control-plane : Update etcd-servers for apiserver] ***************************************************
ok: [node1]
Monday 05 September 2022  14:07:38 +0000 (0:00:00.228)       0:11:59.292 ******
included: /kubespray/roles/kubernetes/control-plane/tasks/kubelet-fix-client-cert-rotation.yml for node1
Monday 05 September 2022  14:07:38 +0000 (0:00:00.034)       0:11:59.327 ******

TASK [kubernetes/control-plane : Fixup kubelet client cert rotation 1/2] **********************************************
ok: [node1]
Monday 05 September 2022  14:07:39 +0000 (0:00:00.225)       0:11:59.553 ******

TASK [kubernetes/control-plane : Fixup kubelet client cert rotation 2/2] **********************************************
ok: [node1]
Monday 05 September 2022  14:07:39 +0000 (0:00:00.211)       0:11:59.764 ******

TASK [kubernetes/control-plane : Install script to renew K8S control plane certificates] ******************************
changed: [node1]
Monday 05 September 2022  14:07:39 +0000 (0:00:00.556)       0:12:00.321 ******
Monday 05 September 2022  14:07:39 +0000 (0:00:00.029)       0:12:00.351 ******
Monday 05 September 2022  14:07:39 +0000 (0:00:00.021)       0:12:00.373 ******

TASK [kubernetes/client : Set external kube-apiserver endpoint] *******************************************************
ok: [node1]
Monday 05 September 2022  14:07:40 +0000 (0:00:00.034)       0:12:00.407 ******

TASK [kubernetes/client : Create kube config dir for current/ansible become user] *************************************
changed: [node1]
Monday 05 September 2022  14:07:40 +0000 (0:00:00.187)       0:12:00.595 ******

TASK [kubernetes/client : Copy admin kubeconfig to current/ansible become user home] **********************************
changed: [node1]
Monday 05 September 2022  14:07:40 +0000 (0:00:00.437)       0:12:01.033 ******
Monday 05 September 2022  14:07:40 +0000 (0:00:00.025)       0:12:01.058 ******

TASK [kubernetes/client : Wait for k8s apiserver] *********************************************************************
ok: [node1]
Monday 05 September 2022  14:07:40 +0000 (0:00:00.207)       0:12:01.266 ******
Monday 05 September 2022  14:07:40 +0000 (0:00:00.022)       0:12:01.289 ******
Monday 05 September 2022  14:07:40 +0000 (0:00:00.024)       0:12:01.314 ******
Monday 05 September 2022  14:07:40 +0000 (0:00:00.024)       0:12:01.338 ******
Monday 05 September 2022  14:07:40 +0000 (0:00:00.023)       0:12:01.361 ******
Monday 05 September 2022  14:07:40 +0000 (0:00:00.025)       0:12:01.386 ******
Monday 05 September 2022  14:07:41 +0000 (0:00:00.023)       0:12:01.410 ******
Monday 05 September 2022  14:07:41 +0000 (0:00:00.023)       0:12:01.433 ******

TASK [kubernetes-apps/cluster_roles : Kubernetes Apps | Wait for kube-apiserver] **************************************
ok: [node1]
Monday 05 September 2022  14:07:41 +0000 (0:00:00.361)       0:12:01.795 ******
Monday 05 September 2022  14:07:41 +0000 (0:00:00.022)       0:12:01.817 ******
Monday 05 September 2022  14:07:41 +0000 (0:00:00.021)       0:12:01.838 ******
Monday 05 September 2022  14:07:41 +0000 (0:00:00.040)       0:12:01.879 ******
Monday 05 September 2022  14:07:41 +0000 (0:00:00.049)       0:12:01.928 ******

TASK [kubernetes-apps/cluster_roles : Kubernetes Apps | Add ClusterRoleBinding to admit nodes] ************************
changed: [node1]
Monday 05 September 2022  14:07:42 +0000 (0:00:00.493)       0:12:02.422 ******

TASK [kubernetes-apps/cluster_roles : Apply workaround to allow all nodes with cert O=system:nodes to register] *******
ok: [node1]
Monday 05 September 2022  14:07:43 +0000 (0:00:01.139)       0:12:03.561 ******

TASK [kubernetes-apps/cluster_roles : Kubernetes Apps | Add webhook ClusterRole that grants access to proxy, stats, log, spec, and metrics on a kubelet] ***
changed: [node1]
Monday 05 September 2022  14:07:43 +0000 (0:00:00.475)       0:12:04.037 ******

TASK [kubernetes-apps/cluster_roles : Apply webhook ClusterRole] ******************************************************
ok: [node1]
Monday 05 September 2022  14:07:43 +0000 (0:00:00.298)       0:12:04.336 ******

TASK [kubernetes-apps/cluster_roles : Kubernetes Apps | Add ClusterRoleBinding for system:nodes to webhook ClusterRole] ***
changed: [node1]
Monday 05 September 2022  14:07:44 +0000 (0:00:00.479)       0:12:04.816 ******

TASK [kubernetes-apps/cluster_roles : Grant system:nodes the webhook ClusterRole] *************************************
ok: [node1]
Monday 05 September 2022  14:07:44 +0000 (0:00:00.304)       0:12:05.121 ******
Monday 05 September 2022  14:07:44 +0000 (0:00:00.022)       0:12:05.143 ******

TASK [kubernetes-apps/cluster_roles : PriorityClass | Copy k8s-cluster-critical-pc.yml file] **************************
changed: [node1]
Monday 05 September 2022  14:07:45 +0000 (0:00:00.488)       0:12:05.632 ******

TASK [kubernetes-apps/cluster_roles : PriorityClass | Create k8s-cluster-critical] ************************************
ok: [node1]
Monday 05 September 2022  14:07:45 +0000 (0:00:00.287)       0:12:05.919 ******

RUNNING HANDLER [kubernetes/control-plane : Master | restart kubelet] *************************************************
changed: [node1]
Monday 05 September 2022  14:07:45 +0000 (0:00:00.181)       0:12:06.101 ******

RUNNING HANDLER [kubernetes/control-plane : Master | wait for master static pods] *************************************
changed: [node1]
Monday 05 September 2022  14:07:45 +0000 (0:00:00.188)       0:12:06.289 ******

RUNNING HANDLER [kubernetes/control-plane : Master | Restart kube-scheduler] ******************************************
changed: [node1]
Monday 05 September 2022  14:07:46 +0000 (0:00:00.174)       0:12:06.464 ******

RUNNING HANDLER [kubernetes/control-plane : Master | Restart kube-controller-manager] *********************************
changed: [node1]
Monday 05 September 2022  14:07:46 +0000 (0:00:00.183)       0:12:06.647 ******

RUNNING HANDLER [kubernetes/control-plane : Master | reload systemd] **************************************************
ok: [node1]
Monday 05 September 2022  14:07:46 +0000 (0:00:00.378)       0:12:07.026 ******

RUNNING HANDLER [kubernetes/control-plane : Master | reload kubelet] **************************************************
changed: [node1]
Monday 05 September 2022  14:07:47 +0000 (0:00:00.386)       0:12:07.413 ******
Monday 05 September 2022  14:07:47 +0000 (0:00:00.031)       0:12:07.445 ******

RUNNING HANDLER [kubernetes/control-plane : Master | Remove scheduler container containerd/crio] **********************
changed: [node1]
Monday 05 September 2022  14:07:47 +0000 (0:00:00.355)       0:12:07.800 ******
Monday 05 September 2022  14:07:47 +0000 (0:00:00.044)       0:12:07.845 ******

RUNNING HANDLER [kubernetes/control-plane : Master | Remove controller manager container containerd/crio] *************
changed: [node1]
Monday 05 September 2022  14:07:47 +0000 (0:00:00.316)       0:12:08.161 ******
FAILED - RETRYING: Master | wait for kube-scheduler (60 retries left).
FAILED - RETRYING: Master | wait for kube-scheduler (59 retries left).
FAILED - RETRYING: Master | wait for kube-scheduler (58 retries left).
FAILED - RETRYING: Master | wait for kube-scheduler (57 retries left).

RUNNING HANDLER [kubernetes/control-plane : Master | wait for kube-scheduler] *****************************************
ok: [node1]
Monday 05 September 2022  14:07:54 +0000 (0:00:06.447)       0:12:14.608 ******

RUNNING HANDLER [kubernetes/control-plane : Master | wait for kube-controller-manager] ********************************
ok: [node1]
Monday 05 September 2022  14:07:54 +0000 (0:00:00.340)       0:12:14.949 ******

RUNNING HANDLER [kubernetes/control-plane : Master | wait for the apiserver to be running] ****************************
ok: [node1]

PLAY [k8s_cluster] ****************************************************************************************************
Monday 05 September 2022  14:07:54 +0000 (0:00:00.402)       0:12:15.351 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.052)       0:12:15.404 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.049)       0:12:15.453 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.051)       0:12:15.505 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.050)       0:12:15.556 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.055)       0:12:15.611 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.050)       0:12:15.661 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.053)       0:12:15.715 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.025)       0:12:15.740 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.026)       0:12:15.767 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.053)       0:12:15.820 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.048)       0:12:15.869 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.054)       0:12:15.923 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.053)       0:12:15.977 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.024)       0:12:16.001 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.052)       0:12:16.054 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.053)       0:12:16.107 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.053)       0:12:16.161 ******
Monday 05 September 2022  14:07:55 +0000 (0:00:00.051)       0:12:16.213 ******
Monday 05 September 2022  14:07:57 +0000 (0:00:02.092)       0:12:18.306 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node2] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node3] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  14:07:57 +0000 (0:00:00.061)       0:12:18.368 ******
Monday 05 September 2022  14:07:58 +0000 (0:00:00.056)       0:12:18.424 ******
Monday 05 September 2022  14:07:58 +0000 (0:00:00.026)       0:12:18.451 ******
Monday 05 September 2022  14:07:58 +0000 (0:00:00.058)       0:12:18.509 ******
Monday 05 September 2022  14:07:58 +0000 (0:00:00.023)       0:12:18.533 ******
Monday 05 September 2022  14:07:58 +0000 (0:00:00.053)       0:12:18.586 ******

TASK [kubernetes/kubeadm : Set kubeadm_discovery_address] *************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:07:58 +0000 (0:00:00.089)       0:12:18.675 ******

TASK [kubernetes/kubeadm : Check if kubelet.conf exists] **************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:07:58 +0000 (0:00:00.255)       0:12:18.931 ******

TASK [kubernetes/kubeadm : Check if kubeadm CA cert is accessible] ****************************************************
ok: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:07:58 +0000 (0:00:00.197)       0:12:19.128 ******

TASK [kubernetes/kubeadm : Calculate kubeadm CA cert hash] ************************************************************
ok: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:07:58 +0000 (0:00:00.193)       0:12:19.322 ******

TASK [kubernetes/kubeadm : Create kubeadm token for joining nodes with 24h expiration (default)] **********************
ok: [node2 -> 192.168.88.211]
ok: [node3 -> 192.168.88.211]
Monday 05 September 2022  14:07:59 +0000 (0:00:00.496)       0:12:19.818 ******

TASK [kubernetes/kubeadm : Set kubeadm_token to generated token] ******************************************************
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:07:59 +0000 (0:00:00.065)       0:12:19.884 ******

TASK [kubernetes/kubeadm : Set kubeadm api version to v1beta2] ********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:07:59 +0000 (0:00:00.061)       0:12:19.945 ******

TASK [kubernetes/kubeadm : Create kubeadm client config] **************************************************************
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:08:00 +0000 (0:00:00.599)       0:12:20.545 ******

TASK [kubernetes/kubeadm : Join to cluster] ***************************************************************************
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:08:30 +0000 (0:00:30.072)       0:12:50.617 ******
Monday 05 September 2022  14:08:30 +0000 (0:00:00.062)       0:12:50.679 ******

TASK [kubernetes/kubeadm : Update server field in kubelet kubeconfig] *************************************************
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:08:30 +0000 (0:00:00.309)       0:12:50.989 ******

TASK [kubernetes/kubeadm : Update server field in kube-proxy kubeconfig] **********************************************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:08:31 +0000 (0:00:00.434)       0:12:51.424 ******

TASK [kubernetes/kubeadm : Set ca.crt file permission] ****************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:08:31 +0000 (0:00:00.284)       0:12:51.709 ******

TASK [kubernetes/kubeadm : Restart all kube-proxy pods to ensure that they load the new configmap] ********************
changed: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:08:31 +0000 (0:00:00.436)       0:12:52.145 ******
Monday 05 September 2022  14:08:31 +0000 (0:00:00.063)       0:12:52.208 ******

TASK [kubernetes/node-label : Kubernetes Apps | Wait for kube-apiserver] **********************************************
ok: [node1]
Monday 05 September 2022  14:08:32 +0000 (0:00:00.427)       0:12:52.636 ******

TASK [kubernetes/node-label : Set role node label to empty list] ******************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:08:32 +0000 (0:00:00.075)       0:12:52.711 ******
Monday 05 September 2022  14:08:32 +0000 (0:00:00.076)       0:12:52.788 ******

TASK [kubernetes/node-label : Set inventory node label to empty list] *************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:08:32 +0000 (0:00:00.103)       0:12:52.892 ******
Monday 05 September 2022  14:08:32 +0000 (0:00:00.046)       0:12:52.938 ******

TASK [kubernetes/node-label : debug] **********************************************************************************
ok: [node1] => {
    "role_node_labels": []
}
ok: [node3] => {
    "role_node_labels": []
}
ok: [node2] => {
    "role_node_labels": []
}
Monday 05 September 2022  14:08:32 +0000 (0:00:00.067)       0:12:53.006 ******

TASK [kubernetes/node-label : debug] **********************************************************************************
ok: [node1] => {
    "inventory_node_labels": []
}
ok: [node2] => {
    "inventory_node_labels": []
}
ok: [node3] => {
    "inventory_node_labels": []
}
Monday 05 September 2022  14:08:32 +0000 (0:00:00.089)       0:12:53.095 ******
Monday 05 September 2022  14:08:32 +0000 (0:00:00.050)       0:12:53.146 ******
Monday 05 September 2022  14:08:32 +0000 (0:00:00.051)       0:12:53.198 ******
Monday 05 September 2022  14:08:32 +0000 (0:00:00.061)       0:12:53.260 ******
Monday 05 September 2022  14:08:32 +0000 (0:00:00.057)       0:12:53.317 ******
Monday 05 September 2022  14:08:33 +0000 (0:00:00.079)       0:12:53.397 ******
Monday 05 September 2022  14:08:33 +0000 (0:00:00.073)       0:12:53.470 ******

TASK [network_plugin/calico : Check vars defined correctly] ***********************************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  14:08:33 +0000 (0:00:00.085)       0:12:53.555 ******
Monday 05 September 2022  14:08:33 +0000 (0:00:00.073)       0:12:53.629 ******

TASK [network_plugin/calico : Check ipip and vxlan mode defined correctly] ********************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  14:08:33 +0000 (0:00:00.090)       0:12:53.719 ******

TASK [network_plugin/calico : Check ipip and vxlan mode if simultaneously enabled] ************************************
ok: [node1] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node2] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [node3] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  14:08:33 +0000 (0:00:00.096)       0:12:53.816 ******
Monday 05 September 2022  14:08:33 +0000 (0:00:00.069)       0:12:53.886 ******

TASK [network_plugin/calico : Get Calico default-pool configuration] **************************************************
ok: [node1 -> 192.168.88.211]
Monday 05 September 2022  14:08:33 +0000 (0:00:00.196)       0:12:54.082 ******
Monday 05 September 2022  14:08:33 +0000 (0:00:00.061)       0:12:54.144 ******
Monday 05 September 2022  14:08:33 +0000 (0:00:00.057)       0:12:54.201 ******

TASK [network_plugin/calico : Slurp CNI config] ***********************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:08:34 +0000 (0:00:00.253)       0:12:54.455 ******
Monday 05 September 2022  14:08:34 +0000 (0:00:00.059)       0:12:54.514 ******
Monday 05 September 2022  14:08:34 +0000 (0:00:00.068)       0:12:54.583 ******
Monday 05 September 2022  14:08:34 +0000 (0:00:00.078)       0:12:54.661 ******

TASK [network_plugin/calico : Calico | Gather os specific variables] **************************************************
ok: [node1] => (item=/kubespray/roles/network_plugin/calico/vars/../vars/redhat.yml)
ok: [node3] => (item=/kubespray/roles/network_plugin/calico/vars/../vars/redhat.yml)
ok: [node2] => (item=/kubespray/roles/network_plugin/calico/vars/../vars/redhat.yml)
Monday 05 September 2022  14:08:34 +0000 (0:00:00.092)       0:12:54.753 ******
Monday 05 September 2022  14:08:34 +0000 (0:00:00.062)       0:12:54.816 ******
included: /kubespray/roles/network_plugin/calico/tasks/install.yml for node1, node2, node3
Monday 05 September 2022  14:08:34 +0000 (0:00:00.092)       0:12:54.909 ******
Monday 05 September 2022  14:08:34 +0000 (0:00:00.087)       0:12:54.996 ******

TASK [network_plugin/calico : Calico | Copy calicoctl binary from download dir] ***************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:08:35 +0000 (0:00:00.529)       0:12:55.525 ******

TASK [network_plugin/calico : Calico | Write Calico cni config] *******************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:08:35 +0000 (0:00:00.704)       0:12:56.230 ******
Monday 05 September 2022  14:08:35 +0000 (0:00:00.061)       0:12:56.291 ******
Monday 05 September 2022  14:08:36 +0000 (0:00:00.119)       0:12:56.411 ******
Monday 05 September 2022  14:08:36 +0000 (0:00:00.056)       0:12:56.468 ******

TASK [network_plugin/calico : Calico | Install calicoctl wrapper script] **********************************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:08:36 +0000 (0:00:00.638)       0:12:57.106 ******
Monday 05 September 2022  14:08:36 +0000 (0:00:00.026)       0:12:57.133 ******

TASK [network_plugin/calico : Calico | Check if calico network pool has already been configured] **********************
ok: [node1]
Monday 05 September 2022  14:08:37 +0000 (0:00:00.271)       0:12:57.405 ******
Monday 05 September 2022  14:08:37 +0000 (0:00:00.057)       0:12:57.462 ******
Monday 05 September 2022  14:08:37 +0000 (0:00:00.059)       0:12:57.522 ******
Monday 05 September 2022  14:08:37 +0000 (0:00:00.065)       0:12:57.587 ******

TASK [network_plugin/calico : Calico | Create calico manifests for kdd] ***********************************************
changed: [node1]
Monday 05 September 2022  14:08:37 +0000 (0:00:00.466)       0:12:58.054 ******

TASK [network_plugin/calico : Calico | Create Calico Kubernetes datastore resources] **********************************
ok: [node1]
Monday 05 September 2022  14:08:38 +0000 (0:00:00.850)       0:12:58.904 ******

TASK [network_plugin/calico : Calico | Configure calico FelixConfiguration] *******************************************
changed: [node1]
Monday 05 September 2022  14:08:40 +0000 (0:00:02.269)       0:13:01.174 ******

TASK [network_plugin/calico : Calico | Configure calico network pool] *************************************************
changed: [node1]
Monday 05 September 2022  14:08:41 +0000 (0:00:00.308)       0:13:01.482 ******
Monday 05 September 2022  14:08:41 +0000 (0:00:00.068)       0:13:01.551 ******
Monday 05 September 2022  14:08:41 +0000 (0:00:00.017)       0:13:01.569 ******
Monday 05 September 2022  14:08:41 +0000 (0:00:00.020)       0:13:01.589 ******
Monday 05 September 2022  14:08:41 +0000 (0:00:00.028)       0:13:01.618 ******

TASK [network_plugin/calico : Calico | Set up BGP Configuration] ******************************************************
ok: [node1]
Monday 05 September 2022  14:08:41 +0000 (0:00:00.280)       0:13:01.899 ******
Monday 05 September 2022  14:08:41 +0000 (0:00:00.067)       0:13:01.966 ******
Monday 05 September 2022  14:08:41 +0000 (0:00:00.058)       0:13:02.025 ******
Monday 05 September 2022  14:08:41 +0000 (0:00:00.047)       0:13:02.073 ******

TASK [network_plugin/calico : Calico | Create calico manifests] *******************************************************
changed: [node1] => (item={'name': 'calico-config', 'file': 'calico-config.yml', 'type': 'cm'})
changed: [node1] => (item={'name': 'calico-node', 'file': 'calico-node.yml', 'type': 'ds'})
changed: [node1] => (item={'name': 'calico', 'file': 'calico-node-sa.yml', 'type': 'sa'})
changed: [node1] => (item={'name': 'calico', 'file': 'calico-cr.yml', 'type': 'clusterrole'})
changed: [node1] => (item={'name': 'calico', 'file': 'calico-crb.yml', 'type': 'clusterrolebinding'})
changed: [node1] => (item={'name': 'kubernetes-services-endpoint', 'file': 'kubernetes-services-endpoint.yml', 'type': 'cm'})
Monday 05 September 2022  14:08:44 +0000 (0:00:03.073)       0:13:05.146 ******
Monday 05 September 2022  14:08:44 +0000 (0:00:00.069)       0:13:05.216 ******

TASK [network_plugin/calico : Start Calico resources] *****************************************************************
ok: [node1] => (item=calico-config.yml)
ok: [node1] => (item=calico-node.yml)
ok: [node1] => (item=calico-node-sa.yml)
ok: [node1] => (item=calico-cr.yml)
ok: [node1] => (item=calico-crb.yml)
ok: [node1] => (item=kubernetes-services-endpoint.yml)
Monday 05 September 2022  14:08:47 +0000 (0:00:02.721)       0:13:07.937 ******

TASK [network_plugin/calico : Wait for calico kubeconfig to be created] ***********************************************
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:08:48 +0000 (0:00:01.293)       0:13:09.231 ******
Monday 05 September 2022  14:08:48 +0000 (0:00:00.057)       0:13:09.289 ******
Monday 05 September 2022  14:08:48 +0000 (0:00:00.060)       0:13:09.349 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.060)       0:13:09.409 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.067)       0:13:09.476 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.071)       0:13:09.547 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.064)       0:13:09.612 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.088)       0:13:09.700 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.057)       0:13:09.758 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.054)       0:13:09.812 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.065)       0:13:09.878 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.063)       0:13:09.941 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.052)       0:13:09.994 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.053)       0:13:10.047 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.122)       0:13:10.170 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.025)       0:13:10.195 ******
Monday 05 September 2022  14:08:49 +0000 (0:00:00.142)       0:13:10.338 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.076)       0:13:10.415 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.097)       0:13:10.512 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.082)       0:13:10.595 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.061)       0:13:10.656 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.069)       0:13:10.726 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.063)       0:13:10.789 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.075)       0:13:10.865 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.057)       0:13:10.923 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.063)       0:13:10.987 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.060)       0:13:11.048 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.065)       0:13:11.113 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.133)       0:13:11.247 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.052)       0:13:11.299 ******
Monday 05 September 2022  14:08:50 +0000 (0:00:00.079)       0:13:11.379 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.065)       0:13:11.444 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.087)       0:13:11.531 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.093)       0:13:11.625 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.055)       0:13:11.681 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.053)       0:13:11.734 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.059)       0:13:11.793 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.053)       0:13:11.846 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.084)       0:13:11.931 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.058)       0:13:11.990 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.055)       0:13:12.045 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.053)       0:13:12.099 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.050)       0:13:12.149 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.057)       0:13:12.207 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.055)       0:13:12.263 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.060)       0:13:12.323 ******
Monday 05 September 2022  14:08:51 +0000 (0:00:00.057)       0:13:12.381 ******
Monday 05 September 2022  14:08:52 +0000 (0:00:00.055)       0:13:12.436 ******
Monday 05 September 2022  14:08:52 +0000 (0:00:00.071)       0:13:12.508 ******
Monday 05 September 2022  14:08:52 +0000 (0:00:00.064)       0:13:12.573 ******
Monday 05 September 2022  14:08:52 +0000 (0:00:00.067)       0:13:12.640 ******
Monday 05 September 2022  14:08:52 +0000 (0:00:00.028)       0:13:12.669 ******
Monday 05 September 2022  14:08:52 +0000 (0:00:00.064)       0:13:12.733 ******
Monday 05 September 2022  14:08:52 +0000 (0:00:00.057)       0:13:12.791 ******
Monday 05 September 2022  14:08:52 +0000 (0:00:00.120)       0:13:12.911 ******
Monday 05 September 2022  14:08:52 +0000 (0:00:00.064)       0:13:12.976 ******

RUNNING HANDLER [kubernetes/kubeadm : Kubeadm | restart kubelet] ******************************************************
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:08:52 +0000 (0:00:00.282)       0:13:13.258 ******

RUNNING HANDLER [kubernetes/kubeadm : Kubeadm | reload systemd] *******************************************************
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:08:53 +0000 (0:00:00.561)       0:13:13.819 ******

RUNNING HANDLER [kubernetes/kubeadm : Kubeadm | reload kubelet] *******************************************************
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:08:53 +0000 (0:00:00.490)       0:13:14.310 ******

PLAY [calico_rr] ******************************************************************************************************
skipping: no hosts matched

PLAY [kube_control_plane[0]] ******************************************************************************************
Monday 05 September 2022  14:08:54 +0000 (0:00:00.095)       0:13:14.406 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.024)       0:13:14.430 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.021)       0:13:14.451 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.021)       0:13:14.473 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.032)       0:13:14.506 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.025)       0:13:14.532 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.021)       0:13:14.553 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.026)       0:13:14.580 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.029)       0:13:14.610 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.027)       0:13:14.637 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.023)       0:13:14.660 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.032)       0:13:14.692 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.029)       0:13:14.722 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.028)       0:13:14.750 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.027)       0:13:14.777 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.025)       0:13:14.803 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.027)       0:13:14.830 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.026)       0:13:14.857 ******
Monday 05 September 2022  14:08:54 +0000 (0:00:00.026)       0:13:14.883 ******
Monday 05 September 2022  14:08:55 +0000 (0:00:01.078)       0:13:15.961 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  14:08:55 +0000 (0:00:00.031)       0:13:15.992 ******
Monday 05 September 2022  14:08:55 +0000 (0:00:00.058)       0:13:16.051 ******
Monday 05 September 2022  14:08:55 +0000 (0:00:00.024)       0:13:16.075 ******
Monday 05 September 2022  14:08:55 +0000 (0:00:00.028)       0:13:16.104 ******
Monday 05 September 2022  14:08:55 +0000 (0:00:00.026)       0:13:16.130 ******
Monday 05 September 2022  14:08:55 +0000 (0:00:00.025)       0:13:16.156 ******

TASK [win_nodes/kubernetes_patch : Ensure that user manifests directory exists] ***************************************
changed: [node1]
Monday 05 September 2022  14:08:55 +0000 (0:00:00.226)       0:13:16.382 ******

TASK [win_nodes/kubernetes_patch : Check current nodeselector for kube-proxy daemonset] *******************************
ok: [node1]
Monday 05 September 2022  14:08:56 +0000 (0:00:00.241)       0:13:16.623 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.031)       0:13:16.655 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.029)       0:13:16.685 ******

PLAY [kube_control_plane] *********************************************************************************************
Monday 05 September 2022  14:08:56 +0000 (0:00:00.109)       0:13:16.794 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.028)       0:13:16.822 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.026)       0:13:16.849 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.025)       0:13:16.874 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.033)       0:13:16.907 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.026)       0:13:16.933 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.024)       0:13:16.958 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.028)       0:13:16.987 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.032)       0:13:17.019 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.027)       0:13:17.046 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.024)       0:13:17.071 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.025)       0:13:17.096 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.025)       0:13:17.122 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.029)       0:13:17.152 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.028)       0:13:17.181 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.031)       0:13:17.212 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.027)       0:13:17.239 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.025)       0:13:17.265 ******
Monday 05 September 2022  14:08:56 +0000 (0:00:00.029)       0:13:17.295 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:01.133)       0:13:18.428 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  14:08:58 +0000 (0:00:00.031)       0:13:18.460 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.058)       0:13:18.518 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.026)       0:13:18.545 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.028)       0:13:18.574 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.026)       0:13:18.601 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.033)       0:13:18.635 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.024)       0:13:18.659 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.028)       0:13:18.687 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.027)       0:13:18.715 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.053)       0:13:18.769 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.052)       0:13:18.821 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.023)       0:13:18.845 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.026)       0:13:18.871 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.050)       0:13:18.922 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.024)       0:13:18.946 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.027)       0:13:18.973 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.050)       0:13:19.024 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.075)       0:13:19.100 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.038)       0:13:19.138 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.022)       0:13:19.161 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.043)       0:13:19.204 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.028)       0:13:19.233 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.030)       0:13:19.264 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.031)       0:13:19.296 ******
Monday 05 September 2022  14:08:58 +0000 (0:00:00.024)       0:13:19.320 ******
Monday 05 September 2022  14:08:59 +0000 (0:00:00.085)       0:13:19.405 ******
Monday 05 September 2022  14:08:59 +0000 (0:00:00.045)       0:13:19.450 ******

TASK [policy_controller/calico : Create calico-kube-controllers manifests] ********************************************
changed: [node1] => (item={'name': 'calico-kube-controllers', 'file': 'calico-kube-controllers.yml', 'type': 'deployment'})
changed: [node1] => (item={'name': 'calico-kube-controllers', 'file': 'calico-kube-sa.yml', 'type': 'sa'})
changed: [node1] => (item={'name': 'calico-kube-controllers', 'file': 'calico-kube-cr.yml', 'type': 'clusterrole'})
changed: [node1] => (item={'name': 'calico-kube-controllers', 'file': 'calico-kube-crb.yml', 'type': 'clusterrolebinding'})
Monday 05 September 2022  14:09:01 +0000 (0:00:02.235)       0:13:21.686 ******

TASK [policy_controller/calico : Start of Calico kube controllers] ****************************************************
ok: [node1] => (item=calico-kube-controllers.yml)
ok: [node1] => (item=calico-kube-sa.yml)
ok: [node1] => (item=calico-kube-cr.yml)
ok: [node1] => (item=calico-kube-crb.yml)
Monday 05 September 2022  14:09:02 +0000 (0:00:01.568)       0:13:23.255 ******
Monday 05 September 2022  14:09:02 +0000 (0:00:00.024)       0:13:23.280 ******
Monday 05 September 2022  14:09:02 +0000 (0:00:00.054)       0:13:23.334 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.054)       0:13:23.389 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.025)       0:13:23.415 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.025)       0:13:23.440 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.026)       0:13:23.467 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.027)       0:13:23.494 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.028)       0:13:23.522 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.031)       0:13:23.554 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.030)       0:13:23.585 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.033)       0:13:23.618 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.035)       0:13:23.654 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.036)       0:13:23.690 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.032)       0:13:23.723 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.034)       0:13:23.757 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.075)       0:13:23.833 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.078)       0:13:23.912 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.056)       0:13:23.968 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.028)       0:13:23.996 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.027)       0:13:24.024 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.144)       0:13:24.169 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.026)       0:13:24.195 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.025)       0:13:24.221 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.025)       0:13:24.247 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.024)       0:13:24.272 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.024)       0:13:24.296 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.024)       0:13:24.320 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.025)       0:13:24.346 ******
Monday 05 September 2022  14:09:03 +0000 (0:00:00.026)       0:13:24.373 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.029)       0:13:24.403 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.031)       0:13:24.434 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.032)       0:13:24.466 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.028)       0:13:24.495 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.026)       0:13:24.521 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.025)       0:13:24.547 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.024)       0:13:24.572 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.026)       0:13:24.598 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.025)       0:13:24.623 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.028)       0:13:24.652 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.023)       0:13:24.675 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.026)       0:13:24.702 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.024)       0:13:24.726 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.027)       0:13:24.754 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.026)       0:13:24.780 ******
Monday 05 September 2022  14:09:04 +0000 (0:00:00.041)       0:13:24.821 ******

TASK [kubernetes-apps/ansible : Kubernetes Apps | Wait for kube-apiserver] ********************************************
ok: [node1]
Monday 05 September 2022  14:09:04 +0000 (0:00:00.361)       0:13:25.182 ******

TASK [kubernetes-apps/ansible : Kubernetes Apps | Register coredns deployment annotation `createdby`] *****************
fatal: [node1]: FAILED! => {"changed": false, "cmd": "/usr/local/bin/kubectl get deploy -n kube-system coredns -o jsonpath='{ .spec.template.metadata.annotations.createdby }'", "delta": "0:00:00.051239", "end": "2022-09-05 14:09:05.009959", "msg": "non-zero return code", "rc": 1, "start": "2022-09-05 14:09:04.958720", "stderr": "Error from server (NotFound): deployments.apps \"coredns\" not found", "stderr_lines": ["Error from server (NotFound): deployments.apps \"coredns\" not found"], "stdout": "", "stdout_lines": []}
...ignoring
Monday 05 September 2022  14:09:05 +0000 (0:00:00.234)       0:13:25.416 ******

TASK [kubernetes-apps/ansible : Kubernetes Apps | Delete kubeadm CoreDNS] *********************************************
ok: [node1]
Monday 05 September 2022  14:09:05 +0000 (0:00:00.228)       0:13:25.645 ******

TASK [kubernetes-apps/ansible : Kubernetes Apps | Delete kubeadm Kube-DNS service] ************************************
ok: [node1]
Monday 05 September 2022  14:09:05 +0000 (0:00:00.229)       0:13:25.875 ******

TASK [kubernetes-apps/ansible : Kubernetes Apps | Lay Down CoreDNS templates] *****************************************
changed: [node1] => (item={'name': 'coredns', 'file': 'coredns-clusterrole.yml', 'type': 'clusterrole'})
changed: [node1] => (item={'name': 'coredns', 'file': 'coredns-clusterrolebinding.yml', 'type': 'clusterrolebinding'})
changed: [node1] => (item={'name': 'coredns', 'file': 'coredns-config.yml', 'type': 'configmap'})
changed: [node1] => (item={'name': 'coredns', 'file': 'coredns-deployment.yml', 'type': 'deployment'})
changed: [node1] => (item={'name': 'coredns', 'file': 'coredns-sa.yml', 'type': 'sa'})
changed: [node1] => (item={'name': 'coredns', 'file': 'coredns-svc.yml', 'type': 'svc'})
changed: [node1] => (item={'name': 'dns-autoscaler', 'file': 'dns-autoscaler.yml', 'type': 'deployment'})
changed: [node1] => (item={'name': 'dns-autoscaler', 'file': 'dns-autoscaler-clusterrole.yml', 'type': 'clusterrole'})
changed: [node1] => (item={'name': 'dns-autoscaler', 'file': 'dns-autoscaler-clusterrolebinding.yml', 'type': 'clusterrolebinding'})
changed: [node1] => (item={'name': 'dns-autoscaler', 'file': 'dns-autoscaler-sa.yml', 'type': 'sa'})
Monday 05 September 2022  14:09:10 +0000 (0:00:05.135)       0:13:31.010 ******
Monday 05 September 2022  14:09:10 +0000 (0:00:00.072)       0:13:31.083 ******

TASK [kubernetes-apps/ansible : Kubernetes Apps | set up necessary nodelocaldns parameters] ***************************
ok: [node1]
Monday 05 September 2022  14:09:10 +0000 (0:00:00.070)       0:13:31.153 ******

TASK [kubernetes-apps/ansible : Kubernetes Apps | Lay Down nodelocaldns Template] *************************************
changed: [node1] => (item={'name': 'nodelocaldns', 'file': 'nodelocaldns-config.yml', 'type': 'configmap'})
changed: [node1] => (item={'name': 'nodelocaldns', 'file': 'nodelocaldns-sa.yml', 'type': 'sa'})
changed: [node1] => (item={'name': 'nodelocaldns', 'file': 'nodelocaldns-daemonset.yml', 'type': 'daemonset'})
Monday 05 September 2022  14:09:12 +0000 (0:00:01.564)       0:13:32.718 ******
Monday 05 September 2022  14:09:12 +0000 (0:00:00.040)       0:13:32.758 ******

TASK [kubernetes-apps/ansible : Kubernetes Apps | Start Resources] ****************************************************
ok: [node1] => (item=coredns-clusterrole.yml)
ok: [node1] => (item=coredns-clusterrolebinding.yml)
ok: [node1] => (item=coredns-config.yml)
ok: [node1] => (item=coredns-deployment.yml)
ok: [node1] => (item=coredns-sa.yml)
ok: [node1] => (item=coredns-svc.yml)
ok: [node1] => (item=dns-autoscaler.yml)
ok: [node1] => (item=dns-autoscaler-clusterrole.yml)
ok: [node1] => (item=dns-autoscaler-clusterrolebinding.yml)
ok: [node1] => (item=dns-autoscaler-sa.yml)
ok: [node1] => (item=nodelocaldns-config.yml)
ok: [node1] => (item=nodelocaldns-sa.yml)
ok: [node1] => (item=nodelocaldns-daemonset.yml)
Monday 05 September 2022  14:09:17 +0000 (0:00:05.027)       0:13:37.786 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.073)       0:13:37.859 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.063)       0:13:37.923 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.030)       0:13:37.953 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.032)       0:13:37.985 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.044)       0:13:38.030 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.041)       0:13:38.071 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.046)       0:13:38.118 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.042)       0:13:38.160 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.043)       0:13:38.203 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.043)       0:13:38.247 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.033)       0:13:38.281 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.041)       0:13:38.323 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.033)       0:13:38.356 ******
Monday 05 September 2022  14:09:17 +0000 (0:00:00.030)       0:13:38.386 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.029)       0:13:38.416 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.027)       0:13:38.444 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.026)       0:13:38.471 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.031)       0:13:38.503 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.033)       0:13:38.536 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.033)       0:13:38.569 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.033)       0:13:38.603 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.036)       0:13:38.639 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.034)       0:13:38.673 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.033)       0:13:38.706 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.026)       0:13:38.733 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.026)       0:13:38.760 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.026)       0:13:38.786 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.026)       0:13:38.813 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.031)       0:13:38.845 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.024)       0:13:38.869 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.026)       0:13:38.896 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.035)       0:13:38.931 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.026)       0:13:38.958 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.027)       0:13:38.986 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.032)       0:13:39.018 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.032)       0:13:39.051 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.023)       0:13:39.074 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.024)       0:13:39.098 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.025)       0:13:39.123 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.024)       0:13:39.148 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.025)       0:13:39.174 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.027)       0:13:39.201 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.026)       0:13:39.228 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.043)       0:13:39.271 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.051)       0:13:39.323 ******
Monday 05 September 2022  14:09:18 +0000 (0:00:00.025)       0:13:39.348 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.046)       0:13:39.394 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.032)       0:13:39.426 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.024)       0:13:39.451 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.095)       0:13:39.547 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.086)       0:13:39.633 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.057)       0:13:39.691 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.053)       0:13:39.745 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.025)       0:13:39.771 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.029)       0:13:39.800 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.025)       0:13:39.826 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.064)       0:13:39.891 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.067)       0:13:39.958 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.023)       0:13:39.982 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.026)       0:13:40.009 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.025)       0:13:40.034 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.054)       0:13:40.089 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.060)       0:13:40.149 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.023)       0:13:40.172 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.028)       0:13:40.200 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.089)       0:13:40.290 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.025)       0:13:40.316 ******
Monday 05 September 2022  14:09:19 +0000 (0:00:00.024)       0:13:40.340 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.101)       0:13:40.441 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.024)       0:13:40.466 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.025)       0:13:40.491 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.026)       0:13:40.518 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.024)       0:13:40.543 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.025)       0:13:40.568 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.024)       0:13:40.593 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.025)       0:13:40.618 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.031)       0:13:40.650 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.024)       0:13:40.675 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.026)       0:13:40.701 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.045)       0:13:40.746 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.046)       0:13:40.793 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.033)       0:13:40.827 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.030)       0:13:40.857 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.032)       0:13:40.890 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.031)       0:13:40.921 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.034)       0:13:40.956 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.033)       0:13:40.989 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.033)       0:13:41.022 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.027)       0:13:41.050 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.030)       0:13:41.081 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.035)       0:13:41.116 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.033)       0:13:41.149 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.028)       0:13:41.178 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.034)       0:13:41.213 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.026)       0:13:41.240 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.029)       0:13:41.269 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.029)       0:13:41.299 ******
Monday 05 September 2022  14:09:20 +0000 (0:00:00.059)       0:13:41.358 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.048)       0:13:41.407 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.033)       0:13:41.441 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.030)       0:13:41.471 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.030)       0:13:41.501 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.035)       0:13:41.537 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.032)       0:13:41.569 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.039)       0:13:41.609 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.037)       0:13:41.646 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.031)       0:13:41.678 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.031)       0:13:41.709 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.033)       0:13:41.743 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.031)       0:13:41.774 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.032)       0:13:41.806 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.031)       0:13:41.838 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.030)       0:13:41.869 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.027)       0:13:41.896 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.034)       0:13:41.931 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.031)       0:13:41.962 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.024)       0:13:41.987 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.024)       0:13:42.012 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.025)       0:13:42.037 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.041)       0:13:42.078 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.040)       0:13:42.119 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.028)       0:13:42.148 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.029)       0:13:42.177 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.033)       0:13:42.210 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.032)       0:13:42.243 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.035)       0:13:42.278 ******
Monday 05 September 2022  14:09:21 +0000 (0:00:00.030)       0:13:42.308 ******

PLAY [k8s_cluster] ****************************************************************************************************
Monday 05 September 2022  14:09:22 +0000 (0:00:00.091)       0:13:42.400 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.050)       0:13:42.450 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.054)       0:13:42.505 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.051)       0:13:42.556 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.051)       0:13:42.607 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.079)       0:13:42.687 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.085)       0:13:42.772 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.082)       0:13:42.854 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.029)       0:13:42.884 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.024)       0:13:42.909 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.059)       0:13:42.968 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.074)       0:13:43.043 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.052)       0:13:43.095 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.055)       0:13:43.150 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.023)       0:13:43.174 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.057)       0:13:43.232 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.051)       0:13:43.283 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.051)       0:13:43.335 ******
Monday 05 September 2022  14:09:22 +0000 (0:00:00.052)       0:13:43.388 ******
Monday 05 September 2022  14:09:24 +0000 (0:00:01.826)       0:13:45.215 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node1] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node2] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
ok: [node3] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  14:09:24 +0000 (0:00:00.066)       0:13:45.282 ******
Monday 05 September 2022  14:09:24 +0000 (0:00:00.062)       0:13:45.344 ******
Monday 05 September 2022  14:09:24 +0000 (0:00:00.026)       0:13:45.370 ******
Monday 05 September 2022  14:09:25 +0000 (0:00:00.062)       0:13:45.432 ******
Monday 05 September 2022  14:09:25 +0000 (0:00:00.026)       0:13:45.459 ******
Monday 05 September 2022  14:09:25 +0000 (0:00:00.057)       0:13:45.516 ******

TASK [adduser : User | Create User Group] *****************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:25 +0000 (0:00:00.275)       0:13:45.792 ******

TASK [adduser : User | Create User] ***********************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:25 +0000 (0:00:00.329)       0:13:46.121 ******
Monday 05 September 2022  14:09:25 +0000 (0:00:00.083)       0:13:46.205 ******
Monday 05 September 2022  14:09:25 +0000 (0:00:00.063)       0:13:46.269 ******
Monday 05 September 2022  14:09:25 +0000 (0:00:00.058)       0:13:46.327 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.075)       0:13:46.403 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.052)       0:13:46.455 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.025)       0:13:46.481 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.058)       0:13:46.540 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.063)       0:13:46.603 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.061)       0:13:46.664 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.059)       0:13:46.724 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.068)       0:13:46.792 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.066)       0:13:46.859 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.074)       0:13:46.933 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.060)       0:13:46.994 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.061)       0:13:47.055 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.068)       0:13:47.124 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.061)       0:13:47.186 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.049)       0:13:47.235 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.059)       0:13:47.295 ******
Monday 05 September 2022  14:09:26 +0000 (0:00:00.057)       0:13:47.352 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.058)       0:13:47.410 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.079)       0:13:47.490 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.083)       0:13:47.573 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.058)       0:13:47.632 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.062)       0:13:47.695 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.052)       0:13:47.747 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.024)       0:13:47.772 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.026)       0:13:47.798 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.026)       0:13:47.824 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.063)       0:13:47.888 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.040)       0:13:47.929 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.040)       0:13:47.969 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.031)       0:13:48.001 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.028)       0:13:48.030 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.026)       0:13:48.057 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.030)       0:13:48.087 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.025)       0:13:48.112 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.055)       0:13:48.168 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.057)       0:13:48.225 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.058)       0:13:48.284 ******
Monday 05 September 2022  14:09:27 +0000 (0:00:00.057)       0:13:48.341 ******
Monday 05 September 2022  14:09:28 +0000 (0:00:00.058)       0:13:48.399 ******
Monday 05 September 2022  14:09:28 +0000 (0:00:00.055)       0:13:48.455 ******
Monday 05 September 2022  14:09:28 +0000 (0:00:00.086)       0:13:48.541 ******
Monday 05 September 2022  14:09:28 +0000 (0:00:00.026)       0:13:48.568 ******
Monday 05 September 2022  14:09:28 +0000 (0:00:00.053)       0:13:48.621 ******
Monday 05 September 2022  14:09:28 +0000 (0:00:00.054)       0:13:48.676 ******

TASK [kubernetes/preinstall : check if booted with ostree] ************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:28 +0000 (0:00:00.239)       0:13:48.916 ******

TASK [kubernetes/preinstall : set is_fedora_coreos] *******************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:28 +0000 (0:00:00.259)       0:13:49.175 ******

TASK [kubernetes/preinstall : set is_fedora_coreos] *******************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:28 +0000 (0:00:00.074)       0:13:49.250 ******

TASK [kubernetes/preinstall : check resolvconf] ***********************************************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:09:29 +0000 (0:00:00.246)       0:13:49.496 ******

TASK [kubernetes/preinstall : check existence of /etc/resolvconf/resolv.conf.d] ***************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:29 +0000 (0:00:00.285)       0:13:49.782 ******

TASK [kubernetes/preinstall : check status of /etc/resolv.conf] *******************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:29 +0000 (0:00:00.256)       0:13:50.038 ******

TASK [kubernetes/preinstall : get content of /etc/resolv.conf] ********************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:29 +0000 (0:00:00.253)       0:13:50.292 ******

TASK [kubernetes/preinstall : get currently configured nameservers] ***************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:30 +0000 (0:00:00.118)       0:13:50.410 ******

TASK [kubernetes/preinstall : check systemd-resolved] *****************************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:09:30 +0000 (0:00:00.254)       0:13:50.665 ******

TASK [kubernetes/preinstall : set dns facts] **************************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:30 +0000 (0:00:00.075)       0:13:50.741 ******

TASK [kubernetes/preinstall : check if kubelet is configured] *********************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:09:30 +0000 (0:00:00.275)       0:13:51.016 ******

TASK [kubernetes/preinstall : check if early DNS configuration stage] *************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:30 +0000 (0:00:00.068)       0:13:51.084 ******

TASK [kubernetes/preinstall : target resolv.conf files] ***************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:30 +0000 (0:00:00.072)       0:13:51.157 ******
Monday 05 September 2022  14:09:30 +0000 (0:00:00.057)       0:13:51.215 ******

TASK [kubernetes/preinstall : check if /etc/dhclient.conf exists] *****************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:31 +0000 (0:00:00.235)       0:13:51.450 ******
Monday 05 September 2022  14:09:31 +0000 (0:00:00.057)       0:13:51.508 ******

TASK [kubernetes/preinstall : check if /etc/dhcp/dhclient.conf exists] ************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:09:31 +0000 (0:00:00.270)       0:13:51.779 ******

TASK [kubernetes/preinstall : target dhclient conf file for /etc/dhcp/dhclient.conf] **********************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:31 +0000 (0:00:00.065)       0:13:51.844 ******

TASK [kubernetes/preinstall : target dhclient hook file for Red Hat family] *******************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:09:31 +0000 (0:00:00.067)       0:13:51.912 ******
Monday 05 September 2022  14:09:31 +0000 (0:00:00.060)       0:13:51.972 ******

TASK [kubernetes/preinstall : generate search domains to resolvconf] **************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:31 +0000 (0:00:00.114)       0:13:52.087 ******

TASK [kubernetes/preinstall : pick coredns cluster IP or default resolver] ********************************************
ok: [node2]
ok: [node1]
ok: [node3]
Monday 05 September 2022  14:09:31 +0000 (0:00:00.138)       0:13:52.225 ******

TASK [kubernetes/preinstall : generate nameservers to resolvconf] *****************************************************
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  14:09:31 +0000 (0:00:00.078)       0:13:52.303 ******

TASK [kubernetes/preinstall : gather os specific variables] ***********************************************************
ok: [node1] => (item=/kubespray/roles/kubernetes/preinstall/vars/../vars/redhat.yml)
ok: [node2] => (item=/kubespray/roles/kubernetes/preinstall/vars/../vars/redhat.yml)
ok: [node3] => (item=/kubespray/roles/kubernetes/preinstall/vars/../vars/redhat.yml)
Monday 05 September 2022  14:09:32 +0000 (0:00:00.095)       0:13:52.399 ******
Monday 05 September 2022  14:09:32 +0000 (0:00:00.092)       0:13:52.492 ******

TASK [kubernetes/preinstall : check /usr readonly] ********************************************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:32 +0000 (0:00:00.239)       0:13:52.731 ******
Monday 05 September 2022  14:09:32 +0000 (0:00:00.076)       0:13:52.807 ******
Monday 05 September 2022  14:09:32 +0000 (0:00:00.072)       0:13:52.879 ******
Monday 05 September 2022  14:09:32 +0000 (0:00:00.055)       0:13:52.935 ******
Monday 05 September 2022  14:09:32 +0000 (0:00:00.165)       0:13:53.101 ******
Monday 05 September 2022  14:09:32 +0000 (0:00:00.069)       0:13:53.171 ******
Monday 05 September 2022  14:09:32 +0000 (0:00:00.056)       0:13:53.227 ******
Monday 05 September 2022  14:09:32 +0000 (0:00:00.058)       0:13:53.286 ******
Monday 05 September 2022  14:09:33 +0000 (0:00:00.141)       0:13:53.427 ******
Monday 05 September 2022  14:09:33 +0000 (0:00:00.133)       0:13:53.560 ******
Monday 05 September 2022  14:09:33 +0000 (0:00:00.114)       0:13:53.674 ******

TASK [kubernetes/preinstall : Add domain/search/nameservers/options to resolv.conf] ***********************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:09:33 +0000 (0:00:00.381)       0:13:54.056 ******

TASK [kubernetes/preinstall : Remove search/domain/nameserver options before block] ***********************************
ok: [node1] => (item=['/etc/resolv.conf', 'search '])
ok: [node2] => (item=['/etc/resolv.conf', 'search '])
ok: [node3] => (item=['/etc/resolv.conf', 'search '])
ok: [node1] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node2] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node3] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node2] => (item=['/etc/resolv.conf', 'domain '])
ok: [node1] => (item=['/etc/resolv.conf', 'domain '])
ok: [node3] => (item=['/etc/resolv.conf', 'domain '])
ok: [node1] => (item=['/etc/resolv.conf', 'options '])
ok: [node2] => (item=['/etc/resolv.conf', 'options '])
ok: [node3] => (item=['/etc/resolv.conf', 'options '])
Monday 05 September 2022  14:09:34 +0000 (0:00:01.060)       0:13:55.116 ******

TASK [kubernetes/preinstall : Remove search/domain/nameserver options after block] ************************************
ok: [node1] => (item=['/etc/resolv.conf', 'search '])
ok: [node2] => (item=['/etc/resolv.conf', 'search '])
ok: [node3] => (item=['/etc/resolv.conf', 'search '])
ok: [node2] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node1] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node3] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node2] => (item=['/etc/resolv.conf', 'domain '])
ok: [node1] => (item=['/etc/resolv.conf', 'domain '])
ok: [node3] => (item=['/etc/resolv.conf', 'domain '])
ok: [node2] => (item=['/etc/resolv.conf', 'options '])
ok: [node1] => (item=['/etc/resolv.conf', 'options '])
ok: [node3] => (item=['/etc/resolv.conf', 'options '])
Monday 05 September 2022  14:09:35 +0000 (0:00:00.982)       0:13:56.098 ******
Monday 05 September 2022  14:09:35 +0000 (0:00:00.071)       0:13:56.170 ******
Monday 05 September 2022  14:09:35 +0000 (0:00:00.081)       0:13:56.251 ******
Monday 05 September 2022  14:09:35 +0000 (0:00:00.065)       0:13:56.317 ******

TASK [kubernetes/preinstall : NetworkManager | Check if host has NetworkManager] **************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:36 +0000 (0:00:00.263)       0:13:56.580 ******

TASK [kubernetes/preinstall : NetworkManager | Ensure NetworkManager conf.d dir] **************************************
ok: [node3]
ok: [node1]
ok: [node2]
Monday 05 September 2022  14:09:36 +0000 (0:00:00.286)       0:13:56.867 ******

TASK [kubernetes/preinstall : NetworkManager | Prevent NetworkManager from managing Calico interfaces (cali*/tunl*/vxlan.calico)] ***
ok: [node2]
ok: [node3]
ok: [node1]
Monday 05 September 2022  14:09:37 +0000 (0:00:00.596)       0:13:57.463 ******

TASK [kubernetes/preinstall : NetworkManager | Prevent NetworkManager from managing K8S interfaces (kube-ipvs0/nodelocaldns)] ***
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:37 +0000 (0:00:00.573)       0:13:58.037 ******
Monday 05 September 2022  14:09:37 +0000 (0:00:00.071)       0:13:58.108 ******
Monday 05 September 2022  14:09:37 +0000 (0:00:00.063)       0:13:58.172 ******
Monday 05 September 2022  14:09:37 +0000 (0:00:00.061)       0:13:58.233 ******
Monday 05 September 2022  14:09:37 +0000 (0:00:00.058)       0:13:58.291 ******
Monday 05 September 2022  14:09:37 +0000 (0:00:00.057)       0:13:58.349 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.058)       0:13:58.407 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.055)       0:13:58.462 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.067)       0:13:58.529 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.056)       0:13:58.586 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.060)       0:13:58.647 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.060)       0:13:58.708 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.056)       0:13:58.765 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.055)       0:13:58.820 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.065)       0:13:58.885 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.057)       0:13:58.943 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.058)       0:13:59.001 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.060)       0:13:59.061 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.071)       0:13:59.133 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.056)       0:13:59.189 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.052)       0:13:59.241 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.057)       0:13:59.298 ******
Monday 05 September 2022  14:09:38 +0000 (0:00:00.087)       0:13:59.386 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.060)       0:13:59.446 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.025)       0:13:59.472 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.059)       0:13:59.531 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.051)       0:13:59.583 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.061)       0:13:59.644 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.063)       0:13:59.708 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.076)       0:13:59.785 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.073)       0:13:59.859 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.056)       0:13:59.915 ******

TASK [kubernetes/preinstall : Configure dhclient to supersede search/domain/nameservers] ******************************
changed: [node1]
changed: [node2]
changed: [node3]
Monday 05 September 2022  14:09:39 +0000 (0:00:00.267)       0:14:00.183 ******
Monday 05 September 2022  14:09:39 +0000 (0:00:00.064)       0:14:00.247 ******

TASK [kubernetes/preinstall : Configure dhclient hooks for resolv.conf (RH-only)] *************************************
ok: [node1]
ok: [node2]
ok: [node3]
Monday 05 September 2022  14:09:40 +0000 (0:00:00.592)       0:14:00.840 ******
Monday 05 September 2022  14:09:40 +0000 (0:00:00.066)       0:14:00.907 ******
Monday 05 September 2022  14:09:40 +0000 (0:00:00.061)       0:14:00.968 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | propagate resolvconf to k8s components] *************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:09:40 +0000 (0:00:00.241)       0:14:01.209 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | reload kubelet] *************************************************
changed: [node1]
changed: [node3]
changed: [node2]
Monday 05 September 2022  14:09:41 +0000 (0:00:00.402)       0:14:01.612 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | kube-apiserver configured] **************************************
ok: [node1]
Monday 05 September 2022  14:09:41 +0000 (0:00:00.214)       0:14:01.827 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | kube-controller configured] *************************************
ok: [node1]
Monday 05 September 2022  14:09:41 +0000 (0:00:00.185)       0:14:02.013 ******
Monday 05 September 2022  14:09:41 +0000 (0:00:00.057)       0:14:02.070 ******
Monday 05 September 2022  14:09:41 +0000 (0:00:00.064)       0:14:02.135 ******
Monday 05 September 2022  14:09:41 +0000 (0:00:00.057)       0:14:02.193 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | restart kube-apiserver crio/containerd] *************************
changed: [node1]
Monday 05 September 2022  14:09:42 +0000 (0:00:00.416)       0:14:02.609 ******
FAILED - RETRYING: Preinstall | wait for the apiserver to be running (60 retries left).
FAILED - RETRYING: Preinstall | wait for the apiserver to be running (59 retries left).
FAILED - RETRYING: Preinstall | wait for the apiserver to be running (58 retries left).
FAILED - RETRYING: Preinstall | wait for the apiserver to be running (57 retries left).
FAILED - RETRYING: Preinstall | wait for the apiserver to be running (56 retries left).

RUNNING HANDLER [kubernetes/preinstall : Preinstall | wait for the apiserver to be running] ***************************
ok: [node1]
Monday 05 September 2022  14:09:51 +0000 (0:00:09.236)       0:14:11.846 ******
Monday 05 September 2022  14:09:51 +0000 (0:00:00.067)       0:14:11.913 ******
Monday 05 September 2022  14:09:51 +0000 (0:00:00.054)       0:14:11.968 ******
Monday 05 September 2022  14:09:51 +0000 (0:00:00.060)       0:14:12.028 ******
Monday 05 September 2022  14:09:51 +0000 (0:00:00.066)       0:14:12.095 ******
Monday 05 September 2022  14:09:51 +0000 (0:00:00.263)       0:14:12.358 ******
Monday 05 September 2022  14:09:52 +0000 (0:00:00.057)       0:14:12.416 ******
Monday 05 September 2022  14:09:52 +0000 (0:00:00.060)       0:14:12.477 ******

PLAY RECAP ************************************************************************************************************
localhost                  : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
node1                      : ok=706  changed=135  unreachable=0    failed=0    skipped=1212 rescued=0    ignored=3
node2                      : ok=484  changed=83   unreachable=0    failed=0    skipped=684  rescued=0    ignored=1
node3                      : ok=484  changed=83   unreachable=0    failed=0    skipped=684  rescued=0    ignored=1

Monday 05 September 2022  14:09:52 +0000 (0:00:00.068)       0:14:12.546 
===============================================================================
download : download_file | Download item ---------------------------------------------------------------------- 35.88s
download : download_container | Download image if required ---------------------------------------------------- 31.11s
kubernetes/kubeadm : Join to cluster -------------------------------------------------------------------------- 30.07s
download : download_container | Download image if required ---------------------------------------------------- 29.03s
container-engine/containerd : download_file | Download item --------------------------------------------------- 27.02s
download : download_container | Download image if required ---------------------------------------------------- 25.06s
download : download_file | Download item ---------------------------------------------------------------------- 24.71s
download : download_container | Download image if required ---------------------------------------------------- 24.26s
download : download_file | Download item ---------------------------------------------------------------------- 24.01s
container-engine/crictl : download_file | Download item ------------------------------------------------------- 22.40s
kubernetes/control-plane : kubeadm | Initialize first master -------------------------------------------------- 21.07s
download : download_container | Download image if required ---------------------------------------------------- 18.87s
download : download_container | Download image if required ---------------------------------------------------- 18.19s
bootstrap-os : Enable RHEL 8 repos ---------------------------------------------------------------------------- 18.05s
download : download_file | Download item ---------------------------------------------------------------------- 18.04s
container-engine/nerdctl : download_file | Download item ------------------------------------------------------ 16.44s
container-engine/runc : download_file | Download item --------------------------------------------------------- 16.06s
download : download_container | Download image if required ---------------------------------------------------- 15.11s
download : download_container | Download image if required ---------------------------------------------------- 14.62s
download : download_file | Download item ---------------------------------------------------------------------- 11.66s
</code></pre>
</details>

<details><summary>
Add Node</summary>
<pre><code>
$ cat inventory/mycluster/hosts.yaml
all:
  vars:
    ansible_user: admin
  hosts:
    node1:
      ansible_host: 192.168.88.211
      ip: 192.168.88.211
      access_ip: 192.168.88.211
    node2:
      ansible_host: 192.168.88.212
      ip: 192.168.88.212
      access_ip: 192.168.88.212
    node3:
      ansible_host: 192.168.88.213
      ip: 192.168.88.213
      access_ip: 192.168.88.213
    node4:
      ansible_host: 192.168.88.214
      ip: 192.168.88.214
      access_ip: 192.168.88.214
  children:
    kube_control_plane:
      hosts:
        node1:
    kube_node:
      hosts:
        node1:
        node2:
        node3:
        node4:
    etcd:
      hosts:
        node1:
    k8s_cluster:
      children:
        kube_control_plane:
        kube_node:
    calico_rr:
      hosts: {}

(venv) $ ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root --limit node4 scale.yml

PLAY [localhost] ******************************************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: kube-master

PLAY [Add kube-master nodes to kube_control_plane] ********************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: kube-node

PLAY [Add kube-node nodes to kube_node] *******************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: k8s-cluster

PLAY [Add k8s-cluster nodes to k8s_cluster] ***************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: calico-rr

PLAY [Add calico-rr nodes to calico_rr] *******************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: no-floating

PLAY [Add no-floating nodes to no_floating] ***************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: bastion

PLAY [bastion[0]] *****************************************************************************************************
skipping: no hosts matched

PLAY [Bootstrap any new workers] **************************************************************************************
Monday 05 September 2022  15:41:22 +0000 (0:00:00.036)       0:00:00.036 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.029)       0:00:00.065 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.028)       0:00:00.094 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.029)       0:00:00.123 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.028)       0:00:00.152 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.034)       0:00:00.187 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.031)       0:00:00.219 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.033)       0:00:00.253 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.032)       0:00:00.285 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.029)       0:00:00.315 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.030)       0:00:00.346 ******
Monday 05 September 2022  15:41:22 +0000 (0:00:00.029)       0:00:00.376 ******
Monday 05 September 2022  15:41:23 +0000 (0:00:00.030)       0:00:00.406 ******
Monday 05 September 2022  15:41:23 +0000 (0:00:00.033)       0:00:00.439 ******
Monday 05 September 2022  15:41:23 +0000 (0:00:00.033)       0:00:00.473 ******
Monday 05 September 2022  15:41:23 +0000 (0:00:00.031)       0:00:00.504 ******
Monday 05 September 2022  15:41:23 +0000 (0:00:00.031)       0:00:00.536 ******
Monday 05 September 2022  15:41:23 +0000 (0:00:00.038)       0:00:00.574 ******
Monday 05 September 2022  15:41:23 +0000 (0:00:00.031)       0:00:00.606 ******
Monday 05 September 2022  15:41:24 +0000 (0:00:00.882)       0:00:01.488 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node4] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  15:41:24 +0000 (0:00:00.040)       0:00:01.528 ******
Monday 05 September 2022  15:41:24 +0000 (0:00:00.063)       0:00:01.591 ******
Monday 05 September 2022  15:41:24 +0000 (0:00:00.028)       0:00:01.620 ******
Monday 05 September 2022  15:41:24 +0000 (0:00:00.030)       0:00:01.651 ******
Monday 05 September 2022  15:41:24 +0000 (0:00:00.031)       0:00:01.683 ******
Monday 05 September 2022  15:41:24 +0000 (0:00:00.028)       0:00:01.711 ******
[WARNING]: raw module does not support the environment keyword

TASK [bootstrap-os : Fetch /etc/os-release] ***************************************************************************
ok: [node4]
Monday 05 September 2022  15:41:24 +0000 (0:00:00.093)       0:00:01.804 ******
Monday 05 September 2022  15:41:24 +0000 (0:00:00.029)       0:00:01.834 ******
Monday 05 September 2022  15:41:24 +0000 (0:00:00.029)       0:00:01.863 ******
included: /kubespray/roles/bootstrap-os/tasks/bootstrap-redhat.yml for node4
Monday 05 September 2022  15:41:24 +0000 (0:00:00.046)       0:00:01.909 ******

TASK [bootstrap-os : Gather host facts to get ansible_distribution_version ansible_distribution_major_version] ********
ok: [node4]
Monday 05 September 2022  15:41:25 +0000 (0:00:00.520)       0:00:02.430 ******

TASK [bootstrap-os : Add proxy to yum.conf or dnf.conf if http_proxy is defined] **************************************
ok: [node4]
Monday 05 September 2022  15:41:25 +0000 (0:00:00.341)       0:00:02.771 ******
Monday 05 September 2022  15:41:25 +0000 (0:00:00.031)       0:00:02.803 ******

TASK [bootstrap-os : Check RHEL subscription-manager status] **********************************************************
changed: [node4]
Monday 05 September 2022  15:41:46 +0000 (0:00:21.519)       0:00:24.323 ******
Monday 05 September 2022  15:41:46 +0000 (0:00:00.036)       0:00:24.359 ******
Monday 05 September 2022  15:41:46 +0000 (0:00:00.033)       0:00:24.393 ******
Monday 05 September 2022  15:41:47 +0000 (0:00:00.037)       0:00:24.431 ******

TASK [bootstrap-os : Enable RHEL 8 repos] *****************************************************************************
ok: [node4]
Monday 05 September 2022  15:42:53 +0000 (0:01:06.878)       0:01:31.309 ******

TASK [bootstrap-os : Check presence of fastestmirror.conf] ************************************************************
ok: [node4]
Monday 05 September 2022  15:42:54 +0000 (0:00:00.326)       0:01:31.636 ******
Monday 05 September 2022  15:42:54 +0000 (0:00:00.033)       0:01:31.670 ******

TASK [bootstrap-os : Install libselinux python package] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:43:01 +0000 (0:00:07.458)       0:01:39.129 ******
Monday 05 September 2022  15:43:01 +0000 (0:00:00.046)       0:01:39.175 ******
Monday 05 September 2022  15:43:01 +0000 (0:00:00.036)       0:01:39.212 ******
Monday 05 September 2022  15:43:01 +0000 (0:00:00.039)       0:01:39.251 ******
Monday 05 September 2022  15:43:01 +0000 (0:00:00.035)       0:01:39.286 ******
Monday 05 September 2022  15:43:01 +0000 (0:00:00.033)       0:01:39.320 ******
Monday 05 September 2022  15:43:01 +0000 (0:00:00.034)       0:01:39.354 ******

TASK [bootstrap-os : Create remote_tmp for it is used by another module] **********************************************
ok: [node4]
Monday 05 September 2022  15:43:02 +0000 (0:00:00.363)       0:01:39.717 ******

TASK [bootstrap-os : Gather host facts to get ansible_os_family] ******************************************************
ok: [node4]
Monday 05 September 2022  15:43:02 +0000 (0:00:00.295)       0:01:40.013 ******

TASK [bootstrap-os : Assign inventory name to unconfigured hostnames (non-CoreOS, non-Flatcar, Suse and ClearLinux, non-Fedora)] ***
ok: [node4]
Monday 05 September 2022  15:43:03 +0000 (0:00:00.700)       0:01:40.713 ******
Monday 05 September 2022  15:43:03 +0000 (0:00:00.032)       0:01:40.745 ******
Monday 05 September 2022  15:43:03 +0000 (0:00:00.031)       0:01:40.776 ******
Monday 05 September 2022  15:43:03 +0000 (0:00:00.032)       0:01:40.808 ******

TASK [bootstrap-os : Ensure bash_completion.d folder exists] **********************************************************
ok: [node4]

PLAY [Gather facts] ***************************************************************************************************
Monday 05 September 2022  15:43:03 +0000 (0:00:00.207)       0:01:41.016 ******

TASK [Gather minimal facts] *******************************************************************************************
ok: [node4]
Monday 05 September 2022  15:43:03 +0000 (0:00:00.274)       0:01:41.291 ******

TASK [Gather necessary facts (network)] *******************************************************************************
ok: [node4]
Monday 05 September 2022  15:43:04 +0000 (0:00:00.296)       0:01:41.587 ******

TASK [Gather necessary facts (hardware)] ******************************************************************************
ok: [node4]

PLAY [Generate the etcd certificates beforehand] **********************************************************************
skipping: no hosts matched

PLAY [Download images to ansible host cache via first kube_control_plane node] ****************************************
skipping: no hosts matched

PLAY [Target only workers to get kubelet installed and checking in on any new nodes(engine)] **************************
Monday 05 September 2022  15:43:04 +0000 (0:00:00.561)       0:01:42.149 ******
Monday 05 September 2022  15:43:04 +0000 (0:00:00.029)       0:01:42.178 ******
Monday 05 September 2022  15:43:04 +0000 (0:00:00.031)       0:01:42.210 ******
Monday 05 September 2022  15:43:04 +0000 (0:00:00.032)       0:01:42.242 ******
Monday 05 September 2022  15:43:04 +0000 (0:00:00.032)       0:01:42.274 ******
Monday 05 September 2022  15:43:04 +0000 (0:00:00.030)       0:01:42.305 ******
Monday 05 September 2022  15:43:04 +0000 (0:00:00.033)       0:01:42.339 ******
Monday 05 September 2022  15:43:04 +0000 (0:00:00.036)       0:01:42.376 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.040)       0:01:42.416 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.033)       0:01:42.449 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.032)       0:01:42.481 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.033)       0:01:42.514 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.031)       0:01:42.546 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.034)       0:01:42.580 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.032)       0:01:42.613 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.035)       0:01:42.648 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.034)       0:01:42.683 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.035)       0:01:42.718 ******
Monday 05 September 2022  15:43:05 +0000 (0:00:00.031)       0:01:42.749 ******
Monday 05 September 2022  15:43:06 +0000 (0:00:00.903)       0:01:43.653 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node4] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  15:43:06 +0000 (0:00:00.039)       0:01:43.692 ******
Monday 05 September 2022  15:43:06 +0000 (0:00:00.117)       0:01:43.809 ******

TASK [kubespray-defaults : create fallback_ips_base] ******************************************************************
ok: [node4]
Monday 05 September 2022  15:43:06 +0000 (0:00:00.063)       0:01:43.873 ******

TASK [kubespray-defaults : set fallback_ips] **************************************************************************
ok: [node4]
Monday 05 September 2022  15:43:06 +0000 (0:00:00.047)       0:01:43.921 ******
Monday 05 September 2022  15:43:06 +0000 (0:00:00.031)       0:01:43.952 ******
Monday 05 September 2022  15:43:06 +0000 (0:00:00.032)       0:01:43.985 ******

TASK [adduser : User | Create User Group] *****************************************************************************
ok: [node4]
Monday 05 September 2022  15:43:06 +0000 (0:00:00.376)       0:01:44.361 ******

TASK [adduser : User | Create User] ***********************************************************************************
ok: [node4]
Monday 05 September 2022  15:43:07 +0000 (0:00:00.402)       0:01:44.764 ******

TASK [kubernetes/preinstall : Remove swapfile from /etc/fstab] ********************************************************
ok: [node4] => (item=swap)
ok: [node4] => (item=none)
Monday 05 September 2022  15:43:07 +0000 (0:00:00.550)       0:01:45.315 ******

TASK [kubernetes/preinstall : check swap] *****************************************************************************
ok: [node4]
Monday 05 September 2022  15:43:08 +0000 (0:00:00.194)       0:01:45.509 ******
Monday 05 September 2022  15:43:08 +0000 (0:00:00.035)       0:01:45.545 ******
Monday 05 September 2022  15:43:08 +0000 (0:00:00.033)       0:01:45.578 ******

TASK [kubernetes/preinstall : Stop if either kube_control_plane or kube_node group is empty] **************************
ok: [node4] => (item=kube_control_plane) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": "kube_control_plane",
    "msg": "All assertions passed"
}
ok: [node4] => (item=kube_node) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": "kube_node",
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.066)       0:01:45.644 ******

TASK [kubernetes/preinstall : Stop if etcd group is empty in external etcd mode] **************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.047)       0:01:45.692 ******

TASK [kubernetes/preinstall : Stop if non systemd OS type] ************************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.044)       0:01:45.737 ******

TASK [kubernetes/preinstall : Stop if unknown OS] *********************************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.047)       0:01:45.784 ******

TASK [kubernetes/preinstall : Stop if unknown network plugin] *********************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.047)       0:01:45.832 ******
Monday 05 September 2022  15:43:08 +0000 (0:00:00.038)       0:01:45.870 ******

TASK [kubernetes/preinstall : Stop if supported Calico versions] ******************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.045)       0:01:45.916 ******

TASK [kubernetes/preinstall : Stop if unsupported version of Kubernetes] **********************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.046)       0:01:45.963 ******

TASK [kubernetes/preinstall : Stop if known booleans are set as strings (Use JSON format on CLI: -e "{'key': true }")] ***
ok: [node4] => (item={'name': 'download_run_once', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "download_run_once",
        "value": false
    },
    "msg": "All assertions passed"
}
ok: [node4] => (item={'name': 'deploy_netchecker', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "deploy_netchecker",
        "value": false
    },
    "msg": "All assertions passed"
}
ok: [node4] => (item={'name': 'download_always_pull', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "download_always_pull",
        "value": false
    },
    "msg": "All assertions passed"
}
ok: [node4] => (item={'name': 'helm_enabled', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "helm_enabled",
        "value": false
    },
    "msg": "All assertions passed"
}
ok: [node4] => (item={'name': 'openstack_lbaas_enabled', 'value': False}) => {
    "ansible_loop_var": "item",
    "changed": false,
    "item": {
        "name": "openstack_lbaas_enabled",
        "value": false
    },
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.124)       0:01:46.087 ******
Monday 05 September 2022  15:43:08 +0000 (0:00:00.037)       0:01:46.125 ******
Monday 05 September 2022  15:43:08 +0000 (0:00:00.036)       0:01:46.162 ******

TASK [kubernetes/preinstall : Stop if memory is too small for nodes] **************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.048)       0:01:46.210 ******

TASK [kubernetes/preinstall : Stop when dynamic_kubelet_configuration enabled for kubernetes >= 1.22] *****************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.045)       0:01:46.255 ******
Monday 05 September 2022  15:43:08 +0000 (0:00:00.039)       0:01:46.294 ******

TASK [kubernetes/preinstall : Stop if ip var does not match local ips] ************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:08 +0000 (0:00:00.049)       0:01:46.344 ******

TASK [kubernetes/preinstall : Stop if access_ip is not pingable] ******************************************************
changed: [node4]
Monday 05 September 2022  15:43:09 +0000 (0:00:00.198)       0:01:46.542 ******
Monday 05 September 2022  15:43:09 +0000 (0:00:00.033)       0:01:46.576 ******
Monday 05 September 2022  15:43:09 +0000 (0:00:00.033)       0:01:46.609 ******
Monday 05 September 2022  15:43:09 +0000 (0:00:00.032)       0:01:46.641 ******
Monday 05 September 2022  15:43:09 +0000 (0:00:00.048)       0:01:46.690 ******

TASK [kubernetes/preinstall : Stop if bad hostname] *******************************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:09 +0000 (0:00:00.050)       0:01:46.741 ******
Monday 05 September 2022  15:43:09 +0000 (0:00:00.035)       0:01:46.777 ******

TASK [kubernetes/preinstall : Get current calico cluster version] *****************************************************
ok: [node4]
Monday 05 September 2022  15:43:13 +0000 (0:00:03.790)       0:01:50.567 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.040)       0:01:50.607 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.035)       0:01:50.643 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.038)       0:01:50.681 ******

TASK [kubernetes/preinstall : Check that kube_service_addresses is a network range] ***********************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:13 +0000 (0:00:00.059)       0:01:50.741 ******

TASK [kubernetes/preinstall : Check that kube_pods_subnet is a network range] *****************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:13 +0000 (0:00:00.058)       0:01:50.800 ******

TASK [kubernetes/preinstall : Check that kube_pods_subnet does not collide with kube_service_addresses] ***************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:13 +0000 (0:00:00.062)       0:01:50.862 ******

TASK [kubernetes/preinstall : Stop if unknown dns mode] ***************************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:13 +0000 (0:00:00.046)       0:01:50.908 ******

TASK [kubernetes/preinstall : Stop if unknown kube proxy mode] ********************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:13 +0000 (0:00:00.044)       0:01:50.953 ******

TASK [kubernetes/preinstall : Stop if unknown cert_management] ********************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:13 +0000 (0:00:00.042)       0:01:50.996 ******

TASK [kubernetes/preinstall : Stop if unknown resolvconf_mode] ********************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:13 +0000 (0:00:00.045)       0:01:51.041 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.032)       0:01:51.074 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.034)       0:01:51.109 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.033)       0:01:51.143 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.037)       0:01:51.180 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.032)       0:01:51.212 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.032)       0:01:51.245 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.032)       0:01:51.278 ******

TASK [kubernetes/preinstall : Ensure minimum containerd version] ******************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:43:13 +0000 (0:00:00.051)       0:01:51.329 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.032)       0:01:51.362 ******
Monday 05 September 2022  15:43:13 +0000 (0:00:00.031)       0:01:51.394 ******

TASK [kubernetes/preinstall : check if booted with ostree] ************************************************************
ok: [node4]
Monday 05 September 2022  15:43:14 +0000 (0:00:00.195)       0:01:51.590 ******

TASK [kubernetes/preinstall : set is_fedora_coreos] *******************************************************************
ok: [node4]
Monday 05 September 2022  15:43:14 +0000 (0:00:00.344)       0:01:51.935 ******

TASK [kubernetes/preinstall : set is_fedora_coreos] *******************************************************************
ok: [node4]
Monday 05 September 2022  15:43:14 +0000 (0:00:00.045)       0:01:51.980 ******

TASK [kubernetes/preinstall : check resolvconf] ***********************************************************************
ok: [node4]
Monday 05 September 2022  15:43:14 +0000 (0:00:00.194)       0:01:52.174 ******

TASK [kubernetes/preinstall : check existence of /etc/resolvconf/resolv.conf.d] ***************************************
ok: [node4]
Monday 05 September 2022  15:43:14 +0000 (0:00:00.199)       0:01:52.374 ******

TASK [kubernetes/preinstall : check status of /etc/resolv.conf] *******************************************************
ok: [node4]
Monday 05 September 2022  15:43:15 +0000 (0:00:00.195)       0:01:52.569 ******

TASK [kubernetes/preinstall : get content of /etc/resolv.conf] ********************************************************
ok: [node4]
Monday 05 September 2022  15:43:15 +0000 (0:00:00.336)       0:01:52.906 ******

TASK [kubernetes/preinstall : get currently configured nameservers] ***************************************************
ok: [node4]
Monday 05 September 2022  15:43:15 +0000 (0:00:00.067)       0:01:52.973 ******

TASK [kubernetes/preinstall : check systemd-resolved] *****************************************************************
ok: [node4]
Monday 05 September 2022  15:43:15 +0000 (0:00:00.214)       0:01:53.188 ******

TASK [kubernetes/preinstall : set dns facts] **************************************************************************
ok: [node4]
Monday 05 September 2022  15:43:15 +0000 (0:00:00.065)       0:01:53.253 ******

TASK [kubernetes/preinstall : check if kubelet is configured] *********************************************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.196)       0:01:53.450 ******

TASK [kubernetes/preinstall : check if early DNS configuration stage] *************************************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.039)       0:01:53.489 ******

TASK [kubernetes/preinstall : target resolv.conf files] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.043)       0:01:53.532 ******
Monday 05 September 2022  15:43:16 +0000 (0:00:00.030)       0:01:53.563 ******

TASK [kubernetes/preinstall : check if /etc/dhclient.conf exists] *****************************************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.193)       0:01:53.757 ******
Monday 05 September 2022  15:43:16 +0000 (0:00:00.033)       0:01:53.790 ******

TASK [kubernetes/preinstall : check if /etc/dhcp/dhclient.conf exists] ************************************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.183)       0:01:53.974 ******

TASK [kubernetes/preinstall : target dhclient conf file for /etc/dhcp/dhclient.conf] **********************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.046)       0:01:54.020 ******

TASK [kubernetes/preinstall : target dhclient hook file for Red Hat family] *******************************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.044)       0:01:54.065 ******
Monday 05 September 2022  15:43:16 +0000 (0:00:00.038)       0:01:54.103 ******

TASK [kubernetes/preinstall : generate search domains to resolvconf] **************************************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.053)       0:01:54.157 ******

TASK [kubernetes/preinstall : pick coredns cluster IP or default resolver] ********************************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.068)       0:01:54.226 ******

TASK [kubernetes/preinstall : generate nameservers to resolvconf] *****************************************************
ok: [node4]
Monday 05 September 2022  15:43:16 +0000 (0:00:00.063)       0:01:54.290 ******

TASK [kubernetes/preinstall : gather os specific variables] ***********************************************************
ok: [node4] => (item=/kubespray/roles/kubernetes/preinstall/vars/../vars/redhat.yml)
Monday 05 September 2022  15:43:16 +0000 (0:00:00.063)       0:01:54.354 ******
Monday 05 September 2022  15:43:16 +0000 (0:00:00.035)       0:01:54.389 ******

TASK [kubernetes/preinstall : check /usr readonly] ********************************************************************
ok: [node4]
Monday 05 September 2022  15:43:17 +0000 (0:00:00.207)       0:01:54.596 ******
Monday 05 September 2022  15:43:17 +0000 (0:00:00.030)       0:01:54.626 ******
Monday 05 September 2022  15:43:17 +0000 (0:00:00.028)       0:01:54.655 ******
Monday 05 September 2022  15:43:17 +0000 (0:00:00.028)       0:01:54.684 ******

TASK [kubernetes/preinstall : Create kubernetes directories] **********************************************************
changed: [node4] => (item=/etc/kubernetes)
changed: [node4] => (item=/etc/kubernetes/ssl)
changed: [node4] => (item=/etc/kubernetes/manifests)
changed: [node4] => (item=/usr/local/bin/kubernetes-scripts)
changed: [node4] => (item=/usr/libexec/kubernetes/kubelet-plugins/volume/exec)
Monday 05 September 2022  15:43:18 +0000 (0:00:00.944)       0:01:55.628 ******

TASK [kubernetes/preinstall : Create other directories] ***************************************************************
ok: [node4] => (item=/usr/local/bin)
Monday 05 September 2022  15:43:18 +0000 (0:00:00.204)       0:01:55.832 ******

TASK [kubernetes/preinstall : Check if kubernetes kubeadm compat cert dir exists] *************************************
ok: [node4]
Monday 05 September 2022  15:43:18 +0000 (0:00:00.194)       0:01:56.027 ******

TASK [kubernetes/preinstall : Create kubernetes kubeadm compat cert dir (kubernetes/kubeadm issue 1498)] **************
changed: [node4]
Monday 05 September 2022  15:43:18 +0000 (0:00:00.204)       0:01:56.231 ******

TASK [kubernetes/preinstall : Create cni directories] *****************************************************************
changed: [node4] => (item=/etc/cni/net.d)
changed: [node4] => (item=/opt/cni/bin)
changed: [node4] => (item=/var/lib/calico)
Monday 05 September 2022  15:43:19 +0000 (0:00:00.577)       0:01:56.809 ******
Monday 05 September 2022  15:43:19 +0000 (0:00:00.044)       0:01:56.853 ******
Monday 05 September 2022  15:43:19 +0000 (0:00:00.042)       0:01:56.896 ******

TASK [kubernetes/preinstall : Add domain/search/nameservers/options to resolv.conf] ***********************************
ok: [node4]
Monday 05 September 2022  15:43:19 +0000 (0:00:00.343)       0:01:57.239 ******

TASK [kubernetes/preinstall : Remove search/domain/nameserver options before block] ***********************************
ok: [node4] => (item=['/etc/resolv.conf', 'search '])
ok: [node4] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node4] => (item=['/etc/resolv.conf', 'domain '])
ok: [node4] => (item=['/etc/resolv.conf', 'options '])
Monday 05 September 2022  15:43:20 +0000 (0:00:00.888)       0:01:58.128 ******

TASK [kubernetes/preinstall : Remove search/domain/nameserver options after block] ************************************
ok: [node4] => (item=['/etc/resolv.conf', 'search '])
ok: [node4] => (item=['/etc/resolv.conf', 'nameserver '])
ok: [node4] => (item=['/etc/resolv.conf', 'domain '])
ok: [node4] => (item=['/etc/resolv.conf', 'options '])
Monday 05 September 2022  15:43:21 +0000 (0:00:00.756)       0:01:58.884 ******
Monday 05 September 2022  15:43:21 +0000 (0:00:00.041)       0:01:58.926 ******
Monday 05 September 2022  15:43:21 +0000 (0:00:00.037)       0:01:58.964 ******
Monday 05 September 2022  15:43:21 +0000 (0:00:00.036)       0:01:59.001 ******

TASK [kubernetes/preinstall : NetworkManager | Check if host has NetworkManager] **************************************
ok: [node4]
Monday 05 September 2022  15:43:21 +0000 (0:00:00.200)       0:01:59.201 ******

TASK [kubernetes/preinstall : NetworkManager | Ensure NetworkManager conf.d dir] **************************************
ok: [node4]
Monday 05 September 2022  15:43:22 +0000 (0:00:00.199)       0:01:59.400 ******

TASK [kubernetes/preinstall : NetworkManager | Prevent NetworkManager from managing Calico interfaces (cali*/tunl*/vxlan.calico)] ***
changed: [node4]
Monday 05 September 2022  15:43:22 +0000 (0:00:00.691)       0:02:00.091 ******

TASK [kubernetes/preinstall : NetworkManager | Prevent NetworkManager from managing K8S interfaces (kube-ipvs0/nodelocaldns)] ***
changed: [node4]
Monday 05 September 2022  15:43:23 +0000 (0:00:00.476)       0:02:00.568 ******
Monday 05 September 2022  15:43:23 +0000 (0:00:00.035)       0:02:00.604 ******
Monday 05 September 2022  15:43:23 +0000 (0:00:00.034)       0:02:00.639 ******
Monday 05 September 2022  15:43:23 +0000 (0:00:00.040)       0:02:00.679 ******
Monday 05 September 2022  15:43:23 +0000 (0:00:00.035)       0:02:00.714 ******
Monday 05 September 2022  15:43:23 +0000 (0:00:00.038)       0:02:00.753 ******

TASK [kubernetes/preinstall : Remove legacy docker repo file] *********************************************************
ok: [node4]
Monday 05 September 2022  15:43:23 +0000 (0:00:00.204)       0:02:00.957 ******
Monday 05 September 2022  15:43:23 +0000 (0:00:00.033)       0:02:00.991 ******
Monday 05 September 2022  15:43:23 +0000 (0:00:00.035)       0:02:01.026 ******

TASK [kubernetes/preinstall : Update common_required_pkgs with ipvsadm when kube_proxy_mode is ipvs] ******************
ok: [node4]
Monday 05 September 2022  15:43:23 +0000 (0:00:00.044)       0:02:01.071 ******

TASK [kubernetes/preinstall : Install packages requirements] **********************************************************
ok: [node4]
Monday 05 September 2022  15:43:30 +0000 (0:00:07.323)       0:02:08.395 ******
Monday 05 September 2022  15:43:31 +0000 (0:00:00.033)       0:02:08.429 ******

TASK [kubernetes/preinstall : Confirm selinux deployed] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:43:31 +0000 (0:00:00.192)       0:02:08.621 ******

TASK [kubernetes/preinstall : Set selinux policy] *********************************************************************
ok: [node4]
Monday 05 September 2022  15:43:31 +0000 (0:00:00.507)       0:02:09.129 ******
Monday 05 September 2022  15:43:31 +0000 (0:00:00.034)       0:02:09.163 ******

TASK [kubernetes/preinstall : Stat sysctl file configuration] *********************************************************
ok: [node4]
Monday 05 September 2022  15:43:31 +0000 (0:00:00.186)       0:02:09.350 ******

TASK [kubernetes/preinstall : Change sysctl file path to link source if linked] ***************************************
ok: [node4]
Monday 05 September 2022  15:43:32 +0000 (0:00:00.049)       0:02:09.399 ******

TASK [kubernetes/preinstall : Make sure sysctl file path folder exists] ***********************************************
ok: [node4]
Monday 05 September 2022  15:43:32 +0000 (0:00:00.206)       0:02:09.606 ******

TASK [kubernetes/preinstall : Enable ip forwarding] *******************************************************************
ok: [node4]
Monday 05 September 2022  15:43:32 +0000 (0:00:00.359)       0:02:09.965 ******
Monday 05 September 2022  15:43:32 +0000 (0:00:00.038)       0:02:10.004 ******

TASK [kubernetes/preinstall : Check if we need to set fs.may_detach_mounts] *******************************************
ok: [node4]
Monday 05 September 2022  15:43:32 +0000 (0:00:00.183)       0:02:10.188 ******
Monday 05 September 2022  15:43:32 +0000 (0:00:00.033)       0:02:10.221 ******

TASK [kubernetes/preinstall : Ensure kube-bench parameters are set] ***************************************************
ok: [node4] => (item={'name': 'vm.overcommit_memory', 'value': 1})
ok: [node4] => (item={'name': 'kernel.panic', 'value': 10})
ok: [node4] => (item={'name': 'kernel.panic_on_oops', 'value': 1})
Monday 05 September 2022  15:43:33 +0000 (0:00:00.543)       0:02:10.765 ******

TASK [kubernetes/preinstall : Check dummy module] *********************************************************************
ok: [node4]
Monday 05 September 2022  15:43:33 +0000 (0:00:00.341)       0:02:11.107 ******

TASK [kubernetes/preinstall : Hosts | create list from inventory] *****************************************************
ok: [node4]
Monday 05 September 2022  15:43:33 +0000 (0:00:00.108)       0:02:11.215 ******

TASK [kubernetes/preinstall : Hosts | populate inventory into hosts file] *********************************************
changed: [node4]
Monday 05 September 2022  15:43:34 +0000 (0:00:00.199)       0:02:11.415 ******
Monday 05 September 2022  15:43:34 +0000 (0:00:00.032)       0:02:11.448 ******

TASK [kubernetes/preinstall : Hosts | Retrieve hosts file content] ****************************************************
ok: [node4]
Monday 05 September 2022  15:43:34 +0000 (0:00:00.192)       0:02:11.641 ******

TASK [kubernetes/preinstall : Hosts | Extract existing entries for localhost from hosts file] *************************
ok: [node4] => (item=127.0.0.1 localhost node4)
ok: [node4] => (item=127.0.0.1 localhost.localdomain localhost)
ok: [node4] => (item=127.0.0.1 localhost4.localdomain4 localhost4 localhost localhost.localdomain)
ok: [node4] => (item=::1 localhost node4)
ok: [node4] => (item=::1 localhost.localdomain localhost)
ok: [node4] => (item=::1 localhost6.localdomain6 localhost6 localhost6.localdomain)
Monday 05 September 2022  15:43:34 +0000 (0:00:00.335)       0:02:11.976 ******

TASK [kubernetes/preinstall : Hosts | Update target hosts file entries dict with required entries] ********************
ok: [node4] => (item={'key': '127.0.0.1', 'value': {'expected': ['localhost', 'localhost.localdomain']}})
ok: [node4] => (item={'key': '::1', 'value': {'expected': ['localhost6', 'localhost6.localdomain'], 'unexpected': ['localhost', 'localhost.localdomain']}})
Monday 05 September 2022  15:43:34 +0000 (0:00:00.078)       0:02:12.054 ******

TASK [kubernetes/preinstall : Hosts | Update (if necessary) hosts file] ***********************************************
ok: [node4] => (item={'key': '127.0.0.1', 'value': ['localhost4.localdomain4', 'localhost4', 'localhost', 'localhost.localdomain']})
ok: [node4] => (item={'key': '::1', 'value': ['localhost6.localdomain6', 'localhost6', 'localhost6.localdomain']})
Monday 05 September 2022  15:43:35 +0000 (0:00:00.385)       0:02:12.440 ******

TASK [kubernetes/preinstall : Update facts] ***************************************************************************
ok: [node4]
Monday 05 September 2022  15:43:35 +0000 (0:00:00.286)       0:02:12.727 ******

TASK [kubernetes/preinstall : Configure dhclient to supersede search/domain/nameservers] ******************************
changed: [node4]
Monday 05 September 2022  15:43:35 +0000 (0:00:00.195)       0:02:12.923 ******
Monday 05 September 2022  15:43:35 +0000 (0:00:00.037)       0:02:12.960 ******

TASK [kubernetes/preinstall : Configure dhclient hooks for resolv.conf (RH-only)] *************************************
changed: [node4]
Monday 05 September 2022  15:43:36 +0000 (0:00:00.498)       0:02:13.459 ******
Monday 05 September 2022  15:43:36 +0000 (0:00:00.033)       0:02:13.493 ******
Monday 05 September 2022  15:43:36 +0000 (0:00:00.033)       0:02:13.527 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | propagate resolvconf to k8s components] *************************
changed: [node4]
Monday 05 September 2022  15:43:36 +0000 (0:00:00.191)       0:02:13.718 ******

RUNNING HANDLER [kubernetes/preinstall : Preinstall | reload NetworkManager] ******************************************
changed: [node4]
Monday 05 September 2022  15:43:37 +0000 (0:00:00.683)       0:02:14.401 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.039)       0:02:14.441 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.030)       0:02:14.472 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.030)       0:02:14.503 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.033)       0:02:14.536 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.032)       0:02:14.569 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.030)       0:02:14.600 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.033)       0:02:14.633 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.037)       0:02:14.671 ******

TASK [kubernetes/preinstall : Check if we are running inside a Azure VM] **********************************************
ok: [node4]
Monday 05 September 2022  15:43:37 +0000 (0:00:00.192)       0:02:14.864 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.032)       0:02:14.896 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.033)       0:02:14.930 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.033)       0:02:14.963 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.037)       0:02:15.001 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.039)       0:02:15.041 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.043)       0:02:15.084 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.034)       0:02:15.119 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.035)       0:02:15.154 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.036)       0:02:15.191 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.035)       0:02:15.226 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.038)       0:02:15.265 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.040)       0:02:15.305 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.054)       0:02:15.360 ******
Monday 05 September 2022  15:43:37 +0000 (0:00:00.035)       0:02:15.395 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.036)       0:02:15.432 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.035)       0:02:15.467 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.053)       0:02:15.520 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.034)       0:02:15.555 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.036)       0:02:15.591 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.038)       0:02:15.629 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.037)       0:02:15.667 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.036)       0:02:15.703 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.049)       0:02:15.753 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.036)       0:02:15.790 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.036)       0:02:15.827 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.040)       0:02:15.867 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.035)       0:02:15.903 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.038)       0:02:15.942 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.033)       0:02:15.975 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.035)       0:02:16.011 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.040)       0:02:16.052 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.034)       0:02:16.086 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.036)       0:02:16.122 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.035)       0:02:16.158 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.034)       0:02:16.193 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.036)       0:02:16.229 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.033)       0:02:16.263 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.054)       0:02:16.317 ******
Monday 05 September 2022  15:43:38 +0000 (0:00:00.045)       0:02:16.363 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.036)       0:02:16.399 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.035)       0:02:16.435 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.036)       0:02:16.472 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.034)       0:02:16.506 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.068)       0:02:16.575 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.036)       0:02:16.611 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.034)       0:02:16.645 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.043)       0:02:16.689 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.041)       0:02:16.730 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.042)       0:02:16.772 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.034)       0:02:16.807 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.038)       0:02:16.845 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.036)       0:02:16.881 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.047)       0:02:16.928 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.034)       0:02:16.963 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.037)       0:02:17.000 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.039)       0:02:17.040 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.035)       0:02:17.076 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.026)       0:02:17.102 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.035)       0:02:17.138 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.049)       0:02:17.187 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.036)       0:02:17.224 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.040)       0:02:17.264 ******
Monday 05 September 2022  15:43:39 +0000 (0:00:00.037)       0:02:17.302 ******

TASK [container-engine/containerd-common : containerd-common | check if fedora coreos] ********************************
ok: [node4]
Monday 05 September 2022  15:43:40 +0000 (0:00:00.197)       0:02:17.499 ******

TASK [container-engine/containerd-common : containerd-common | set is_ostree] *****************************************
ok: [node4]
Monday 05 September 2022  15:43:40 +0000 (0:00:00.046)       0:02:17.545 ******
Monday 05 September 2022  15:43:40 +0000 (0:00:00.042)       0:02:17.587 ******

TASK [container-engine/runc : runc | set is_ostree] *******************************************************************
ok: [node4]
Monday 05 September 2022  15:43:40 +0000 (0:00:00.046)       0:02:17.634 ******

TASK [container-engine/runc : runc | Uninstall runc package managed by package manager] *******************************
ok: [node4]
Monday 05 September 2022  15:43:47 +0000 (0:00:07.187)       0:02:24.821 ******
included: /kubespray/roles/container-engine/runc/tasks/../../../download/tasks/download_file.yml for node4
Monday 05 September 2022  15:43:47 +0000 (0:00:00.073)       0:02:24.895 ******

TASK [container-engine/runc : download_file | Starting download of file] **********************************************
ok: [node4] => {
    "msg": "https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64"
}
Monday 05 September 2022  15:43:47 +0000 (0:00:00.476)       0:02:25.372 ******

TASK [container-engine/runc : download_file | Set pathname of cached file] ********************************************
ok: [node4]
Monday 05 September 2022  15:43:48 +0000 (0:00:00.456)       0:02:25.828 ******

TASK [container-engine/runc : download_file | Create dest directory on node] ******************************************
ok: [node4]
Monday 05 September 2022  15:43:49 +0000 (0:00:00.764)       0:02:26.592 ******
Monday 05 September 2022  15:43:49 +0000 (0:00:00.058)       0:02:26.651 ******
Monday 05 September 2022  15:43:49 +0000 (0:00:00.055)       0:02:26.707 ******

TASK [container-engine/runc : download_file | Download item] **********************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:43:51 +0000 (0:00:01.837)       0:02:28.545 ******
Monday 05 September 2022  15:43:51 +0000 (0:00:00.037)       0:02:28.582 ******
Monday 05 September 2022  15:43:51 +0000 (0:00:00.033)       0:02:28.616 ******
Monday 05 September 2022  15:43:51 +0000 (0:00:00.038)       0:02:28.655 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:43:51 +0000 (0:00:00.044)       0:02:28.699 ******
Monday 05 September 2022  15:43:51 +0000 (0:00:00.438)       0:02:29.137 ******

TASK [container-engine/runc : Copy runc binary from download dir] *****************************************************
changed: [node4]
Monday 05 September 2022  15:43:52 +0000 (0:00:00.707)       0:02:29.844 ******

TASK [container-engine/runc : runc | Remove orphaned binary] **********************************************************
ok: [node4]
Monday 05 September 2022  15:43:52 +0000 (0:00:00.201)       0:02:30.046 ******
included: /kubespray/roles/container-engine/crictl/tasks/crictl.yml for node4
Monday 05 September 2022  15:43:52 +0000 (0:00:00.039)       0:02:30.086 ******
included: /kubespray/roles/container-engine/crictl/tasks/../../../download/tasks/download_file.yml for node4
Monday 05 September 2022  15:43:52 +0000 (0:00:00.046)       0:02:30.132 ******

TASK [container-engine/crictl : download_file | Starting download of file] ********************************************
ok: [node4] => {
    "msg": "https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  15:43:53 +0000 (0:00:00.460)       0:02:30.593 ******

TASK [container-engine/crictl : download_file | Set pathname of cached file] ******************************************
ok: [node4]
Monday 05 September 2022  15:43:53 +0000 (0:00:00.428)       0:02:31.021 ******

TASK [container-engine/crictl : download_file | Create dest directory on node] ****************************************
ok: [node4]
Monday 05 September 2022  15:43:54 +0000 (0:00:00.748)       0:02:31.769 ******
Monday 05 September 2022  15:43:54 +0000 (0:00:00.044)       0:02:31.814 ******
Monday 05 September 2022  15:43:54 +0000 (0:00:00.050)       0:02:31.864 ******

TASK [container-engine/crictl : download_file | Download item] ********************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:43:56 +0000 (0:00:01.694)       0:02:33.559 ******
Monday 05 September 2022  15:43:56 +0000 (0:00:00.034)       0:02:33.593 ******
Monday 05 September 2022  15:43:56 +0000 (0:00:00.037)       0:02:33.630 ******
Monday 05 September 2022  15:43:56 +0000 (0:00:00.034)       0:02:33.665 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:43:56 +0000 (0:00:00.043)       0:02:33.709 ******

TASK [container-engine/crictl : extract_file | Unpacking archive] *****************************************************
ok: [node4]
Monday 05 September 2022  15:43:58 +0000 (0:00:02.350)       0:02:36.060 ******

TASK [container-engine/crictl : Install crictl config] ****************************************************************
ok: [node4]
Monday 05 September 2022  15:43:59 +0000 (0:00:00.597)       0:02:36.657 ******

TASK [container-engine/crictl : Copy crictl binary from download dir] *************************************************
changed: [node4]
Monday 05 September 2022  15:43:59 +0000 (0:00:00.417)       0:02:37.075 ******

TASK [container-engine/crictl : Set fact crictl_installed] ************************************************************
ok: [node4]
Monday 05 September 2022  15:43:59 +0000 (0:00:00.041)       0:02:37.117 ******
included: /kubespray/roles/container-engine/nerdctl/tasks/../../../download/tasks/download_file.yml for node4
Monday 05 September 2022  15:43:59 +0000 (0:00:00.039)       0:02:37.156 ******

TASK [container-engine/nerdctl : download_file | Starting download of file] *******************************************
ok: [node4] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  15:44:00 +0000 (0:00:00.422)       0:02:37.578 ******

TASK [container-engine/nerdctl : download_file | Set pathname of cached file] *****************************************
ok: [node4]
Monday 05 September 2022  15:44:00 +0000 (0:00:00.422)       0:02:38.001 ******

TASK [container-engine/nerdctl : download_file | Create dest directory on node] ***************************************
ok: [node4]
Monday 05 September 2022  15:44:01 +0000 (0:00:00.734)       0:02:38.735 ******
Monday 05 September 2022  15:44:01 +0000 (0:00:00.040)       0:02:38.776 ******
Monday 05 September 2022  15:44:01 +0000 (0:00:00.040)       0:02:38.817 ******

TASK [container-engine/nerdctl : download_file | Download item] *******************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:03 +0000 (0:00:01.892)       0:02:40.710 ******
Monday 05 September 2022  15:44:03 +0000 (0:00:00.034)       0:02:40.744 ******
Monday 05 September 2022  15:44:03 +0000 (0:00:00.032)       0:02:40.776 ******
Monday 05 September 2022  15:44:03 +0000 (0:00:00.031)       0:02:40.808 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:03 +0000 (0:00:00.038)       0:02:40.847 ******

TASK [container-engine/nerdctl : extract_file | Unpacking archive] ****************************************************
ok: [node4]
Monday 05 September 2022  15:44:05 +0000 (0:00:02.045)       0:02:42.892 ******

TASK [container-engine/nerdctl : nerdctl | Copy nerdctl binary from download dir] *************************************
changed: [node4]
Monday 05 September 2022  15:44:05 +0000 (0:00:00.350)       0:02:43.243 ******
Monday 05 September 2022  15:44:05 +0000 (0:00:00.027)       0:02:43.270 ******

TASK [container-engine/containerd : containerd | Remove any package manager controlled containerd package] ************
ok: [node4]
Monday 05 September 2022  15:44:12 +0000 (0:00:07.112)       0:02:50.382 ******

TASK [container-engine/containerd : containerd | Remove containerd repository] ****************************************
ok: [node4]
Monday 05 September 2022  15:44:13 +0000 (0:00:00.190)       0:02:50.573 ******
Monday 05 September 2022  15:44:13 +0000 (0:00:00.038)       0:02:50.612 ******
included: /kubespray/roles/container-engine/containerd/tasks/../../../download/tasks/download_file.yml for node4
Monday 05 September 2022  15:44:13 +0000 (0:00:00.116)       0:02:50.728 ******

TASK [container-engine/containerd : download_file | Starting download of file] ****************************************
ok: [node4] => {
    "msg": "https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz"
}
Monday 05 September 2022  15:44:13 +0000 (0:00:00.452)       0:02:51.180 ******

TASK [container-engine/containerd : download_file | Set pathname of cached file] **************************************
ok: [node4]
Monday 05 September 2022  15:44:14 +0000 (0:00:00.439)       0:02:51.620 ******

TASK [container-engine/containerd : download_file | Create dest directory on node] ************************************
ok: [node4]
Monday 05 September 2022  15:44:14 +0000 (0:00:00.773)       0:02:52.393 ******
Monday 05 September 2022  15:44:15 +0000 (0:00:00.043)       0:02:52.436 ******
Monday 05 September 2022  15:44:15 +0000 (0:00:00.041)       0:02:52.478 ******

TASK [container-engine/containerd : download_file | Download item] ****************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:16 +0000 (0:00:01.717)       0:02:54.195 ******
Monday 05 September 2022  15:44:16 +0000 (0:00:00.035)       0:02:54.230 ******
Monday 05 September 2022  15:44:16 +0000 (0:00:00.035)       0:02:54.266 ******
Monday 05 September 2022  15:44:16 +0000 (0:00:00.032)       0:02:54.298 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:16 +0000 (0:00:00.040)       0:02:54.339 ******
Monday 05 September 2022  15:44:17 +0000 (0:00:00.594)       0:02:54.934 ******

TASK [container-engine/containerd : containerd | Unpack containerd archive] *******************************************
changed: [node4]
Monday 05 September 2022  15:44:20 +0000 (0:00:02.597)       0:02:57.531 ******

TASK [container-engine/containerd : containerd | Remove orphaned binary] **********************************************
ok: [node4] => (item=containerd)
ok: [node4] => (item=containerd-shim)
ok: [node4] => (item=containerd-shim-runc-v1)
ok: [node4] => (item=containerd-shim-runc-v2)
ok: [node4] => (item=ctr)
Monday 05 September 2022  15:44:21 +0000 (0:00:00.922)       0:02:58.453 ******

TASK [container-engine/containerd : containerd | Generate systemd service for containerd] *****************************
changed: [node4]
Monday 05 September 2022  15:44:21 +0000 (0:00:00.492)       0:02:58.946 ******

TASK [container-engine/containerd : containerd | Ensure containerd directories exist] *********************************
ok: [node4] => (item=/etc/systemd/system/containerd.service.d)
ok: [node4] => (item=/etc/containerd)
ok: [node4] => (item=/var/lib/containerd)
ok: [node4] => (item=/run/containerd)
Monday 05 September 2022  15:44:22 +0000 (0:00:00.723)       0:02:59.669 ******
Monday 05 September 2022  15:44:22 +0000 (0:00:00.026)       0:02:59.695 ******

TASK [container-engine/containerd : containerd | Copy containerd config file] *****************************************
ok: [node4]
[WARNING]: flush_handlers task does not support when conditional
Monday 05 September 2022  15:44:22 +0000 (0:00:00.443)       0:03:00.139 ******

RUNNING HANDLER [container-engine/containerd : restart containerd] ****************************************************
changed: [node4]
Monday 05 September 2022  15:44:22 +0000 (0:00:00.181)       0:03:00.320 ******

RUNNING HANDLER [container-engine/containerd : Containerd | restart containerd] ***************************************
changed: [node4]
Monday 05 September 2022  15:44:23 +0000 (0:00:00.709)       0:03:01.030 ******

RUNNING HANDLER [container-engine/containerd : Containerd | wait for containerd] **************************************
changed: [node4]
Monday 05 September 2022  15:44:23 +0000 (0:00:00.206)       0:03:01.237 ******

RUNNING HANDLER [container-engine/crictl : Get crictl completion] *****************************************************
ok: [node4]
Monday 05 September 2022  15:44:24 +0000 (0:00:00.198)       0:03:01.435 ******

RUNNING HANDLER [container-engine/crictl : Install crictl completion] *************************************************
changed: [node4]
Monday 05 September 2022  15:44:24 +0000 (0:00:00.475)       0:03:01.910 ******

RUNNING HANDLER [container-engine/nerdctl : Get nerdctl completion] ***************************************************
ok: [node4]
Monday 05 September 2022  15:44:24 +0000 (0:00:00.214)       0:03:02.125 ******

RUNNING HANDLER [container-engine/nerdctl : Install nerdctl completion] ***********************************************
changed: [node4]
Monday 05 September 2022  15:44:25 +0000 (0:00:00.526)       0:03:02.651 ******

TASK [container-engine/containerd : containerd | Ensure containerd is started and enabled] ****************************
ok: [node4]
Monday 05 September 2022  15:44:25 +0000 (0:00:00.321)       0:03:02.972 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.041)       0:03:03.014 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.025)       0:03:03.040 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.026)       0:03:03.066 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.026)       0:03:03.093 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.026)       0:03:03.120 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.025)       0:03:03.146 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.025)       0:03:03.171 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.025)       0:03:03.197 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.046)       0:03:03.244 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.026)       0:03:03.270 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.025)       0:03:03.296 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.027)       0:03:03.323 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.027)       0:03:03.350 ******
Monday 05 September 2022  15:44:25 +0000 (0:00:00.026)       0:03:03.377 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.026)       0:03:03.403 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.039)       0:03:03.443 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.037)       0:03:03.480 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.027)       0:03:03.507 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.026)       0:03:03.534 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.054)       0:03:03.589 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.026)       0:03:03.615 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.058)       0:03:03.673 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.030)       0:03:03.704 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.018)       0:03:03.722 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.028)       0:03:03.750 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.027)       0:03:03.778 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.026)       0:03:03.804 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.027)       0:03:03.831 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.027)       0:03:03.859 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.026)       0:03:03.886 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.025)       0:03:03.911 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.035)       0:03:03.947 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.032)       0:03:03.980 ******

TASK [download : prep_download | Set a few facts] *********************************************************************
ok: [node4]
Monday 05 September 2022  15:44:26 +0000 (0:00:00.036)       0:03:04.016 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.026)       0:03:04.042 ******

TASK [download : prep_download | Set image pull/info command for containerd] ******************************************
ok: [node4]
Monday 05 September 2022  15:44:26 +0000 (0:00:00.040)       0:03:04.083 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.026)       0:03:04.109 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.029)       0:03:04.138 ******

TASK [download : prep_download | Set image pull/info command for containerd on localhost] *****************************
ok: [node4]
Monday 05 September 2022  15:44:26 +0000 (0:00:00.039)       0:03:04.177 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.029)       0:03:04.207 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.039)       0:03:04.246 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.034)       0:03:04.281 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.025)       0:03:04.306 ******
Monday 05 September 2022  15:44:26 +0000 (0:00:00.025)       0:03:04.332 ******

TASK [download : prep_download | Register docker images info] *********************************************************
ok: [node4]
Monday 05 September 2022  15:44:27 +0000 (0:00:00.340)       0:03:04.673 ******

TASK [download : prep_download | Create staging directory on remote node] *********************************************
changed: [node4]
Monday 05 September 2022  15:44:27 +0000 (0:00:00.193)       0:03:04.866 ******
Monday 05 September 2022  15:44:27 +0000 (0:00:00.031)       0:03:04.898 ******
Monday 05 September 2022  15:44:27 +0000 (0:00:00.037)       0:03:04.936 ******
included: /kubespray/roles/container-engine/nerdctl/tasks/../../../download/tasks/download_file.yml for node4
Monday 05 September 2022  15:44:27 +0000 (0:00:00.051)       0:03:04.987 ******

TASK [container-engine/nerdctl : download_file | Starting download of file] *******************************************
ok: [node4] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  15:44:28 +0000 (0:00:00.475)       0:03:05.463 ******

TASK [container-engine/nerdctl : download_file | Set pathname of cached file] *****************************************
ok: [node4]
Monday 05 September 2022  15:44:28 +0000 (0:00:00.503)       0:03:05.966 ******

TASK [container-engine/nerdctl : download_file | Create dest directory on node] ***************************************
changed: [node4]
Monday 05 September 2022  15:44:29 +0000 (0:00:00.998)       0:03:06.965 ******
Monday 05 September 2022  15:44:29 +0000 (0:00:00.035)       0:03:07.000 ******
Monday 05 September 2022  15:44:29 +0000 (0:00:00.038)       0:03:07.038 ******

TASK [container-engine/nerdctl : download_file | Download item] *******************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:31 +0000 (0:00:01.788)       0:03:08.827 ******
Monday 05 September 2022  15:44:31 +0000 (0:00:00.030)       0:03:08.857 ******
Monday 05 September 2022  15:44:31 +0000 (0:00:00.030)       0:03:08.887 ******
Monday 05 September 2022  15:44:31 +0000 (0:00:00.027)       0:03:08.915 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:31 +0000 (0:00:00.033)       0:03:08.949 ******

TASK [container-engine/nerdctl : extract_file | Unpacking archive] ****************************************************
ok: [node4]
Monday 05 September 2022  15:44:33 +0000 (0:00:02.010)       0:03:10.959 ******

TASK [container-engine/nerdctl : nerdctl | Copy nerdctl binary from download dir] *************************************
ok: [node4]
Monday 05 September 2022  15:44:33 +0000 (0:00:00.339)       0:03:11.299 ******
Monday 05 September 2022  15:44:33 +0000 (0:00:00.030)       0:03:11.330 ******
included: /kubespray/roles/download/tasks/download_file.yml for node4 => (item={'key': 'cni', 'value': {'enabled': True, 'file': True, 'version': 'v1.0.1', 'dest': '/tmp/releases/cni-plugins-linux-amd64-v1.0.1.tgz', 'sha256': '5238fbb2767cbf6aae736ad97a7aa29167525dcd405196dfbc064672a730d3cf', 'url': 'https://github.com/containernetworking/plugins/releases/download/v1.0.1/cni-plugins-linux-amd64-v1.0.1.tgz', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node4 => (item={'key': 'kubeadm', 'value': {'enabled': True, 'file': True, 'version': 'v1.22.8', 'dest': '/tmp/releases/kubeadm-v1.22.8-amd64', 'sha256': 'fc10b4e5b66c9bfa6dc297bbb4a93f58051a6069c969905ef23c19680d8d49dc', 'url': 'https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubeadm', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node4 => (item={'key': 'kubelet', 'value': {'enabled': True, 'file': True, 'version': 'v1.22.8', 'dest': '/tmp/releases/kubelet-v1.22.8-amd64', 'sha256': '2e6d1774f18c4d4527c3b9197a64ea5705edcf1b547c77b3e683458d771f3ce7', 'url': 'https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubelet', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node4 => (item={'key': 'crictl', 'value': {'file': True, 'enabled': True, 'version': 'v1.22.0', 'dest': '/tmp/releases/crictl-v1.22.0-linux-amd64.tar.gz', 'sha256': '45e0556c42616af60ebe93bf4691056338b3ea0001c0201a6a8ff8b1dbc0652a', 'url': 'https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz', 'unarchive': True, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node4 => (item={'key': 'runc', 'value': {'file': True, 'enabled': True, 'version': 'v1.0.3', 'dest': '/tmp/releases/runc', 'sha256': '5d4c0e5a4e8d6ccbb9c6696bb239f31cfab8d94b15801bafe09aaee600714f61', 'url': 'https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node4 => (item={'key': 'containerd', 'value': {'enabled': True, 'file': True, 'version': '1.5.8', 'dest': '/tmp/releases/containerd-1.5.8-linux-amd64.tar.gz', 'sha256': 'feeda3f563edf0294e33b6c4b89bd7dbe0ee182ca61a2f9b8c3de2766bcbc99b', 'url': 'https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node4 => (item={'key': 'nerdctl', 'value': {'file': True, 'enabled': True, 'version': '0.15.0', 'dest': '/tmp/releases/nerdctl-0.15.0-linux-amd64.tar.gz', 'sha256': '1371da3f6bd461f331946654f6dd3ef2ef4b9da0dd7bc5f78ed1166f32ad5adc', 'url': 'https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz', 'unarchive': True, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_file.yml for node4 => (item={'key': 'calicoctl', 'value': {'enabled': True, 'file': True, 'version': 'v3.20.3', 'dest': '/tmp/releases/calicoctl', 'sha256': '29bec97b1dfc135b830b0cbfd3dfe216f00e97e9e6ef08e620d81d4a09db6393', 'url': 'https://github.com/projectcalico/calicoctl/releases/download/v3.20.3/calicoctl-linux-amd64', 'unarchive': False, 'owner': 'root', 'mode': '0755', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node4 => (item={'key': 'calico_node', 'value': {'enabled': True, 'container': True, 'repo': 'quay.io/calico/node', 'tag': 'v3.20.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node4 => (item={'key': 'calico_cni', 'value': {'enabled': True, 'container': True, 'repo': 'quay.io/calico/cni', 'tag': 'v3.20.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node4 => (item={'key': 'calico_flexvol', 'value': {'enabled': True, 'container': True, 'repo': 'quay.io/calico/pod2daemon-flexvol', 'tag': 'v3.20.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node4 => (item={'key': 'calico_policy', 'value': {'enabled': True, 'container': True, 'repo': 'quay.io/calico/kube-controllers', 'tag': 'v3.20.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node4 => (item={'key': 'pod_infra', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/pause', 'tag': '3.3', 'sha256': '', 'groups': ['k8s_cluster']}})
included: /kubespray/roles/download/tasks/download_container.yml for node4 => (item={'key': 'nginx', 'value': {'enabled': True, 'container': True, 'repo': 'docker.io/library/nginx', 'tag': '1.21.4', 'sha256': '', 'groups': ['kube_node']}})
included: /kubespray/roles/download/tasks/download_container.yml for node4 => (item={'key': 'nodelocaldns', 'value': {'enabled': True, 'container': True, 'repo': 'k8s.gcr.io/dns/k8s-dns-node-cache', 'tag': '1.21.1', 'sha256': '', 'groups': ['k8s_cluster']}})
Monday 05 September 2022  15:44:35 +0000 (0:00:01.640)       0:03:12.970 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node4] => {
    "msg": "https://github.com/containernetworking/plugins/releases/download/v1.0.1/cni-plugins-linux-amd64-v1.0.1.tgz"
}
Monday 05 September 2022  15:44:35 +0000 (0:00:00.037)       0:03:13.008 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node4]
Monday 05 September 2022  15:44:35 +0000 (0:00:00.035)       0:03:13.044 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node4]
Monday 05 September 2022  15:44:35 +0000 (0:00:00.202)       0:03:13.247 ******
Monday 05 September 2022  15:44:35 +0000 (0:00:00.031)       0:03:13.278 ******
Monday 05 September 2022  15:44:35 +0000 (0:00:00.034)       0:03:13.313 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:36 +0000 (0:00:00.330)       0:03:13.643 ******
Monday 05 September 2022  15:44:36 +0000 (0:00:00.024)       0:03:13.668 ******
Monday 05 September 2022  15:44:36 +0000 (0:00:00.023)       0:03:13.692 ******
Monday 05 September 2022  15:44:36 +0000 (0:00:00.024)       0:03:13.716 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:36 +0000 (0:00:00.034)       0:03:13.751 ******
Monday 05 September 2022  15:44:36 +0000 (0:00:00.024)       0:03:13.776 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node4] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubeadm"
}
Monday 05 September 2022  15:44:36 +0000 (0:00:00.037)       0:03:13.813 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node4]
Monday 05 September 2022  15:44:36 +0000 (0:00:00.034)       0:03:13.847 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node4]
Monday 05 September 2022  15:44:36 +0000 (0:00:00.196)       0:03:14.044 ******
Monday 05 September 2022  15:44:36 +0000 (0:00:00.031)       0:03:14.076 ******
Monday 05 September 2022  15:44:36 +0000 (0:00:00.039)       0:03:14.115 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:37 +0000 (0:00:00.332)       0:03:14.448 ******
Monday 05 September 2022  15:44:37 +0000 (0:00:00.024)       0:03:14.472 ******
Monday 05 September 2022  15:44:37 +0000 (0:00:00.025)       0:03:14.498 ******
Monday 05 September 2022  15:44:37 +0000 (0:00:00.027)       0:03:14.526 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:37 +0000 (0:00:00.038)       0:03:14.564 ******
Monday 05 September 2022  15:44:37 +0000 (0:00:00.025)       0:03:14.590 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node4] => {
    "msg": "https://storage.googleapis.com/kubernetes-release/release/v1.22.8/bin/linux/amd64/kubelet"
}
Monday 05 September 2022  15:44:37 +0000 (0:00:00.046)       0:03:14.636 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node4]
Monday 05 September 2022  15:44:37 +0000 (0:00:00.039)       0:03:14.676 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node4]
Monday 05 September 2022  15:44:37 +0000 (0:00:00.201)       0:03:14.877 ******
Monday 05 September 2022  15:44:37 +0000 (0:00:00.036)       0:03:14.913 ******
Monday 05 September 2022  15:44:37 +0000 (0:00:00.033)       0:03:14.946 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:37 +0000 (0:00:00.370)       0:03:15.317 ******
Monday 05 September 2022  15:44:37 +0000 (0:00:00.025)       0:03:15.342 ******
Monday 05 September 2022  15:44:37 +0000 (0:00:00.024)       0:03:15.366 ******
Monday 05 September 2022  15:44:37 +0000 (0:00:00.023)       0:03:15.390 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:38 +0000 (0:00:00.037)       0:03:15.428 ******
Monday 05 September 2022  15:44:38 +0000 (0:00:00.025)       0:03:15.454 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node4] => {
    "msg": "https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.22.0/crictl-v1.22.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  15:44:38 +0000 (0:00:00.034)       0:03:15.488 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node4]
Monday 05 September 2022  15:44:38 +0000 (0:00:00.036)       0:03:15.524 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node4]
Monday 05 September 2022  15:44:38 +0000 (0:00:00.202)       0:03:15.727 ******
Monday 05 September 2022  15:44:38 +0000 (0:00:00.031)       0:03:15.759 ******
Monday 05 September 2022  15:44:38 +0000 (0:00:00.033)       0:03:15.792 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:38 +0000 (0:00:00.309)       0:03:16.102 ******
Monday 05 September 2022  15:44:38 +0000 (0:00:00.026)       0:03:16.129 ******
Monday 05 September 2022  15:44:38 +0000 (0:00:00.025)       0:03:16.154 ******
Monday 05 September 2022  15:44:38 +0000 (0:00:00.024)       0:03:16.178 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:38 +0000 (0:00:00.039)       0:03:16.218 ******

TASK [download : extract_file | Unpacking archive] ********************************************************************
ok: [node4]
Monday 05 September 2022  15:44:39 +0000 (0:00:01.009)       0:03:17.228 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node4] => {
    "msg": "https://github.com/opencontainers/runc/releases/download/v1.0.3/runc.amd64"
}
Monday 05 September 2022  15:44:39 +0000 (0:00:00.035)       0:03:17.263 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node4]
Monday 05 September 2022  15:44:39 +0000 (0:00:00.033)       0:03:17.297 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node4]
Monday 05 September 2022  15:44:40 +0000 (0:00:00.202)       0:03:17.499 ******
Monday 05 September 2022  15:44:40 +0000 (0:00:00.035)       0:03:17.534 ******
Monday 05 September 2022  15:44:40 +0000 (0:00:00.031)       0:03:17.566 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:40 +0000 (0:00:00.303)       0:03:17.869 ******
Monday 05 September 2022  15:44:40 +0000 (0:00:00.023)       0:03:17.892 ******
Monday 05 September 2022  15:44:40 +0000 (0:00:00.022)       0:03:17.915 ******
Monday 05 September 2022  15:44:40 +0000 (0:00:00.024)       0:03:17.940 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:40 +0000 (0:00:00.036)       0:03:17.977 ******
Monday 05 September 2022  15:44:40 +0000 (0:00:00.026)       0:03:18.003 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node4] => {
    "msg": "https://github.com/containerd/containerd/releases/download/v1.5.8/containerd-1.5.8-linux-amd64.tar.gz"
}
Monday 05 September 2022  15:44:40 +0000 (0:00:00.036)       0:03:18.039 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node4]
Monday 05 September 2022  15:44:40 +0000 (0:00:00.034)       0:03:18.074 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node4]
Monday 05 September 2022  15:44:40 +0000 (0:00:00.208)       0:03:18.282 ******
Monday 05 September 2022  15:44:40 +0000 (0:00:00.035)       0:03:18.318 ******
Monday 05 September 2022  15:44:40 +0000 (0:00:00.034)       0:03:18.353 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:41 +0000 (0:00:00.310)       0:03:18.663 ******
Monday 05 September 2022  15:44:41 +0000 (0:00:00.028)       0:03:18.692 ******
Monday 05 September 2022  15:44:41 +0000 (0:00:00.025)       0:03:18.717 ******
Monday 05 September 2022  15:44:41 +0000 (0:00:00.029)       0:03:18.746 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:41 +0000 (0:00:00.034)       0:03:18.781 ******
Monday 05 September 2022  15:44:41 +0000 (0:00:00.027)       0:03:18.808 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node4] => {
    "msg": "https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz"
}
Monday 05 September 2022  15:44:41 +0000 (0:00:00.036)       0:03:18.845 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node4]
Monday 05 September 2022  15:44:41 +0000 (0:00:00.035)       0:03:18.881 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node4]
Monday 05 September 2022  15:44:41 +0000 (0:00:00.206)       0:03:19.087 ******
Monday 05 September 2022  15:44:41 +0000 (0:00:00.030)       0:03:19.118 ******
Monday 05 September 2022  15:44:41 +0000 (0:00:00.032)       0:03:19.151 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:42 +0000 (0:00:00.309)       0:03:19.460 ******
Monday 05 September 2022  15:44:42 +0000 (0:00:00.030)       0:03:19.491 ******
Monday 05 September 2022  15:44:42 +0000 (0:00:00.033)       0:03:19.524 ******
Monday 05 September 2022  15:44:42 +0000 (0:00:00.031)       0:03:19.556 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:42 +0000 (0:00:00.047)       0:03:19.604 ******

TASK [download : extract_file | Unpacking archive] ********************************************************************
ok: [node4]
Monday 05 September 2022  15:44:43 +0000 (0:00:00.829)       0:03:20.434 ******

TASK [download : download_file | Starting download of file] ***********************************************************
ok: [node4] => {
    "msg": "https://github.com/projectcalico/calicoctl/releases/download/v3.20.3/calicoctl-linux-amd64"
}
Monday 05 September 2022  15:44:43 +0000 (0:00:00.034)       0:03:20.468 ******

TASK [download : download_file | Set pathname of cached file] *********************************************************
ok: [node4]
Monday 05 September 2022  15:44:43 +0000 (0:00:00.032)       0:03:20.500 ******

TASK [download : download_file | Create dest directory on node] *******************************************************
ok: [node4]
Monday 05 September 2022  15:44:43 +0000 (0:00:00.197)       0:03:20.698 ******
Monday 05 September 2022  15:44:43 +0000 (0:00:00.033)       0:03:20.732 ******
Monday 05 September 2022  15:44:43 +0000 (0:00:00.035)       0:03:20.767 ******

TASK [download : download_file | Download item] ***********************************************************************
ok: [node4 -> 192.168.88.214]
Monday 05 September 2022  15:44:43 +0000 (0:00:00.322)       0:03:21.089 ******
Monday 05 September 2022  15:44:43 +0000 (0:00:00.022)       0:03:21.112 ******
Monday 05 September 2022  15:44:43 +0000 (0:00:00.024)       0:03:21.136 ******
Monday 05 September 2022  15:44:43 +0000 (0:00:00.023)       0:03:21.159 ******
included: /kubespray/roles/download/tasks/extract_file.yml for node4
Monday 05 September 2022  15:44:43 +0000 (0:00:00.032)       0:03:21.191 ******
Monday 05 September 2022  15:44:43 +0000 (0:00:00.024)       0:03:21.216 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:44:43 +0000 (0:00:00.033)       0:03:21.250 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node4] => {
    "msg": "quay.io/calico/node"
}
Monday 05 September 2022  15:44:43 +0000 (0:00:00.033)       0:03:21.283 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node4]
Monday 05 September 2022  15:44:43 +0000 (0:00:00.032)       0:03:21.316 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node4]
Monday 05 September 2022  15:44:43 +0000 (0:00:00.037)       0:03:21.353 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node4]
Monday 05 September 2022  15:44:43 +0000 (0:00:00.031)       0:03:21.385 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:44 +0000 (0:00:00.031)       0:03:21.416 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.024)       0:03:21.441 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:44 +0000 (0:00:00.032)       0:03:21.474 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.025)       0:03:21.499 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.023)       0:03:21.522 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node4]
Monday 05 September 2022  15:44:44 +0000 (0:00:00.048)       0:03:21.570 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.023)       0:03:21.594 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node4
Monday 05 September 2022  15:44:44 +0000 (0:00:00.040)       0:03:21.635 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node4]
Monday 05 September 2022  15:44:44 +0000 (0:00:00.331)       0:03:21.967 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node4]
Monday 05 September 2022  15:44:44 +0000 (0:00:00.037)       0:03:22.004 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.025)       0:03:22.030 ******

TASK [download : debug] ***********************************************************************************************
ok: [node4] => {
    "msg": "Pull quay.io/calico/node:v3.20.3 required is: False"
}
Monday 05 September 2022  15:44:44 +0000 (0:00:00.040)       0:03:22.071 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.025)       0:03:22.096 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.024)       0:03:22.120 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.032)       0:03:22.153 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.035)       0:03:22.189 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.028)       0:03:22.217 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.030)       0:03:22.247 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.024)       0:03:22.272 ******
Monday 05 September 2022  15:44:44 +0000 (0:00:00.024)       0:03:22.296 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.190)       0:03:22.486 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.034)       0:03:22.520 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node4] => {
    "msg": "quay.io/calico/cni"
}
Monday 05 September 2022  15:44:45 +0000 (0:00:00.034)       0:03:22.554 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.034)       0:03:22.589 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.038)       0:03:22.628 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.038)       0:03:22.667 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.034)       0:03:22.701 ******
Monday 05 September 2022  15:44:45 +0000 (0:00:00.025)       0:03:22.727 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.037)       0:03:22.764 ******
Monday 05 September 2022  15:44:45 +0000 (0:00:00.023)       0:03:22.787 ******
Monday 05 September 2022  15:44:45 +0000 (0:00:00.028)       0:03:22.816 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.038)       0:03:22.855 ******
Monday 05 September 2022  15:44:45 +0000 (0:00:00.027)       0:03:22.882 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node4
Monday 05 September 2022  15:44:45 +0000 (0:00:00.041)       0:03:22.923 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.316)       0:03:23.240 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node4]
Monday 05 September 2022  15:44:45 +0000 (0:00:00.039)       0:03:23.279 ******
Monday 05 September 2022  15:44:45 +0000 (0:00:00.026)       0:03:23.306 ******

TASK [download : debug] ***********************************************************************************************
ok: [node4] => {
    "msg": "Pull quay.io/calico/cni:v3.20.3 required is: False"
}
Monday 05 September 2022  15:44:45 +0000 (0:00:00.037)       0:03:23.343 ******
Monday 05 September 2022  15:44:45 +0000 (0:00:00.024)       0:03:23.367 ******
Monday 05 September 2022  15:44:45 +0000 (0:00:00.025)       0:03:23.392 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.029)       0:03:23.421 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.035)       0:03:23.456 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.031)       0:03:23.488 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.027)       0:03:23.516 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.022)       0:03:23.538 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.028)       0:03:23.567 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node4]
Monday 05 September 2022  15:44:46 +0000 (0:00:00.193)       0:03:23.761 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:44:46 +0000 (0:00:00.043)       0:03:23.805 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node4] => {
    "msg": "quay.io/calico/pod2daemon-flexvol"
}
Monday 05 September 2022  15:44:46 +0000 (0:00:00.036)       0:03:23.841 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node4]
Monday 05 September 2022  15:44:46 +0000 (0:00:00.039)       0:03:23.881 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node4]
Monday 05 September 2022  15:44:46 +0000 (0:00:00.036)       0:03:23.917 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node4]
Monday 05 September 2022  15:44:46 +0000 (0:00:00.034)       0:03:23.952 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:46 +0000 (0:00:00.033)       0:03:23.986 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.024)       0:03:24.010 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:46 +0000 (0:00:00.047)       0:03:24.058 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.034)       0:03:24.092 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.031)       0:03:24.123 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node4]
Monday 05 September 2022  15:44:46 +0000 (0:00:00.038)       0:03:24.162 ******
Monday 05 September 2022  15:44:46 +0000 (0:00:00.025)       0:03:24.187 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node4
Monday 05 September 2022  15:44:46 +0000 (0:00:00.043)       0:03:24.230 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node4]
Monday 05 September 2022  15:44:47 +0000 (0:00:00.319)       0:03:24.550 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node4]
Monday 05 September 2022  15:44:47 +0000 (0:00:00.035)       0:03:24.586 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.028)       0:03:24.614 ******

TASK [download : debug] ***********************************************************************************************
ok: [node4] => {
    "msg": "Pull quay.io/calico/pod2daemon-flexvol:v3.20.3 required is: False"
}
Monday 05 September 2022  15:44:47 +0000 (0:00:00.036)       0:03:24.651 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.025)       0:03:24.676 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.024)       0:03:24.701 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.037)       0:03:24.738 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.054)       0:03:24.793 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.038)       0:03:24.831 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.034)       0:03:24.866 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.025)       0:03:24.892 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.024)       0:03:24.917 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node4]
Monday 05 September 2022  15:44:47 +0000 (0:00:00.184)       0:03:25.101 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:44:47 +0000 (0:00:00.031)       0:03:25.133 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node4] => {
    "msg": "quay.io/calico/kube-controllers"
}
Monday 05 September 2022  15:44:47 +0000 (0:00:00.036)       0:03:25.170 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node4]
Monday 05 September 2022  15:44:47 +0000 (0:00:00.033)       0:03:25.204 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node4]
Monday 05 September 2022  15:44:47 +0000 (0:00:00.034)       0:03:25.239 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node4]
Monday 05 September 2022  15:44:47 +0000 (0:00:00.038)       0:03:25.277 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:47 +0000 (0:00:00.033)       0:03:25.311 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.023)       0:03:25.335 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:47 +0000 (0:00:00.035)       0:03:25.370 ******
Monday 05 September 2022  15:44:47 +0000 (0:00:00.024)       0:03:25.395 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.025)       0:03:25.420 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node4]
Monday 05 September 2022  15:44:48 +0000 (0:00:00.039)       0:03:25.459 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.025)       0:03:25.485 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node4
Monday 05 September 2022  15:44:48 +0000 (0:00:00.043)       0:03:25.528 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node4]
Monday 05 September 2022  15:44:48 +0000 (0:00:00.312)       0:03:25.841 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node4]
Monday 05 September 2022  15:44:48 +0000 (0:00:00.042)       0:03:25.883 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.026)       0:03:25.909 ******

TASK [download : debug] ***********************************************************************************************
ok: [node4] => {
    "msg": "Pull quay.io/calico/kube-controllers:v3.20.3 required is: False"
}
Monday 05 September 2022  15:44:48 +0000 (0:00:00.033)       0:03:25.943 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.026)       0:03:25.970 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.023)       0:03:25.994 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.029)       0:03:26.023 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.034)       0:03:26.058 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.035)       0:03:26.094 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.023)       0:03:26.118 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.024)       0:03:26.142 ******
Monday 05 September 2022  15:44:48 +0000 (0:00:00.024)       0:03:26.167 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node4]
Monday 05 September 2022  15:44:48 +0000 (0:00:00.192)       0:03:26.359 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:44:48 +0000 (0:00:00.033)       0:03:26.393 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node4] => {
    "msg": "k8s.gcr.io/pause"
}
Monday 05 September 2022  15:44:49 +0000 (0:00:00.035)       0:03:26.429 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node4]
Monday 05 September 2022  15:44:49 +0000 (0:00:00.035)       0:03:26.465 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node4]
Monday 05 September 2022  15:44:49 +0000 (0:00:00.037)       0:03:26.502 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node4]
Monday 05 September 2022  15:44:49 +0000 (0:00:00.032)       0:03:26.534 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:49 +0000 (0:00:00.035)       0:03:26.569 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.023)       0:03:26.593 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:49 +0000 (0:00:00.034)       0:03:26.628 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.023)       0:03:26.651 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.027)       0:03:26.679 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node4]
Monday 05 September 2022  15:44:49 +0000 (0:00:00.039)       0:03:26.719 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.026)       0:03:26.746 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node4
Monday 05 September 2022  15:44:49 +0000 (0:00:00.045)       0:03:26.791 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node4]
Monday 05 September 2022  15:44:49 +0000 (0:00:00.323)       0:03:27.115 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node4]
Monday 05 September 2022  15:44:49 +0000 (0:00:00.039)       0:03:27.154 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.027)       0:03:27.181 ******

TASK [download : debug] ***********************************************************************************************
ok: [node4] => {
    "msg": "Pull k8s.gcr.io/pause:3.3 required is: False"
}
Monday 05 September 2022  15:44:49 +0000 (0:00:00.035)       0:03:27.217 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.025)       0:03:27.242 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.026)       0:03:27.268 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.035)       0:03:27.303 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.040)       0:03:27.344 ******
Monday 05 September 2022  15:44:49 +0000 (0:00:00.041)       0:03:27.386 ******
Monday 05 September 2022  15:44:50 +0000 (0:00:00.031)       0:03:27.417 ******
Monday 05 September 2022  15:44:50 +0000 (0:00:00.025)       0:03:27.443 ******
Monday 05 September 2022  15:44:50 +0000 (0:00:00.024)       0:03:27.468 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node4]
Monday 05 September 2022  15:44:50 +0000 (0:00:00.184)       0:03:27.652 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:44:50 +0000 (0:00:00.032)       0:03:27.685 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node4] => {
    "msg": "docker.io/library/nginx"
}
Monday 05 September 2022  15:44:50 +0000 (0:00:00.035)       0:03:27.720 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node4]
Monday 05 September 2022  15:44:50 +0000 (0:00:00.034)       0:03:27.755 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node4]
Monday 05 September 2022  15:44:50 +0000 (0:00:00.038)       0:03:27.794 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node4]
Monday 05 September 2022  15:44:50 +0000 (0:00:00.034)       0:03:27.828 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:50 +0000 (0:00:00.032)       0:03:27.861 ******
Monday 05 September 2022  15:44:50 +0000 (0:00:00.032)       0:03:27.893 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:50 +0000 (0:00:00.035)       0:03:27.928 ******
Monday 05 September 2022  15:44:50 +0000 (0:00:00.023)       0:03:27.952 ******
Monday 05 September 2022  15:44:50 +0000 (0:00:00.026)       0:03:27.979 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node4]
Monday 05 September 2022  15:44:50 +0000 (0:00:00.040)       0:03:28.020 ******
Monday 05 September 2022  15:44:50 +0000 (0:00:00.033)       0:03:28.053 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node4
Monday 05 September 2022  15:44:50 +0000 (0:00:00.046)       0:03:28.100 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.313)       0:03:28.413 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.039)       0:03:28.453 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.028)       0:03:28.481 ******

TASK [download : debug] ***********************************************************************************************
ok: [node4] => {
    "msg": "Pull docker.io/library/nginx:1.21.4 required is: False"
}
Monday 05 September 2022  15:44:51 +0000 (0:00:00.037)       0:03:28.519 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.028)       0:03:28.548 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.026)       0:03:28.574 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.031)       0:03:28.606 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.036)       0:03:28.643 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.031)       0:03:28.674 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.025)       0:03:28.700 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.023)       0:03:28.724 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.025)       0:03:28.749 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.187)       0:03:28.937 ******

TASK [download : set default values for flag variables] ***************************************************************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.032)       0:03:28.969 ******

TASK [download : set_container_facts | Display the name of the image being processed] *********************************
ok: [node4] => {
    "msg": "k8s.gcr.io/dns/k8s-dns-node-cache"
}
Monday 05 September 2022  15:44:51 +0000 (0:00:00.037)       0:03:29.006 ******

TASK [download : set_container_facts | Set if containers should be pulled by digest] **********************************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.038)       0:03:29.044 ******

TASK [download : set_container_facts | Define by what name to pull the image] *****************************************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.040)       0:03:29.085 ******

TASK [download : set_container_facts | Define file name of image] *****************************************************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.032)       0:03:29.118 ******

TASK [download : set_container_facts | Define path of image] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.033)       0:03:29.151 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.024)       0:03:29.176 ******

TASK [download : Set image save/load command for containerd] **********************************************************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.035)       0:03:29.212 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.024)       0:03:29.236 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.024)       0:03:29.260 ******

TASK [download : Set image save/load command for containerd on localhost] *********************************************
ok: [node4]
Monday 05 September 2022  15:44:51 +0000 (0:00:00.041)       0:03:29.302 ******
Monday 05 September 2022  15:44:51 +0000 (0:00:00.026)       0:03:29.328 ******
included: /kubespray/roles/download/tasks/check_pull_required.yml for node4
Monday 05 September 2022  15:44:51 +0000 (0:00:00.046)       0:03:29.374 ******

TASK [download : check_pull_required |  Generate a list of information about the images on a node] ********************
ok: [node4]
Monday 05 September 2022  15:44:52 +0000 (0:00:00.314)       0:03:29.689 ******

TASK [download : check_pull_required | Set pull_required if the desired image is not yet loaded] **********************
ok: [node4]
Monday 05 September 2022  15:44:52 +0000 (0:00:00.037)       0:03:29.726 ******
Monday 05 September 2022  15:44:52 +0000 (0:00:00.026)       0:03:29.753 ******

TASK [download : debug] ***********************************************************************************************
ok: [node4] => {
    "msg": "Pull k8s.gcr.io/dns/k8s-dns-node-cache:1.21.1 required is: False"
}
Monday 05 September 2022  15:44:52 +0000 (0:00:00.041)       0:03:29.794 ******
Monday 05 September 2022  15:44:52 +0000 (0:00:00.024)       0:03:29.819 ******
Monday 05 September 2022  15:44:52 +0000 (0:00:00.024)       0:03:29.843 ******
Monday 05 September 2022  15:44:52 +0000 (0:00:00.029)       0:03:29.873 ******
Monday 05 September 2022  15:44:52 +0000 (0:00:00.039)       0:03:29.912 ******
Monday 05 September 2022  15:44:52 +0000 (0:00:00.032)       0:03:29.945 ******
Monday 05 September 2022  15:44:52 +0000 (0:00:00.027)       0:03:29.972 ******
Monday 05 September 2022  15:44:52 +0000 (0:00:00.026)       0:03:29.998 ******
Monday 05 September 2022  15:44:52 +0000 (0:00:00.029)       0:03:30.027 ******

TASK [download : download_container | Remove container image from cache] **********************************************
ok: [node4]
Monday 05 September 2022  15:44:52 +0000 (0:00:00.201)       0:03:30.229 ******

TASK [adduser : User | Create User Group] *****************************************************************************
ok: [node4]
Monday 05 September 2022  15:44:53 +0000 (0:00:00.198)       0:03:30.428 ******

TASK [adduser : User | Create User] ***********************************************************************************
ok: [node4]
Monday 05 September 2022  15:44:53 +0000 (0:00:00.235)       0:03:30.663 ******

TASK [adduser : User | Create User Group] *****************************************************************************
ok: [node4]
Monday 05 September 2022  15:44:53 +0000 (0:00:00.200)       0:03:30.864 ******

TASK [adduser : User | Create User] ***********************************************************************************
ok: [node4]
Monday 05 September 2022  15:44:53 +0000 (0:00:00.254)       0:03:31.118 ******
included: /kubespray/roles/etcd/tasks/check_certs.yml for node4
Monday 05 September 2022  15:44:53 +0000 (0:00:00.043)       0:03:31.161 ******

TASK [etcd : Check_certs | Register certs that have already been generated on first etcd node] ************************
ok: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:44:54 +0000 (0:00:00.385)       0:03:31.547 ******

TASK [etcd : Check_certs | Set default value for 'sync_certs', 'gen_certs' and 'etcd_secret_changed' to false] ********
ok: [node4]
Monday 05 September 2022  15:44:54 +0000 (0:00:00.032)       0:03:31.580 ******
Monday 05 September 2022  15:44:54 +0000 (0:00:00.071)       0:03:31.652 ******

TASK [etcd : Check certs | Register ca and etcd node certs on kubernetes hosts] ***************************************
ok: [node4] => (item=ca.pem)
ok: [node4] => (item=node-node4.pem)
ok: [node4] => (item=node-node4-key.pem)
Monday 05 September 2022  15:44:54 +0000 (0:00:00.522)       0:03:32.175 ******

TASK [etcd : Check_certs | Set 'gen_certs' to true if expected certificates are not on the first etcd node] ***********
ok: [node4] => (item=/etc/ssl/etcd/ssl/ca.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/admin-node1.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/admin-node1-key.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/member-node1.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/member-node1-key.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/node-node1.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/node-node1-key.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/node-node2.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/node-node2-key.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/node-node3.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/node-node3-key.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/node-node4.pem)
ok: [node4] => (item=/etc/ssl/etcd/ssl/node-node4-key.pem)
Monday 05 September 2022  15:44:55 +0000 (0:00:00.260)       0:03:32.436 ******

TASK [etcd : Check_certs | Set 'gen_master_certs' object to track whether member and admin certs exist on first etcd node] ***
ok: [node4]
Monday 05 September 2022  15:44:55 +0000 (0:00:00.039)       0:03:32.476 ******

TASK [etcd : Check_certs | Set 'gen_node_certs' object to track whether node certs exist on first etcd node] **********
ok: [node4]
Monday 05 September 2022  15:44:55 +0000 (0:00:00.039)       0:03:32.516 ******
Monday 05 September 2022  15:44:55 +0000 (0:00:00.025)       0:03:32.541 ******

TASK [etcd : Check_certs | Set 'kubernetes_host_requires_sync' to true if ca or node cert and key don't exist on kubernetes host or checksum doesn't match] ***
ok: [node4]
Monday 05 September 2022  15:44:55 +0000 (0:00:00.048)       0:03:32.589 ******

TASK [etcd : Check_certs | Set 'sync_certs' to true] ******************************************************************
ok: [node4]
Monday 05 September 2022  15:44:55 +0000 (0:00:00.041)       0:03:32.631 ******
included: /kubespray/roles/etcd/tasks/gen_certs_script.yml for node4
Monday 05 September 2022  15:44:55 +0000 (0:00:00.045)       0:03:32.676 ******

TASK [etcd : Gen_certs | create etcd cert dir] ************************************************************************
changed: [node4]
Monday 05 September 2022  15:44:55 +0000 (0:00:00.228)       0:03:32.905 ******
Monday 05 September 2022  15:44:55 +0000 (0:00:00.029)       0:03:32.935 ******
Monday 05 September 2022  15:44:55 +0000 (0:00:00.027)       0:03:32.962 ******
Monday 05 September 2022  15:44:55 +0000 (0:00:00.038)       0:03:33.001 ******

TASK [etcd : Gen_certs | run cert generation script] ******************************************************************
changed: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:44:56 +0000 (0:00:00.769)       0:03:33.771 ******
Monday 05 September 2022  15:44:56 +0000 (0:00:00.082)       0:03:33.853 ******
Monday 05 September 2022  15:44:56 +0000 (0:00:00.107)       0:03:33.961 ******
Monday 05 September 2022  15:44:56 +0000 (0:00:00.096)       0:03:34.057 ******
Monday 05 September 2022  15:44:56 +0000 (0:00:00.093)       0:03:34.150 ******

TASK [etcd : Gen_certs | Set cert names per node] *********************************************************************
ok: [node4]
Monday 05 September 2022  15:44:56 +0000 (0:00:00.033)       0:03:34.184 ******

TASK [etcd : Check_certs | Set 'sync_certs' to true on nodes] *********************************************************
ok: [node4] => (item=ca.pem)
ok: [node4] => (item=node-node4.pem)
ok: [node4] => (item=node-node4-key.pem)
Monday 05 September 2022  15:44:56 +0000 (0:00:00.082)       0:03:34.267 ******

TASK [etcd : Gen_certs | Gather node certs] ***************************************************************************
changed: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:44:57 +0000 (0:00:00.243)       0:03:34.510 ******

TASK [etcd : Gen_certs | Copy certs on nodes] *************************************************************************
ok: [node4]
Monday 05 September 2022  15:44:57 +0000 (0:00:00.204)       0:03:34.714 ******

TASK [etcd : Gen_certs | check certificate permissions] ***************************************************************
changed: [node4]
Monday 05 September 2022  15:44:57 +0000 (0:00:00.201)       0:03:34.916 ******
included: /kubespray/roles/etcd/tasks/upd_ca_trust.yml for node4
Monday 05 September 2022  15:44:57 +0000 (0:00:00.047)       0:03:34.963 ******

TASK [etcd : Gen_certs | target ca-certificate store file] ************************************************************
ok: [node4]
Monday 05 September 2022  15:44:57 +0000 (0:00:00.049)       0:03:35.013 ******

TASK [etcd : Gen_certs | add CA to trusted CA dir] ********************************************************************
changed: [node4]
Monday 05 September 2022  15:44:57 +0000 (0:00:00.276)       0:03:35.289 ******
Monday 05 September 2022  15:44:57 +0000 (0:00:00.026)       0:03:35.316 ******

TASK [etcd : Gen_certs | update ca-certificates (RedHat)] *************************************************************
changed: [node4]
Monday 05 September 2022  15:44:58 +0000 (0:00:00.387)       0:03:35.703 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.024)       0:03:35.728 ******

TASK [etcd : Gen_certs | Get etcd certificate serials] ****************************************************************
ok: [node4]
Monday 05 September 2022  15:44:58 +0000 (0:00:00.208)       0:03:35.936 ******

TASK [etcd : Set etcd_client_cert_serial] *****************************************************************************
ok: [node4]
Monday 05 September 2022  15:44:58 +0000 (0:00:00.038)       0:03:35.975 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.033)       0:03:36.009 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.028)       0:03:36.037 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.025)       0:03:36.063 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.026)       0:03:36.090 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.027)       0:03:36.117 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.027)       0:03:36.145 ******

RUNNING HANDLER [etcd : set etcd_secret_changed] **********************************************************************
ok: [node4]

PLAY [Target only workers to get kubelet installed and checking in on any new nodes(node)] ****************************
Monday 05 September 2022  15:44:58 +0000 (0:00:00.068)       0:03:36.213 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.024)       0:03:36.237 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.022)       0:03:36.260 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.023)       0:03:36.284 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.023)       0:03:36.307 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.023)       0:03:36.331 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.022)       0:03:36.353 ******
Monday 05 September 2022  15:44:58 +0000 (0:00:00.026)       0:03:36.380 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.026)       0:03:36.407 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.023)       0:03:36.431 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.022)       0:03:36.453 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.023)       0:03:36.476 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.022)       0:03:36.499 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.025)       0:03:36.525 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.023)       0:03:36.548 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.024)       0:03:36.572 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.022)       0:03:36.595 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.022)       0:03:36.617 ******
Monday 05 September 2022  15:44:59 +0000 (0:00:00.023)       0:03:36.641 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:01.000)       0:03:37.641 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node4] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  15:45:00 +0000 (0:00:00.030)       0:03:37.671 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.064)       0:03:37.736 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.025)       0:03:37.762 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.024)       0:03:37.787 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.024)       0:03:37.812 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.028)       0:03:37.841 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.022)       0:03:37.863 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.021)       0:03:37.884 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.021)       0:03:37.906 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.024)       0:03:37.931 ******

TASK [kubernetes/node : set kubelet_cgroup_driver_detected fact for containerd] ***************************************
ok: [node4]
Monday 05 September 2022  15:45:00 +0000 (0:00:00.031)       0:03:37.962 ******

TASK [kubernetes/node : set kubelet_cgroup_driver] ********************************************************************
ok: [node4]
Monday 05 September 2022  15:45:00 +0000 (0:00:00.032)       0:03:37.994 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.021)       0:03:38.016 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.026)       0:03:38.042 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.024)       0:03:38.066 ******

TASK [kubernetes/node : Pre-upgrade | check if kubelet container exists] **********************************************
ok: [node4]
Monday 05 September 2022  15:45:00 +0000 (0:00:00.198)       0:03:38.265 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.022)       0:03:38.288 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.022)       0:03:38.311 ******
Monday 05 September 2022  15:45:00 +0000 (0:00:00.024)       0:03:38.335 ******

TASK [kubernetes/node : Ensure /var/lib/cni exists] *******************************************************************
changed: [node4]
Monday 05 September 2022  15:45:01 +0000 (0:00:00.198)       0:03:38.534 ******

TASK [kubernetes/node : install | Copy kubeadm binary from download dir] **********************************************
changed: [node4]
Monday 05 September 2022  15:45:01 +0000 (0:00:00.399)       0:03:38.933 ******

TASK [kubernetes/node : install | Copy kubelet binary from download dir] **********************************************
changed: [node4]
Monday 05 September 2022  15:45:02 +0000 (0:00:00.597)       0:03:39.531 ******
Monday 05 September 2022  15:45:02 +0000 (0:00:00.020)       0:03:39.551 ******
Monday 05 September 2022  15:45:02 +0000 (0:00:00.021)       0:03:39.573 ******
Monday 05 September 2022  15:45:02 +0000 (0:00:00.020)       0:03:39.594 ******

TASK [kubernetes/node : haproxy | Cleanup potentially deployed haproxy] ***********************************************
ok: [node4]
Monday 05 September 2022  15:45:02 +0000 (0:00:00.194)       0:03:39.788 ******

TASK [kubernetes/node : nginx-proxy | Make nginx directory] ***********************************************************
changed: [node4]
Monday 05 September 2022  15:45:02 +0000 (0:00:00.220)       0:03:40.009 ******

TASK [kubernetes/node : nginx-proxy | Write nginx-proxy configuration] ************************************************
changed: [node4]
Monday 05 September 2022  15:45:03 +0000 (0:00:00.491)       0:03:40.500 ******

TASK [kubernetes/node : nginx-proxy | Get checksum from config] *******************************************************
ok: [node4]
Monday 05 September 2022  15:45:03 +0000 (0:00:00.192)       0:03:40.693 ******

TASK [kubernetes/node : nginx-proxy | Write static pod] ***************************************************************
changed: [node4]
Monday 05 September 2022  15:45:03 +0000 (0:00:00.497)       0:03:41.191 ******
Monday 05 September 2022  15:45:03 +0000 (0:00:00.026)       0:03:41.218 ******
Monday 05 September 2022  15:45:03 +0000 (0:00:00.028)       0:03:41.246 ******
Monday 05 September 2022  15:45:03 +0000 (0:00:00.028)       0:03:41.275 ******
Monday 05 September 2022  15:45:03 +0000 (0:00:00.028)       0:03:41.303 ******
Monday 05 September 2022  15:45:03 +0000 (0:00:00.027)       0:03:41.330 ******

TASK [kubernetes/node : Ensure nodePort range is reserved] ************************************************************
ok: [node4]
Monday 05 September 2022  15:45:04 +0000 (0:00:00.188)       0:03:41.519 ******

TASK [kubernetes/node : Verify if br_netfilter module exists] *********************************************************
ok: [node4]
Monday 05 September 2022  15:45:04 +0000 (0:00:00.188)       0:03:41.708 ******

TASK [kubernetes/node : Verify br_netfilter module path exists] *******************************************************
ok: [node4]
Monday 05 September 2022  15:45:04 +0000 (0:00:00.191)       0:03:41.900 ******

TASK [kubernetes/node : Enable br_netfilter module] *******************************************************************
ok: [node4]
Monday 05 September 2022  15:45:04 +0000 (0:00:00.195)       0:03:42.095 ******

TASK [kubernetes/node : Persist br_netfilter module] ******************************************************************
changed: [node4]
Monday 05 September 2022  15:45:05 +0000 (0:00:00.485)       0:03:42.580 ******

TASK [kubernetes/node : Check if bridge-nf-call-iptables key exists] **************************************************
ok: [node4]
Monday 05 September 2022  15:45:05 +0000 (0:00:00.179)       0:03:42.759 ******

TASK [kubernetes/node : Enable bridge-nf-call tables] *****************************************************************
ok: [node4] => (item=net.bridge.bridge-nf-call-iptables)
ok: [node4] => (item=net.bridge.bridge-nf-call-arptables)
ok: [node4] => (item=net.bridge.bridge-nf-call-ip6tables)
Monday 05 September 2022  15:45:05 +0000 (0:00:00.527)       0:03:43.287 ******

TASK [kubernetes/node : Modprobe Kernel Module for IPVS] **************************************************************
ok: [node4] => (item=ip_vs)
ok: [node4] => (item=ip_vs_rr)
ok: [node4] => (item=ip_vs_wrr)
ok: [node4] => (item=ip_vs_sh)
Monday 05 September 2022  15:45:06 +0000 (0:00:00.675)       0:03:43.963 ******

TASK [kubernetes/node : Modprobe nf_conntrack_ipv4] *******************************************************************
fatal: [node4]: FAILED! => {"changed": false, "msg": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64\n", "name": "nf_conntrack_ipv4", "params": "", "rc": 1, "state": "present", "stderr": "modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64\n", "stderr_lines": ["modprobe: FATAL: Module nf_conntrack_ipv4 not found in directory /lib/modules/4.18.0-372.19.1.el8_6.x86_64"], "stdout": "", "stdout_lines": []}
...ignoring
Monday 05 September 2022  15:45:06 +0000 (0:00:00.177)       0:03:44.141 ******

TASK [kubernetes/node : Persist ip_vs modules] ************************************************************************
changed: [node4]
Monday 05 September 2022  15:45:07 +0000 (0:00:00.498)       0:03:44.639 ******
Monday 05 September 2022  15:45:07 +0000 (0:00:00.021)       0:03:44.660 ******
Monday 05 September 2022  15:45:07 +0000 (0:00:00.022)       0:03:44.683 ******
Monday 05 September 2022  15:45:07 +0000 (0:00:00.021)       0:03:44.704 ******
Monday 05 September 2022  15:45:07 +0000 (0:00:00.020)       0:03:44.725 ******
Monday 05 September 2022  15:45:07 +0000 (0:00:00.020)       0:03:44.746 ******

TASK [kubernetes/node : Set kubelet api version to v1beta1] ***********************************************************
ok: [node4]
Monday 05 September 2022  15:45:07 +0000 (0:00:00.028)       0:03:44.775 ******

TASK [kubernetes/node : Write kubelet environment config file (kubeadm)] **********************************************
changed: [node4]
Monday 05 September 2022  15:45:07 +0000 (0:00:00.507)       0:03:45.283 ******

TASK [kubernetes/node : Write kubelet config file] ********************************************************************
changed: [node4]
Monday 05 September 2022  15:45:08 +0000 (0:00:00.532)       0:03:45.816 ******

TASK [kubernetes/node : Write kubelet systemd init file] **************************************************************
changed: [node4]
Monday 05 September 2022  15:45:08 +0000 (0:00:00.496)       0:03:46.312 ******

RUNNING HANDLER [kubernetes/node : Node | restart kubelet] ************************************************************
changed: [node4]
Monday 05 September 2022  15:45:09 +0000 (0:00:00.181)       0:03:46.493 ******

RUNNING HANDLER [kubernetes/node : Kubelet | reload systemd] **********************************************************
ok: [node4]
Monday 05 September 2022  15:45:09 +0000 (0:00:00.391)       0:03:46.885 ******

RUNNING HANDLER [kubernetes/node : Kubelet | restart kubelet] *********************************************************
changed: [node4]
Monday 05 September 2022  15:45:09 +0000 (0:00:00.322)       0:03:47.207 ******

TASK [kubernetes/node : Enable kubelet] *******************************************************************************
ok: [node4]
[WARNING]: Could not match supplied host pattern, ignoring: |
[WARNING]: Could not match supplied host pattern, ignoring: first

PLAY [Upload control plane certs and retrieve encryption key] *********************************************************
skipping: no hosts matched

PLAY [Target only workers to get kubelet installed and checking in on any new nodes(network)] *************************
Monday 05 September 2022  15:45:10 +0000 (0:00:00.414)       0:03:47.622 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.028)       0:03:47.650 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.023)       0:03:47.674 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.024)       0:03:47.698 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.024)       0:03:47.723 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.024)       0:03:47.748 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.022)       0:03:47.770 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.028)       0:03:47.798 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.027)       0:03:47.826 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.023)       0:03:47.849 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.022)       0:03:47.872 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.030)       0:03:47.902 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.023)       0:03:47.926 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.026)       0:03:47.952 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.022)       0:03:47.975 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.022)       0:03:47.997 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.022)       0:03:48.020 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.024)       0:03:48.044 ******
Monday 05 September 2022  15:45:10 +0000 (0:00:00.022)       0:03:48.067 ******
Monday 05 September 2022  15:45:11 +0000 (0:00:01.025)       0:03:49.092 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node4] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  15:45:11 +0000 (0:00:00.042)       0:03:49.134 ******
Monday 05 September 2022  15:45:11 +0000 (0:00:00.080)       0:03:49.215 ******
Monday 05 September 2022  15:45:11 +0000 (0:00:00.025)       0:03:49.240 ******
Monday 05 September 2022  15:45:11 +0000 (0:00:00.025)       0:03:49.266 ******
Monday 05 September 2022  15:45:11 +0000 (0:00:00.025)       0:03:49.291 ******
Monday 05 September 2022  15:45:11 +0000 (0:00:00.023)       0:03:49.315 ******

TASK [kubernetes/kubeadm : Set kubeadm_discovery_address] *************************************************************
ok: [node4]
Monday 05 September 2022  15:45:11 +0000 (0:00:00.056)       0:03:49.371 ******

TASK [kubernetes/kubeadm : Check if kubelet.conf exists] **************************************************************
ok: [node4]
Monday 05 September 2022  15:45:12 +0000 (0:00:00.195)       0:03:49.567 ******

TASK [kubernetes/kubeadm : Check if kubeadm CA cert is accessible] ****************************************************
ok: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:45:12 +0000 (0:00:00.206)       0:03:49.773 ******

TASK [kubernetes/kubeadm : Calculate kubeadm CA cert hash] ************************************************************
ok: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:45:12 +0000 (0:00:00.246)       0:03:50.020 ******

TASK [kubernetes/kubeadm : Create kubeadm token for joining nodes with 24h expiration (default)] **********************
ok: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:45:12 +0000 (0:00:00.234)       0:03:50.254 ******

TASK [kubernetes/kubeadm : Set kubeadm_token to generated token] ******************************************************
ok: [node4]
Monday 05 September 2022  15:45:12 +0000 (0:00:00.032)       0:03:50.286 ******

TASK [kubernetes/kubeadm : Set kubeadm api version to v1beta2] ********************************************************
ok: [node4]
Monday 05 September 2022  15:45:12 +0000 (0:00:00.029)       0:03:50.316 ******

TASK [kubernetes/kubeadm : Create kubeadm client config] **************************************************************
changed: [node4]
Monday 05 September 2022  15:45:13 +0000 (0:00:00.488)       0:03:50.804 ******

TASK [kubernetes/kubeadm : Join to cluster] ***************************************************************************
changed: [node4]
Monday 05 September 2022  15:45:19 +0000 (0:00:06.116)       0:03:56.921 ******
Monday 05 September 2022  15:45:19 +0000 (0:00:00.029)       0:03:56.951 ******

TASK [kubernetes/kubeadm : Update server field in kubelet kubeconfig] *************************************************
changed: [node4]
Monday 05 September 2022  15:45:19 +0000 (0:00:00.278)       0:03:57.230 ******

TASK [kubernetes/kubeadm : Update server field in kube-proxy kubeconfig] **********************************************
changed: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:45:20 +0000 (0:00:00.405)       0:03:57.635 ******

TASK [kubernetes/kubeadm : Set ca.crt file permission] ****************************************************************
ok: [node4]
Monday 05 September 2022  15:45:20 +0000 (0:00:00.198)       0:03:57.834 ******

TASK [kubernetes/kubeadm : Restart all kube-proxy pods to ensure that they load the new configmap] ********************
changed: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:45:20 +0000 (0:00:00.539)       0:03:58.373 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.023)       0:03:58.397 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.024)       0:03:58.422 ******

TASK [kubernetes/node-label : Set role node label to empty list] ******************************************************
ok: [node4]
Monday 05 September 2022  15:45:21 +0000 (0:00:00.037)       0:03:58.460 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.030)       0:03:58.490 ******

TASK [kubernetes/node-label : Set inventory node label to empty list] *************************************************
ok: [node4]
Monday 05 September 2022  15:45:21 +0000 (0:00:00.032)       0:03:58.523 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.027)       0:03:58.551 ******

TASK [kubernetes/node-label : debug] **********************************************************************************
ok: [node4] => {
    "role_node_labels": []
}
Monday 05 September 2022  15:45:21 +0000 (0:00:00.044)       0:03:58.595 ******

TASK [kubernetes/node-label : debug] **********************************************************************************
ok: [node4] => {
    "inventory_node_labels": []
}
Monday 05 September 2022  15:45:21 +0000 (0:00:00.050)       0:03:58.645 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.024)       0:03:58.669 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.038)       0:03:58.708 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.040)       0:03:58.748 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.034)       0:03:58.783 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.036)       0:03:58.820 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.026)       0:03:58.846 ******

TASK [network_plugin/calico : Check vars defined correctly] ***********************************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:45:21 +0000 (0:00:00.049)       0:03:58.896 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.037)       0:03:58.933 ******

TASK [network_plugin/calico : Check ipip and vxlan mode defined correctly] ********************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:45:21 +0000 (0:00:00.059)       0:03:58.993 ******

TASK [network_plugin/calico : Check ipip and vxlan mode if simultaneously enabled] ************************************
ok: [node4] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:45:21 +0000 (0:00:00.059)       0:03:59.053 ******
Monday 05 September 2022  15:45:21 +0000 (0:00:00.042)       0:03:59.096 ******

TASK [network_plugin/calico : Get Calico default-pool configuration] **************************************************
ok: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:45:21 +0000 (0:00:00.281)       0:03:59.377 ******
Monday 05 September 2022  15:45:22 +0000 (0:00:00.028)       0:03:59.405 ******
Monday 05 September 2022  15:45:22 +0000 (0:00:00.029)       0:03:59.435 ******

TASK [network_plugin/calico : Slurp CNI config] ***********************************************************************
ok: [node4]
Monday 05 September 2022  15:45:22 +0000 (0:00:00.237)       0:03:59.673 ******
Monday 05 September 2022  15:45:22 +0000 (0:00:00.026)       0:03:59.700 ******
Monday 05 September 2022  15:45:22 +0000 (0:00:00.035)       0:03:59.735 ******
Monday 05 September 2022  15:45:22 +0000 (0:00:00.025)       0:03:59.760 ******

TASK [network_plugin/calico : Calico | Gather os specific variables] **************************************************
ok: [node4] => (item=/kubespray/roles/network_plugin/calico/vars/../vars/redhat.yml)
Monday 05 September 2022  15:45:22 +0000 (0:00:00.047)       0:03:59.808 ******
Monday 05 September 2022  15:45:22 +0000 (0:00:00.029)       0:03:59.837 ******
included: /kubespray/roles/network_plugin/calico/tasks/install.yml for node4
Monday 05 September 2022  15:45:22 +0000 (0:00:00.056)       0:03:59.893 ******
Monday 05 September 2022  15:45:22 +0000 (0:00:00.045)       0:03:59.939 ******

TASK [network_plugin/calico : Calico | Copy calicoctl binary from download dir] ***************************************
changed: [node4]
Monday 05 September 2022  15:45:23 +0000 (0:00:00.504)       0:04:00.443 ******

TASK [network_plugin/calico : Calico | Write Calico cni config] *******************************************************
changed: [node4]
Monday 05 September 2022  15:45:23 +0000 (0:00:00.649)       0:04:01.093 ******
Monday 05 September 2022  15:45:23 +0000 (0:00:00.027)       0:04:01.121 ******
Monday 05 September 2022  15:45:23 +0000 (0:00:00.071)       0:04:01.192 ******
Monday 05 September 2022  15:45:23 +0000 (0:00:00.026)       0:04:01.218 ******

TASK [network_plugin/calico : Calico | Install calicoctl wrapper script] **********************************************
changed: [node4]
Monday 05 September 2022  15:45:24 +0000 (0:00:00.555)       0:04:01.774 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.030)       0:04:01.804 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.029)       0:04:01.834 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.030)       0:04:01.865 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.027)       0:04:01.892 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.026)       0:04:01.919 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.027)       0:04:01.947 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.028)       0:04:01.975 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.029)       0:04:02.004 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.025)       0:04:02.030 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.029)       0:04:02.060 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.019)       0:04:02.079 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.018)       0:04:02.098 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.029)       0:04:02.128 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.032)       0:04:02.161 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.027)       0:04:02.188 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.018)       0:04:02.207 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.019)       0:04:02.226 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.079)       0:04:02.306 ******
Monday 05 September 2022  15:45:24 +0000 (0:00:00.030)       0:04:02.336 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.094)       0:04:02.431 ******

TASK [network_plugin/calico : Wait for calico kubeconfig to be created] ***********************************************
ok: [node4]
Monday 05 September 2022  15:45:25 +0000 (0:00:00.399)       0:04:02.831 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.029)       0:04:02.860 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.035)       0:04:02.896 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.029)       0:04:02.925 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.024)       0:04:02.949 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.027)       0:04:02.977 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.023)       0:04:03.000 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.047)       0:04:03.047 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.027)       0:04:03.074 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.024)       0:04:03.099 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.022)       0:04:03.122 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.025)       0:04:03.147 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.030)       0:04:03.178 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.030)       0:04:03.209 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.069)       0:04:03.278 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.024)       0:04:03.303 ******
Monday 05 September 2022  15:45:25 +0000 (0:00:00.085)       0:04:03.388 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.023)       0:04:03.412 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.023)       0:04:03.436 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.029)       0:04:03.465 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.022)       0:04:03.488 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.025)       0:04:03.513 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.029)       0:04:03.543 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.024)       0:04:03.567 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.022)       0:04:03.589 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.032)       0:04:03.622 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.028)       0:04:03.651 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.025)       0:04:03.676 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.051)       0:04:03.728 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.022)       0:04:03.750 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.050)       0:04:03.800 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.026)       0:04:03.826 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.036)       0:04:03.863 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.044)       0:04:03.907 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.022)       0:04:03.930 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.025)       0:04:03.956 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.024)       0:04:03.980 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.022)       0:04:04.003 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.048)       0:04:04.051 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.024)       0:04:04.075 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.026)       0:04:04.102 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.023)       0:04:04.125 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.019)       0:04:04.145 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.021)       0:04:04.167 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.024)       0:04:04.191 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.025)       0:04:04.217 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.029)       0:04:04.247 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.027)       0:04:04.274 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.023)       0:04:04.298 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.023)       0:04:04.321 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.034)       0:04:04.356 ******
Monday 05 September 2022  15:45:26 +0000 (0:00:00.028)       0:04:04.384 ******
Monday 05 September 2022  15:45:27 +0000 (0:00:00.039)       0:04:04.423 ******
Monday 05 September 2022  15:45:27 +0000 (0:00:00.035)       0:04:04.459 ******
Monday 05 September 2022  15:45:27 +0000 (0:00:00.054)       0:04:04.514 ******
Monday 05 September 2022  15:45:27 +0000 (0:00:00.029)       0:04:04.543 ******

RUNNING HANDLER [kubernetes/kubeadm : Kubeadm | restart kubelet] ******************************************************
changed: [node4]
Monday 05 September 2022  15:45:27 +0000 (0:00:00.236)       0:04:04.779 ******

RUNNING HANDLER [kubernetes/kubeadm : Kubeadm | reload systemd] *******************************************************
ok: [node4]
Monday 05 September 2022  15:45:27 +0000 (0:00:00.505)       0:04:05.285 ******

RUNNING HANDLER [kubernetes/kubeadm : Kubeadm | reload kubelet] *******************************************************
changed: [node4]
Monday 05 September 2022  15:45:28 +0000 (0:00:00.384)       0:04:05.669 ******

PLAY RECAP ************************************************************************************************************
node4                      : ok=406  changed=53   unreachable=0    failed=0    skipped=582  rescued=0    ignored=1

Monday 05 September 2022  15:45:28 +0000 (0:00:00.038)       0:04:05.708 
===============================================================================
bootstrap-os : Enable RHEL 8 repos ---------------------------------------------------------------------------- 66.88s
bootstrap-os : Check RHEL subscription-manager status --------------------------------------------------------- 21.52s
bootstrap-os : Install libselinux python package --------------------------------------------------------------- 7.46s
kubernetes/preinstall : Install packages requirements ---------------------------------------------------------- 7.32s
container-engine/runc : runc | Uninstall runc package managed by package manager ------------------------------- 7.19s
container-engine/containerd : containerd | Remove any package manager controlled containerd package ------------ 7.11s
kubernetes/kubeadm : Join to cluster --------------------------------------------------------------------------- 6.12s
kubernetes/preinstall : Get current calico cluster version ----------------------------------------------------- 3.79s
container-engine/containerd : containerd | Unpack containerd archive ------------------------------------------- 2.60s
container-engine/crictl : extract_file | Unpacking archive ----------------------------------------------------- 2.35s
container-engine/nerdctl : extract_file | Unpacking archive ---------------------------------------------------- 2.05s
container-engine/nerdctl : extract_file | Unpacking archive ---------------------------------------------------- 2.01s
container-engine/nerdctl : download_file | Download item ------------------------------------------------------- 1.89s
container-engine/runc : download_file | Download item ---------------------------------------------------------- 1.84s
container-engine/nerdctl : download_file | Download item ------------------------------------------------------- 1.79s
container-engine/containerd : download_file | Download item ---------------------------------------------------- 1.72s
container-engine/crictl : download_file | Download item -------------------------------------------------------- 1.69s
download : download | Download files / images ------------------------------------------------------------------ 1.64s
download : download | Download files / images ------------------------------------------------------------------ 1.03s
download : extract_file | Unpacking archive -------------------------------------------------------------------- 1.01s


kubectl get nodes
NAME    STATUS   ROLES                  AGE   VERSION
node1   Ready    control-plane,master   98m   v1.22.8
node2   Ready    <none>                 97m   v1.22.8
node3   Ready    <none>                 97m   v1.22.8
node4   Ready    <none>                 28s   v1.22.8
</code></pre>
</details>


<details><summary>
Remove Node</summary>
<pre><code>
(venv) $ ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root -e node=node4 remove-node.yml

PLAY [localhost] ******************************************************************************************************
Monday 05 September 2022  15:37:05 +0000 (0:00:00.030)       0:00:00.030 ******

TASK [Check 2.9.0 <= Ansible version < 2.12.0] ************************************************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:37:05 +0000 (0:00:00.029)       0:00:00.060 ******

TASK [Check Ansible version > 2.10.11 when using ansible 2.10] ********************************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:37:05 +0000 (0:00:00.022)       0:00:00.082 ******

TASK [Check that python netaddr is installed] *************************************************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}
Monday 05 September 2022  15:37:05 +0000 (0:00:00.036)       0:00:00.119 ******

TASK [Check that jinja is not too old (install via pip)] **************************************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}
[WARNING]: Could not match supplied host pattern, ignoring: kube-master

PLAY [Add kube-master nodes to kube_control_plane] ********************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: kube-node

PLAY [Add kube-node nodes to kube_node] *******************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: k8s-cluster

PLAY [Add k8s-cluster nodes to k8s_cluster] ***************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: calico-rr

PLAY [Add calico-rr nodes to calico_rr] *******************************************************************************
skipping: no hosts matched
[WARNING]: Could not match supplied host pattern, ignoring: no-floating

PLAY [Add no-floating nodes to no_floating] ***************************************************************************
skipping: no hosts matched

PLAY [node4] **********************************************************************************************************
Monday 05 September 2022  15:37:05 +0000 (0:00:00.045)       0:00:00.164 ******
[Confirm Execution]
Are you sure you want to delete nodes state? Type 'yes' to delete nodes.:

TASK [Confirm Execution] **********************************************************************************************
ok: [node4]
Monday 05 September 2022  15:37:10 +0000 (0:00:04.573)       0:00:04.738 ******

PLAY [Gather facts] ***************************************************************************************************
Monday 05 September 2022  15:37:10 +0000 (0:00:00.028)       0:00:04.767 ******

TASK [Gather minimal facts] *******************************************************************************************
ok: [node4]
ok: [node1]
ok: [node3]
ok: [node2]
Monday 05 September 2022  15:37:11 +0000 (0:00:00.898)       0:00:05.665 ******

TASK [Gather necessary facts (network)] *******************************************************************************
ok: [node4]
ok: [node2]
ok: [node3]
ok: [node1]
Monday 05 September 2022  15:37:11 +0000 (0:00:00.493)       0:00:06.159 ******

TASK [Gather necessary facts (hardware)] ******************************************************************************
ok: [node2]
ok: [node4]
ok: [node3]
ok: [node1]

PLAY [node4] **********************************************************************************************************
Monday 05 September 2022  15:37:12 +0000 (0:00:00.560)       0:00:06.719 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.021)       0:00:06.740 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.022)       0:00:06.762 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.020)       0:00:06.783 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.020)       0:00:06.803 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.021)       0:00:06.824 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.021)       0:00:06.846 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.024)       0:00:06.870 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.025)       0:00:06.896 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.023)       0:00:06.919 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.027)       0:00:06.947 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.034)       0:00:06.981 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.026)       0:00:07.008 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.029)       0:00:07.037 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.021)       0:00:07.059 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.020)       0:00:07.079 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.020)       0:00:07.100 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.020)       0:00:07.120 ******
Monday 05 September 2022  15:37:12 +0000 (0:00:00.020)       0:00:07.140 ******
Monday 05 September 2022  15:37:13 +0000 (0:00:00.987)       0:00:08.127 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node4] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  15:37:13 +0000 (0:00:00.030)       0:00:08.158 ******
Monday 05 September 2022  15:37:13 +0000 (0:00:00.136)       0:00:08.295 ******

TASK [kubespray-defaults : create fallback_ips_base] ******************************************************************
ok: [node4]
Monday 05 September 2022  15:37:13 +0000 (0:00:00.050)       0:00:08.345 ******

TASK [kubespray-defaults : set fallback_ips] **************************************************************************
ok: [node4]
Monday 05 September 2022  15:37:13 +0000 (0:00:00.036)       0:00:08.382 ******
Monday 05 September 2022  15:37:13 +0000 (0:00:00.022)       0:00:08.405 ******
Monday 05 September 2022  15:37:13 +0000 (0:00:00.026)       0:00:08.431 ******

TASK [remove-node/pre-remove : remove-node | List nodes] **************************************************************
ok: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:37:14 +0000 (0:00:00.437)       0:00:08.868 ******

TASK [remove-node/pre-remove : remove-node | Drain node except daemonsets resource] ***********************************
changed: [node4 -> 192.168.88.211]
Monday 05 September 2022  15:37:14 +0000 (0:00:00.269)       0:00:09.138 ******
Monday 05 September 2022  15:37:14 +0000 (0:00:00.019)       0:00:09.157 ******
Monday 05 September 2022  15:37:14 +0000 (0:00:00.021)       0:00:09.178 ******
Monday 05 September 2022  15:37:14 +0000 (0:00:00.027)       0:00:09.206 ******
Monday 05 September 2022  15:37:14 +0000 (0:00:00.024)       0:00:09.231 ******
Monday 05 September 2022  15:37:14 +0000 (0:00:00.021)       0:00:09.252 ******

TASK [reset : reset | stop services] **********************************************************************************
ok: [node4] => (item=kubelet)
Monday 05 September 2022  15:37:15 +0000 (0:00:00.454)       0:00:09.706 ******

TASK [reset : reset | remove services] ********************************************************************************
ok: [node4] => (item=kubelet.service)
ok: [node4] => (item=calico-node.service)
ok: [node4] => (item=containerd.service.d/http-proxy.conf)
ok: [node4] => (item=crio.service.d/http-proxy.conf)
ok: [node4] => (item=k8s-certs-renew.service)
ok: [node4] => (item=k8s-certs-renew.timer)
Monday 05 September 2022  15:37:16 +0000 (0:00:01.153)       0:00:10.860 ******

TASK [reset : reset | remove docker dropins] **************************************************************************
ok: [node4] => (item=docker-dns.conf)
ok: [node4] => (item=docker-options.conf)
ok: [node4] => (item=http-proxy.conf)
ok: [node4] => (item=docker-orphan-cleanup.conf)
Monday 05 September 2022  15:37:17 +0000 (0:00:00.703)       0:00:11.564 ******
Monday 05 September 2022  15:37:17 +0000 (0:00:00.021)       0:00:11.586 ******
Monday 05 September 2022  15:37:17 +0000 (0:00:00.027)       0:00:11.613 ******
Monday 05 September 2022  15:37:17 +0000 (0:00:00.021)       0:00:11.634 ******

TASK [reset : reset | check if crictl is present] *********************************************************************
ok: [node4]
Monday 05 September 2022  15:37:17 +0000 (0:00:00.291)       0:00:11.926 ******
Monday 05 September 2022  15:37:17 +0000 (0:00:00.022)       0:00:11.948 ******
Monday 05 September 2022  15:37:17 +0000 (0:00:00.024)       0:00:11.973 ******
Monday 05 September 2022  15:37:17 +0000 (0:00:00.022)       0:00:11.996 ******
Monday 05 September 2022  15:37:17 +0000 (0:00:00.026)       0:00:12.022 ******
Monday 05 September 2022  15:37:17 +0000 (0:00:00.019)       0:00:12.042 ******
Monday 05 September 2022  15:37:17 +0000 (0:00:00.020)       0:00:12.063 ******

TASK [reset : reset | stop etcd services] *****************************************************************************
ok: [node4] => (item=etcd)
ok: [node4] => (item=etcd-events)
Monday 05 September 2022  15:37:18 +0000 (0:00:00.529)       0:00:12.593 ******

TASK [reset : reset | remove etcd services] ***************************************************************************
ok: [node4] => (item=etcd)
ok: [node4] => (item=etcd-events)
Monday 05 September 2022  15:37:18 +0000 (0:00:00.363)       0:00:12.956 ******

TASK [reset : reset | stop containerd service] ************************************************************************
ok: [node4]
Monday 05 September 2022  15:37:18 +0000 (0:00:00.300)       0:00:13.257 ******

TASK [reset : reset | remove containerd service] **********************************************************************
ok: [node4]
Monday 05 September 2022  15:37:19 +0000 (0:00:00.182)       0:00:13.440 ******

TASK [reset : reset | gather mounted kubelet dirs] ********************************************************************
changed: [node4]
Monday 05 September 2022  15:37:19 +0000 (0:00:00.178)       0:00:13.618 ******
Monday 05 September 2022  15:37:19 +0000 (0:00:00.021)       0:00:13.640 ******

TASK [reset : flush iptables] *****************************************************************************************
changed: [node4] => (item=filter)
changed: [node4] => (item=nat)
changed: [node4] => (item=mangle)
changed: [node4] => (item=raw)
Monday 05 September 2022  15:37:20 +0000 (0:00:00.821)       0:00:14.461 ******

TASK [reset : Clear IPVS virtual server table] ************************************************************************
changed: [node4]
Monday 05 September 2022  15:37:20 +0000 (0:00:00.179)       0:00:14.641 ******

TASK [reset : reset | check kube-ipvs0 network device] ****************************************************************
ok: [node4]
Monday 05 September 2022  15:37:20 +0000 (0:00:00.187)       0:00:14.829 ******
Monday 05 September 2022  15:37:20 +0000 (0:00:00.024)       0:00:14.853 ******

TASK [reset : reset | check nodelocaldns network device] **************************************************************
ok: [node4]
Monday 05 September 2022  15:37:20 +0000 (0:00:00.179)       0:00:15.033 ******
Monday 05 September 2022  15:37:20 +0000 (0:00:00.025)       0:00:15.058 ******

TASK [reset : reset | delete some files and directories] **************************************************************
ok: [node4] => (item=/etc/kubernetes)
ok: [node4] => (item=/var/lib/kubelet)
ok: [node4] => (item=/root/.kube)
ok: [node4] => (item=/root/.helm)
ok: [node4] => (item=/var/lib/etcd)
ok: [node4] => (item=/var/lib/etcd-events)
ok: [node4] => (item=/etc/ssl/etcd)
ok: [node4] => (item=/var/log/calico)
ok: [node4] => (item=/etc/cni)
ok: [node4] => (item=/etc/nginx)
ok: [node4] => (item=/etc/dnsmasq.d)
ok: [node4] => (item=/etc/dnsmasq.conf)
ok: [node4] => (item=/etc/dnsmasq.d-available)
ok: [node4] => (item=/etc/etcd.env)
ok: [node4] => (item=/etc/calico)
ok: [node4] => (item=/etc/NetworkManager/conf.d/calico.conf)
ok: [node4] => (item=/etc/NetworkManager/conf.d/k8s.conf)
ok: [node4] => (item=/etc/weave.env)
ok: [node4] => (item=/opt/cni)
ok: [node4] => (item=/etc/dhcp/dhclient.d/zdnsupdate.sh)
ok: [node4] => (item=/etc/dhcp/dhclient-exit-hooks.d/zdnsupdate)
ok: [node4] => (item=/run/flannel)
ok: [node4] => (item=/etc/flannel)
ok: [node4] => (item=/run/kubernetes)
ok: [node4] => (item=/usr/local/share/ca-certificates/etcd-ca.crt)
ok: [node4] => (item=/usr/local/share/ca-certificates/kube-ca.crt)
ok: [node4] => (item=/etc/ssl/certs/etcd-ca.pem)
ok: [node4] => (item=/etc/ssl/certs/kube-ca.pem)
ok: [node4] => (item=/etc/pki/ca-trust/source/anchors/etcd-ca.crt)
ok: [node4] => (item=/etc/pki/ca-trust/source/anchors/kube-ca.crt)
ok: [node4] => (item=/var/log/pods/)
ok: [node4] => (item=/usr/local/bin/kubelet)
ok: [node4] => (item=/usr/local/bin/etcd-scripts)
ok: [node4] => (item=/usr/local/bin/etcd)
ok: [node4] => (item=/usr/local/bin/etcd-events)
ok: [node4] => (item=/usr/local/bin/etcdctl)
ok: [node4] => (item=/usr/local/bin/etcdctl.sh)
ok: [node4] => (item=/usr/local/bin/kubernetes-scripts)
ok: [node4] => (item=/usr/local/bin/kubectl)
ok: [node4] => (item=/usr/local/bin/kubeadm)
ok: [node4] => (item=/usr/local/bin/helm)
ok: [node4] => (item=/usr/local/bin/calicoctl)
ok: [node4] => (item=/usr/local/bin/calicoctl.sh)
ok: [node4] => (item=/usr/local/bin/calico-upgrade)
ok: [node4] => (item=/usr/local/bin/weave)
ok: [node4] => (item=/usr/local/bin/crictl)
ok: [node4] => (item=/usr/local/bin/nerdctl)
ok: [node4] => (item=/usr/local/bin/netctl)
ok: [node4] => (item=/usr/local/bin/k8s-certs-renew.sh)
ok: [node4] => (item=/var/lib/cni)
ok: [node4] => (item=/etc/openvswitch)
ok: [node4] => (item=/run/openvswitch)
ok: [node4] => (item=/var/lib/kube-router)
ok: [node4] => (item=/var/lib/calico)
ok: [node4] => (item=/etc/cilium)
ok: [node4] => (item=/run/calico)
ok: [node4] => (item=/etc/bash_completion.d/kubectl.sh)
ok: [node4] => (item=/etc/bash_completion.d/crictl)
ok: [node4] => (item=/etc/bash_completion.d/nerdctl)
ok: [node4] => (item=/etc/bash_completion.d/krew)
ok: [node4] => (item=/etc/bash_completion.d/krew.sh)
ok: [node4] => (item=/usr/local/krew)
ok: [node4] => (item=/etc/modules-load.d/kube_proxy-ipvs.conf)
ok: [node4] => (item=/etc/modules-load.d/kubespray-br_netfilter.conf)
ok: [node4] => (item=/etc/modules-load.d/kubespray-kata-containers.conf)
ok: [node4] => (item=/usr/libexec/kubernetes)
ok: [node4] => (item=/etc/origin/openvswitch)
ok: [node4] => (item=/etc/origin/ovn)
Monday 05 September 2022  15:37:32 +0000 (0:00:11.454)       0:00:26.512 ******

TASK [reset : reset | remove containerd binary files] *****************************************************************
ok: [node4] => (item=containerd)
ok: [node4] => (item=containerd-shim)
ok: [node4] => (item=containerd-shim-runc-v1)
ok: [node4] => (item=containerd-shim-runc-v2)
ok: [node4] => (item=containerd-stress)
ok: [node4] => (item=crictl)
ok: [node4] => (item=critest)
ok: [node4] => (item=ctd-decoder)
ok: [node4] => (item=ctr)
ok: [node4] => (item=runc)
Monday 05 September 2022  15:37:33 +0000 (0:00:01.779)       0:00:28.292 ******

TASK [reset : reset | remove dns settings from dhclient.conf] *********************************************************
ok: [node4] => (item=/etc/dhclient.conf)
ok: [node4] => (item=/etc/dhcp/dhclient.conf)
Monday 05 September 2022  15:37:34 +0000 (0:00:00.452)       0:00:28.744 ******

TASK [reset : reset | remove host entries from /etc/hosts] ************************************************************
ok: [node4]
Monday 05 September 2022  15:37:34 +0000 (0:00:00.176)       0:00:28.921 ******
included: /kubespray/roles/network_plugin/calico/tasks/reset.yml for node4
Monday 05 September 2022  15:37:34 +0000 (0:00:00.040)       0:00:28.962 ******

TASK [reset : reset | check dummy0 network device] ********************************************************************
ok: [node4]
Monday 05 September 2022  15:37:34 +0000 (0:00:00.191)       0:00:29.153 ******
Monday 05 September 2022  15:37:34 +0000 (0:00:00.020)       0:00:29.173 ******

TASK [reset : reset | get and remove remaining routes set by bird] ****************************************************
ok: [node4]
Monday 05 September 2022  15:37:34 +0000 (0:00:00.179)       0:00:29.353 ******

TASK [reset : reset | Restart network] ********************************************************************************
changed: [node4]

PLAY [node4] **********************************************************************************************************
Monday 05 September 2022  15:37:35 +0000 (0:00:00.363)       0:00:29.716 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.021)       0:00:29.738 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.022)       0:00:29.761 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.024)       0:00:29.785 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.023)       0:00:29.809 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.021)       0:00:29.830 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.019)       0:00:29.850 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.022)       0:00:29.872 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.022)       0:00:29.895 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.024)       0:00:29.919 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.020)       0:00:29.940 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.019)       0:00:29.959 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.022)       0:00:29.982 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.024)       0:00:30.006 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.022)       0:00:30.029 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.028)       0:00:30.058 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.022)       0:00:30.080 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.021)       0:00:30.102 ******
Monday 05 September 2022  15:37:35 +0000 (0:00:00.022)       0:00:30.125 ******
Monday 05 September 2022  15:37:36 +0000 (0:00:00.978)       0:00:31.103 ******

TASK [kubespray-defaults : Configure defaults] ************************************************************************
ok: [node4] => {
    "msg": "Check roles/kubespray-defaults/defaults/main.yml"
}
Monday 05 September 2022  15:37:36 +0000 (0:00:00.029)       0:00:31.133 ******
Monday 05 September 2022  15:37:36 +0000 (0:00:00.067)       0:00:31.200 ******
Monday 05 September 2022  15:37:36 +0000 (0:00:00.022)       0:00:31.222 ******
Monday 05 September 2022  15:37:36 +0000 (0:00:00.025)       0:00:31.248 ******
Monday 05 September 2022  15:37:36 +0000 (0:00:00.025)       0:00:31.273 ******
Monday 05 September 2022  15:37:36 +0000 (0:00:00.029)       0:00:31.303 ******

TASK [remove-node/post-remove : Delete node] **************************************************************************
changed: [node4 -> 192.168.88.211]

PLAY RECAP ************************************************************************************************************
localhost                  : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
node1                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
node2                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
node3                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
node4                      : ok=32   changed=6    unreachable=0    failed=0    skipped=65   rescued=0    ignored=0

Monday 05 September 2022  15:37:37 +0000 (0:00:00.277)       0:00:31.581 
===============================================================================
reset : reset | delete some files and directories ------------------------------------------------------------- 11.45s
Confirm Execution ---------------------------------------------------------------------------------------------- 4.57s
reset : reset | remove containerd binary files ----------------------------------------------------------------- 1.78s
reset : reset | remove services -------------------------------------------------------------------------------- 1.15s
download : download | Download files / images ------------------------------------------------------------------ 0.99s
download : download | Download files / images ------------------------------------------------------------------ 0.98s
Gather minimal facts ------------------------------------------------------------------------------------------- 0.90s
reset : flush iptables ----------------------------------------------------------------------------------------- 0.82s
reset : reset | remove docker dropins -------------------------------------------------------------------------- 0.70s
Gather necessary facts (hardware) ------------------------------------------------------------------------------ 0.56s
reset : reset | stop etcd services ----------------------------------------------------------------------------- 0.53s
Gather necessary facts (network) ------------------------------------------------------------------------------- 0.49s
reset : reset | stop services ---------------------------------------------------------------------------------- 0.46s
reset : reset | remove dns settings from dhclient.conf --------------------------------------------------------- 0.45s
remove-node/pre-remove : remove-node | List nodes -------------------------------------------------------------- 0.44s
reset : reset | remove etcd services --------------------------------------------------------------------------- 0.36s
reset : reset | Restart network -------------------------------------------------------------------------------- 0.36s
reset : reset | stop containerd service ------------------------------------------------------------------------ 0.30s
reset : reset | check if crictl is present --------------------------------------------------------------------- 0.29s
remove-node/post-remove : Delete node -------------------------------------------------------------------------- 0.28s


kubectl get nodes
NAME    STATUS   ROLES                  AGE   VERSION
node1   Ready    control-plane,master   91m   v1.22.8
node2   Ready    <none>                 90m   v1.22.8
node3   Ready    <none>                 90m   v1.22.8
</code></pre>
</details>
