Kubernetes Object Role
====================

Manages Kubernetes objects.

Requirements
------------

- Requires the [Kubernetes Core Ansible Collection](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/index.html) be installed.
- Requires jsonpatch python library.

Role Variables
--------------

This role takes a list named `k8s_objects` as input.  
Each item in the list can have parameters set according to the [k8s module](https://docs.ansible.com/ansible/latest/collections/kubernetes/core/k8s_module.html) documentation.

For example:

```yaml
k8s_objects:
# Apply a CRD via HTTPS
  - apply: true
    src: "https://github.com/cert-manager/cert-manager/releases/download/v1.11.0/cert-manager.crds.yaml"
# Delete a object
  - name: "example-namespace"
    api_version: "v1"
    kind: "Namespace"
    state: "absent"
# Full example for `state: "present"`
  - api_key: "abcdef01-2345-6789-abcd-ef0123456789" # Can also be specified via K8S_AUTH_API_KEY environment variable.
    api_version: "v1"
    append_hash: false
    apply: true
    ca_cert: "/etc/ssl/certs/ca-certificates.crt" # Can also be specified via K8S_AUTH_SSL_CA_CERT environment variable.
    client_cert: "$HOME/.pki/user.crt" # Can also be specified via K8S_AUTH_CERT_FILE environment variable.
    client_key: "$HOME/.pki/user.key" # Can also be specified via K8S_AUTH_KEY_FILE environment variable.
    context: "default" # Can also be specified via K8S_AUTH_CONTEXT environment variable.
    continue_on_error: false
    force: false
    generate_name: "example-"
    host: "https://k8s.example.com:6443" # Can also be specified via K8S_AUTH_HOST environment variable.
    impersonate_groups: "exampleGroup1,exampleGroup2" # Can also be specified via K8S_AUTH_IMPERSONATE_GROUPS environment variable.
    impersonate_user: "john.doe@example.com" # Can also be specified via K8S_AUTH_IMPERSONATE_USER environment variable.
    kubeconfig: "$HOME/.kube/config" # Can also be provided as a dictionary, or specified via K8S_AUTH_KUBECONFIG environment variable.
    label_selectors:
      - kubernetes.io/control-plane=true
    no_proxy: “localhost,.local,.example.com,127.0.0.1,127.0.0.0/8,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16” # Can also be specified via K8S_AUTH_NO_PROXY environment variable.
    password: "SUPERSECRETPASSWORD" # Can also be specified via K8S_AUTH_PASSWORD environment variable.
    persist_config: "{{ item.persist_config | default(omit) }}" # Can also be specified via K8S_AUTH_PERSIST_CONFIG environment variable.
    proxy: "http://proxy.example.com:3128/" # Can also be specified via K8S_AUTH_PROXY environment variable.
    proxy_headers:
      basic_auth: "admin:SUPERSECRETPASSWORD" # Can also be specified via K8S_AUTH_PROXY_HEADERS_BASIC_AUTH environment variable.
      proxy_basic_auth: "admin:SUPERSECRETPASSWORD" # Can also be specified via K8S_AUTH_PROXY_HEADERS_PROXY_BASIC_AUTH environment variable.
      user_agent: "foo/1.0" # Can also be specified via K8S_AUTH_PROXY_HEADERS_USER_AGENT environment variable.
    resource_definition:
      apiVersion: v1
      kind: ConfigMap
      metadata:
        name: example-configmap
    server_side_apply:
      field_manager: "ansible"
      force_conflicts: false
    state: "present"
    username: "admin" # Can also be specified via K8S_AUTH_USERNAME environment variable.
    validate:
      fail_on_error: true
      strict: true
      version: "v1.24.11"
    validate_certs: true # Can also be specified via K8S_AUTH_VERIFY_SSL environment variable.
    wait: false
    wait_condition:
      reason: "Ready"
      status: "True"
      type: "Pod"
    wait_sleep: 5
    wait_timeout: 120
# Example for `state: "absent"` with options not included above
  - delete_options:
      gracePeriodSeconds: 300
      preconditions:
        resourceVersion: 1234
        uid: "abcdef01-2345-6789-abcd-ef0123456789"
      propagationPolicy: "Foreground"
    template: example-template.j2
```

Example Playbook
----------------

```yaml
---
- hosts: localhost
  connection: local
  roles:
    - mwadman.kubernetes_roles.k8s_object
```
