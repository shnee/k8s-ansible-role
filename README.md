Ansible Role: Install Kubernetes
================================================================================

An Ansible role that installs Kubernetes.

This role has been tested on:
- CentOS 7 & 8
- Ubuntu 20.04

Role Variables
----------------------------------------

The users that will be added to the `docker` group.
This role will also install Docker via the `install_docker` role. The
`install_docker` role accepts a `docker_users` variable. The `install_k8s` role
just passes through this varaible.
```yml
docker_users: [admin docker_admin]
```

The kubelet config is automatically added to `~/.kube/config` of `ansible_user`
(the user used to run Ansible). Additionally you can set
`copy_kubectl_config_to_local` to have the file copied to you local machine
running the Ansible playbook.
```yml
# Defaults to false.
copy_kubectl_config_to_local: true
```

Versions of `kubelet`, `kubeadm`, and `kubectl` are pinned when installed. The
version intalled is controlled by these variables.
```yml
# Used for all non-RHEL-based distros.
k8s_version: "1.22"
k8s_rhel_version: "1.22.4"
```

Dependencies
----------------------------------------

### install\_docker

Use in requirements.yml
```yml
- src: git+https://github.com/shnee/docker-ansible-role.git
  name: install_docker
  version: master
```

Example Playbook
----------------------------------------

```yaml
- hosts: all
  vars:
    # This will set the role to 'master' if the string 'master' exists in the
    # group name. We expect each host to only be in 1 group for this role,
    # therefore we only check the first group.
    - kubernetes_role: >-
        {{  'master' if 'master' in group_names[0] else 'worker' }}
    - docker_users: [admin]
    - copy_kubectl_config_to_local: true
  roles:
    - install_k8s
```

License
----------------------------------------

MIT

Author Information
----------------------------------------

This role was created by [shnee](https://github.com/shnee).
