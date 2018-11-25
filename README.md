# Kubernetes Master Ansible Role
Configures a Kubernetes master node.

SSL is enabled by default and requires certificates to be configured.
See the [SSL Configuration](#ssl-configuration) section for more information.

## Configuration
| Key | Description |
|-----|-------------|
| ``kube_master_taint_node``           | Whether to taint the node to prevent pods being scheduled on the master. Generally the default of ``yes`` should be used unless running a single node cluster. |
| ``kube_master_kubelet_args``         | The arguments to be passed to kubelet. This passed down through to the ``jumperfly.kubernetes_node`` and ``jumperfly.kubelet`` role configuration. |
| ``kube_apiserver_iface``             | The network interface used to listen for apiserver requests. Defaults to the Ansible default as described by the default_ipv4 fact. |
| ``kube_apiserver_etcd_group``        | The Ansible group containing all etcd server hosts. This is used to configure apiserver to with etcd connection information. |
| ``kube_apiserver_admission_plugins`` | The list of apiserver [admission plugins](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) to be enabled. |
| ``kube_controller_manager_iface``    | The network interface used to listen for controller manager requests. Defaults to the Ansible default as described by the default_ipv4 fact. |


## Configuration (dependencies)
Some of the relevant configuration from dependencies is shown below.
| Key | Dependency |
|-----|------------|
| ``kube_version``        | jumperfly.kubelet |
| ``kube_pod_cidr_range`` | jumperfly.kubernetes_node |
| ``kube_cluster_dns``    | jumperfly.kuberneteS_node |
| ``kube_cluster_domain`` | jumperfly.kubernetes_node |

## SSL Configuration
Keys and certificates are loaded from the locations shown below.
The ``jumperfly.ssl_cert`` role can be used to generate them if required. For example, the following will work with the default configuration:
```
  - role: jumperfly.ssl_cert
    ssl_cert_type: client
    ssl_cert_file_base_dir: /srv/kubernetes
    ssl_cert_name: apiserver-{{ ansible_hostname }}-etcd
    ssl_cert_ca_delegate_name: etcd-ca
    ssl_cert_subject_common_name: "kubernetes"
  - role: jumperfly.ssl_cert
    ssl_cert_type: server
    ssl_cert_file_base_dir: /srv/kubernetes
    ssl_cert_name: apiserver-{{ ansible_hostname }}-kube
    ssl_cert_ca_delegate_name: kubernetes-ca
    ssl_cert_subject_common_name: kubernetes
    ssl_cert_hosts:
      - kubernetes
      - kubernetes.default
      - kubernetes.default.svc
      - kubernetes.default.svc.cluster
      - kubernetes.default.svc.cluster.local
      - 10.0.0.1
      - "{{ ansible_default_ipv4.address }}"
```

| Key | Description |
|-----|-------------|
| ``kube_apiserver_cert_dir``     | The base directory for all certificates. All locations listed below must fall within this directory. |
| ``kube_apiserver_etcd_key``     | The location of the etcd client private key. Defaults to ``/srv/kubernetes/apiserver-{{ ansible_hostname }}-etcd-key.pem``. |
| ``kube_apiserver_etcd_cert``    | The location of the etcd client certificate. Defaults to ``/srv/kubernetes/apiserver-{{ ansible_hostname }}-etcd.pem``. |
| ``kube_apiserver_etcd_ca_cert`` | The location of the etcd CA certificate chain. Defaults to ``/srv/kubernetes/apiserver-{{ ansible_hostname }}-etcd-chain.pem`` |
| ``kube_apiserver_key``          | The location of the apiserver private key. Defaults to ``/srv/kubernetes/apiserver-{{ ansible_hostname }}-kube-key.pem``. |
| ``kube_apiserver_cert``         | The location of the apiserver certificate. Defaults to ``/srv/kubernetes/apiserver-{{ ansible_hostname }}-kube.pem``. |
| ``kube_apiserver_ca_cert``      | The location of the apiserver CA certificate chain. Defaults to ``/srv/kubernetes/apiserver-{{ ansible_hostname }}-kube-chain.pem`` |
