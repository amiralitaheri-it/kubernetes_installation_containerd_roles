Role Name
=========

A Kubernetes Ansible role for installation is a pre-defined set of tasks and configurations that can be used with Ansible to automate the installation and configuration of Kubernetes on target machines. Ansible roles are a way to organize and package tasks, templates, and variables into reusable units, making it easier to manage complex configurations and deployments.

Requirements
------------
kubernetes.core module
```
ansible-galaxy collection install kubernetes.core
```

Debian base server

Example Hosts
----------------

```
[k8s_master_node]
k8smaster ansible_host=192.168.177.133

[k8s_other_master_nodes]
master2 ansible_host=192.168.177.144

[k8s_worker_nodes]
worker1 ansible_host=192.168.177.142
worker2 ansible_host=192.168.177.143
```

Example Playbook
----------------

```
- name: Install kubernetes cluster with kubeadm on ubuntu with containerd run-time
  hosts: all
  become: yes

  tasks:
    - name: Run tasks for all nodes
      import_role:
        name: kubernetes_containerd_installation
        tasks_from: main_all_nodes.yml

- name: Initialize k8s cluster on master node
  hosts: k8s_master_node
  become: yes

  tasks:
    - name: Run tasks for main master node
      import_role:
        name: kubernetes_containerd_installation
        tasks_from: main_master_nodes.yml

- name: Send kubeadm init result file to all nodes
  hosts:
    - k8s_other_master_nodes
    - k8s_worker_nodes
  become: yes

  tasks:
    - name: Run tasks for sending files to nodes
      import_role:
        name: kubernetes_containerd_installation
        tasks_from: send_init_result.yml

- name: Join master nodes into the cluster
  hosts: k8s_other_master_nodes
    become: yes

  tasks:
    - name: Run tasks for join master nodes into the cluster
      import_role:
        name: kubernetes_containerd_installation
        tasks_from: join_master_nodes.yml

- name: Join worker nodes into the cluster
  hosts: k8s_worker_nodes
  become: yes

  tasks:
    - name: Run tasks for join worker nodes into the cluster
      import_role:
        name: kubernetes_containerd_installation
        tasks_from: join_worker_nodes.yml

- name: Deploy Nginx deployment
  hosts: k8s_master_node
  become: yes

  tasks:
    - name: Run tasks for Deploy Nginx deployment
      import_role:
        name: kubernetes_containerd_installation
        tasks_from: deploy_nginx_pod.yml
```

Author Information
------------------

Amir Ali Taheri :)
