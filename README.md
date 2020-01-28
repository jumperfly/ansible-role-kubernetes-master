# Kubernetes Master Ansible Role
Creates a standalone Kubernetes master node.

## Configuration
| Key | Description |
|-----|-------------|
| `kube_master_version`   | The Kubernetes version. |
| `kube_master_base_install_enabled` | Whether to attempt to install the base binaries. Can be disabled if it is known they will already exist, eg. on a base image. |
| `kube_master_base_config_enabled` | Whether to attempt to reconfigure the master from the template files. Can be disabled if the system is already configured and no vars have changed. |
| `kube_master_encryption_config_enabled` | Whether to attempt to reconfigure the apiserver encrpytion from the template file. Can be disabled if the system is already configured and the template has not changed. |
| `kube_master_enable_services` | Whether to enable, start and verify services are running. |
| `kube_master_services`     | The list of master services to be installed. |
| `kube_master_binaries` | The list of kubernetes binaries to be installed. |
| `kube_master_listen_iface` | The network interface used for master services. |
| `kube_master_listen_ip` | The IP to listen on for master services. Defaults to determine this from the configured `kube_master_listen_iface`. |
| `kube_master_etcd_group` | The ansible group containing all etcd nodes. |
| `kube_master_pod_cidr_range` | The subnet to use for pods. |

## SSL Configuration
Keys and certificates are loaded from the locations shown below.
The `jumperfly.ssl_cert` role can be used to generate them if required. See the tests folder for an example.

| Key | Description |
|-----|-------------|
| `kube_master_cert_dir` | The base directory for all certificates. By default all certificates below are expected here. |
| SSL file paths         | See `defaults/main.yml` for the full list of file locations. All defaults are configured to reside in `kube_master_cert_dir`. |
