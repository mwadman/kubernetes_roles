Helm Package Role
=================

Installs, upgrades and deletes Helm packages.

Requirements
------------

- Requires the [Kubernetes Core Ansible Collection](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) be installed.
- Requires Helm be installed.

Role Variables
--------------

<!---
Commented out this section because a repo update can be completed by setting a list item in the `helm_packages` variable.


The first task in the role will run a Helm repository update *before* any package actions are taken, if `helm_repo_preupdate_enable` is set to true.

The repository update task accepts parameters for connecting to a Kubernetes cluster in the case that you're running a Helm version that requires it (i.e. version 2 and older).  
By default, this task will use the first value it finds for the equivalent parameter in the `helm_packages` list, or will omit these values if none are defined.  
This behaviour can be overridden by setting the `helm_repo_preupdate_*` variables below if required.

```yaml
helm_repo_preupdate_enable: true
# helm_repo_preupdate_api_key:
# helm_repo_preupdate_binary_path:
# helm_repo_preupdate_ca_cert:
# helm_repo_preupdate_context:
# helm_repo_preupdate_host:
# helm_repo_preupdate_kubeconfig:
# helm_repo_preupdate_validate_certs:
```
-->

This role takes a list named `helm_packages` as input.  
Each item in the list can have parameters set according to the [helm module](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/helm_module.html) documentation.

For example:

```yaml
helm_packages:
# Update packages first
  # Both name and namespace are required parameters, so we set them to dummy values to update the Helm repo cache first
  - release_name: "dummy-name-for-helm-repo-update"
    release_namespace: "dummy-namespace-for-helm-repo-update"
    release_state: absent
    update_repo_cache: true
# Installs a package
  - release_name: "example-ingress-nginx"
    release_namespace: "ingress-nginx"
    chart_ref: "nginx-stable/nginx-ingress"
    create_namespace: true
# Installs a package with configuration
  - release_name: "example-cert-manager"
    release_namespace: "cert-manager"
    chart_ref: "jetstack/cert-manager"
    chart_version: "v1.11.0"
    create_namespace: true
    set_values:
      - value: "installCRDs=true"
# Deletes a package
  - release_name: "example-jenkins"
    release_namespace: "default"
    release_state: "absent"
# Full example
    release_name: "example-prometheus"
    release_namespace: "monitoring"
    release_state: "present"
    chart_ref: "prometheus-community/prometheus"
    chart_repo_url: "https://prometheus-community.github.io/helm-charts"
    chart_version: "20.0.2"
    api_key: "abcdef01-2345-6789-abcd-ef0123456789" # Can also be specified via K8S_AUTH_API_KEY environment variable.
    atomic: false
    binary_path: "/usr/bin/helm"
    ca_cert: "/etc/ssl/certs/ca-certificates.crt" # Can also be specified via K8S_AUTH_SSL_CA_CERT environment variable.
    context: "default" # Can also be specified via K8S_AUTH_CONTEXT environment variable.
    create_namespace: false
    dependency_update: false # Requires the dependencies block is present in chart.yaml or requirements.yaml.
    disable_hook: false
    force: false
    history_max: 3
    host: "https://k8s.example.com:6443" # Can also be specified via K8S_AUTH_HOST environment variable.
    kubeconfig: "$HOME/.kube/config" # Can also be provided as a dictionary, or specified via K8S_AUTH_KUBECONFIG environment variable.
    post_renderer: "./kustomize"
    purge: true
    replace: false
    skip_crds: false
    timeout: "5m0s"
    update_repo_cache: false
    validate_certs: true
    values_files:
      - values.yaml
    wait: false
```

Example Playbook
----------------

```yaml
---
- hosts: localhost
  connection: local
  roles:
    - mwadman.kubernetes_roles.helm_packages
```
