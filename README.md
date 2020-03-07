KVM & Kubernetes
==
Ansible playbooks to install and maintain KVM & Kubernetes. This is useful to create you own Kubernetes cluster within a dedicated single server or local computer running on CentOS 8. The Kubernetes nodes are using Ubuntu Xenial (16.04) Cloud Server.

This project has been created to satisfy customized requirements to use Kubernetes in a dedicated single server, however you can modify it and contribute back as it has been released as [LGPLv3](LICENSE).

Includes two playbooks:

- `kvm.yml`: Provides the following tagged actions:
  - `install`: Installs libvirt and its required libraries and a basic structure to for images and iso files (`/kvm/{iso|img}`).
  - `download`: Download `qcow2` images used to allocate the Kubernetes nodes.
  - `config`: Configure images to create the Kubernetes nodes, you can modify their settings (memory, hard disks, vCPUs) in `ansible/playbooks/roles/virt/defaults/main.yml`.
  - `create`: Execute the creation of images and VMs.
  - `reload`: Reloads VMs to apply custome IPs & network settings.

  **SSH KVM host note**: In order to access your KVM host you must setup your SSH config file, the current `.ssh/config` includes a file located in `~/.ssh/config.d/kvm-host`. The suggested configuration is:
  ```bash
  Host kvm-host
  Hostname <your-kvm-host-ip-address>
  User centos
  IdentityFile ~/.ssh/id_rsa
  ```
  **SSH public key note**: In order to access the nodes, it is necessary to copy your SSH public key, to do that please set a local environment variable named `PUB_KEY` with you public key, you can do that by executing the following command:
  ```bash
  export PUB_KEY=$(cat ~/.ssh/id_rsa.pub)
  ```
  If you SSH public key is located somewhere else or has a different name please use such location.
```bash
ansible-playbook -i inventories/ovh playbooks/kvm.yml -t install,download,config,create,reload -e "pub_key='$PUB_KEY'"
```

- `k8s.yml`: Provides the following tagged actions:
  - `install`: Installs Kubernetes and its dependencies.
  - `init`: Initializes the Kubernetes cluster.
  - `join`: Executes the kubeadm on each node so they can join the cluster.

```bash
ansible-playbook -i inventories/ovh playbooks/k8s.yml -t install,init,join
```
---
Feel yourself free to contact the author if you have any suggestions to improve this project.
