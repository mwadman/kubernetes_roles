Helm Repository Role
====================

Adds and removes Helm repositories.

Requirements
------------

- Requires the [Kubernetes Core Ansible Collection](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) be installed.
- Requires Helm be installed.

Role Variables
--------------

This role takes a list named `helm_repositories` as input.  
Each item in the list can have parameters set according to the [helm_repository module](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/helm_repository_module.html) documentation.

For example:

```yaml
helm_repositories:
# Adds a repository
  - repo_name: "ingress-nginx"
    repo_url: "https://kubernetes.github.io/ingress-nginx"
# Removes a repository
  - repo_name: "jetstack"
    repo_state: "absent"
# Full example
  - repo_name: "prometheus-community"
    repo_state: "present"
    repo_url: "https://prometheus-community.github.io/helm-charts"
    api_key: "abcdef01-2345-6789-abcd-ef0123456789" # Can also be specified via K8S_AUTH_API_KEY environment variable.
    binary_path: "/usr/bin/helm"
    ca_cert: "/etc/ssl/certs/ca-certificates.crt" # Can also be specified via K8S_AUTH_SSL_CA_CERT environment variable.
    context: "default" # Can also be specified via K8S_AUTH_CONTEXT environment variable.
    force_update: false
    host: "https://k8s.example.com:6443" # Can also be specified via K8S_AUTH_HOST environment variable.
    kubeconfig: "$HOME/.kube/config" # Can also be provided as a dictionary, or specified via K8S_AUTH_KUBECONFIG environment variable.
    pass_credentials: false
    repo_username: "repo_admin"
    repo_password: "SUPERSECUREPASSWORD"
    validate_certs: true
```

Example Playbook
----------------

```yaml
---
- hosts: localhost
  connection: local
  roles:
    - mwadman.kubernetes_roles.helm_repository
```
