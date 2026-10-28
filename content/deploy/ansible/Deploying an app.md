Deploying a new web app in the lab requires creating a new [[Ansible|/deploy/Ansible.md]] playbook and deploying it on the cluster.

Refer to the [[Ansible|/deploy/Ansible.md]] page for details on the repo.

## Resources
[[Kubernetes|/deploy/Kubernetes.md]] resources are defined under /kubernetes-resources/<app>/.
We use [[jinja2|https://jinja.palletsprojects.com/en/stable/]] templates for the resources to let us pass in variables (as opposed to plain yaml). Each file represents a resource to be deployed in the cluster.

## Playbooks
Create a new playbook under the project root/playbooks. Use the below example as a template for a fresh playbook.
```yaml
- name: Setup app
  hosts: localhost

  vars_files:
    - ../vault/vault.yaml

  vars:
    base_url: "app.{{ base_domain }}"
    issuer: "letsencrypt-staging"
    state: present
    app: app

  environment: 
    K8S_AUTH_KUBECONFIG: "{{ kubeconfig_location }}"

  tasks:
  - name: Create individual resource
    ansible.builtin.include_role:
      name: k8s
      tasks_from: apply-j2-template.yaml
    vars:
      resource: ingress

```

- `hosts` must be set to `localhost` when deploying in the [[K8S|/deploy/Kubernetes.md]] cluster
- the `vault.yaml` file is used for storing secrets, we generally load this in every playbook or as needed
- `vars`
    - `base_url` is used for setting the url used on the ingress and SSL cert
    - `issuer` determines which issuer to issue the cert under. Use `letsencrypt` while testing deployments
    - `state` determines the default state of the deployment. Using the run script we can pass through `absent` to quickly deploy/undeploy the app
    - `app` is used for the K8S roles to find resources under `/kubernetes-resources`
- `K8S_AUTH_KUBECONFIG` is required for some K8S roles to correctly find the kubeconfig. It is by default set to `.config/kubeconfig.yaml`

### Roles
Generally we can deploy K8S resources using a single role. The role assumes that resources are written using jinja2 templates and deploys them. The role has 4 different tasks in it:

- `apply-j2-template.yaml` deploys a single resource given a single j2 template
- `apply-j2-templates-list` deploys resources given a list of j2 templates
- `apply-j2-templates-dir` deploys all resources under an apps resource folder
- `apply-helm` applies a helm chart given the relevent chart details


## Running the playbook
Playbooks can be run with `./runplaybook playbooks/<playbook>.yaml`.

> Optionally they can be run with `./runplaybook playbooks/<playbook>.yaml --absent` to unapply the resource