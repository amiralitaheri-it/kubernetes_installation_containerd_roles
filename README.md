Role Name
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

- name: Install kubernetes cluster with kubeadm on ubuntu with containerd run-time
  hosts: all
  become: yes

  tasks:
    - name: Run tasks for all nodes
      import_role:
        name: kubernetes_containerd_installation
        tasks_from: main_all_nodes.yml

- name: Initialize k8s cluster on master node
  hosts: k8s_master_nodes
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


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
