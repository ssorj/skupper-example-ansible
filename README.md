<!-- NOTE: This file is generated from skewer.yaml.  Do not edit it directly. -->

# Skupper Hello World using Ansible

[![main](https://github.com/ssorj/skupper-example-ansible/actions/workflows/main.yaml/badge.svg)](https://github.com/ssorj/skupper-example-ansible/actions/workflows/main.yaml)

This example is part of a [suite of examples][examples] showing the
different ways you can use [Skupper][website] to connect services
across cloud providers, data centers, and edge sites.

[website]: https://skupper.io/
[examples]: https://skupper.io/examples/index.html

#### Contents

* [Prerequisites](#prerequisites)
* [Step 1: Install the Skupper command-line tool](#step-1-install-the-skupper-command-line-tool)
* [Step 2: Install the Skupper Ansible collection](#step-2-install-the-skupper-ansible-collection)
* [Step 3: Set up your clusters](#step-3-set-up-your-clusters)
* [Step 4: Inspect the Ansible inventory](#step-4-inspect-the-ansible-inventory)
* [Step 5: Run the setup playbook](#step-5-run-the-setup-playbook)
* [Step 6: Access the frontend](#step-6-access-the-frontend)
* [Step 7: Run the teardown playbook](#step-7-run-the-teardown-playbook)
* [Next steps](#next-steps)
* [About this example](#about-this-example)

## Prerequisites

* The `kubectl` command-line tool, version 1.15 or later
  ([installation guide][install-kubectl])

* Access to at least one Kubernetes cluster, from [any provider you
  choose][kube-providers]

[install-kubectl]: https://kubernetes.io/docs/tasks/tools/install-kubectl/
[kube-providers]: https://skupper.io/start/kubernetes.html

## Step 1: Install the Skupper command-line tool

This example uses the Skupper command-line tool to deploy Skupper.
You need to install the `skupper` command only once for each
development environment.

On Linux or Mac, you can use the install script (inspect it
[here][install-script]) to download and extract the command:

~~~ shell
curl https://skupper.io/install.sh | sh
~~~

The script installs the command under your home directory.  It
prompts you to add the command to your path if necessary.

For Windows and other installation options, see [Installing
Skupper][install-docs].

[install-script]: https://github.com/skupperproject/skupper-website/blob/main/input/install.sh
[install-docs]: https://skupper.io/install/

## Step 2: Install the Skupper Ansible collection

_**Terminal:**_

~~~ shell
ansible-galaxy collection install skupper.network
~~~

## Step 3: Set up your clusters

Skupper is designed for use with multiple Kubernetes clusters.
The `skupper` and `kubectl` commands use your
[kubeconfig][kubeconfig] to select the cluster where they
operate.

[kubeconfig]: https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/

Your kubeconfig is stored in a file in your home directory.  The
`skupper` and `kubectl` commands use the `KUBECONFIG` environment
variable to locate it.

A single kubeconfig supports only one active cluster per user.
Since you will be using multiple clusters at once in this
exercise, you need to create distinct kubeconfigs.

For each cluster, set the `KUBECONFIG` environment variable to a
different path and log in.

**Note:** The login procedure varies by provider.  See the
documentation for yours:

* [Minikube](https://skupper.io/start/minikube.html#cluster-access)
* [Amazon Elastic Kubernetes Service (EKS)](https://skupper.io/start/eks.html#cluster-access)
* [Azure Kubernetes Service (AKS)](https://skupper.io/start/aks.html#cluster-access)
* [Google Kubernetes Engine (GKE)](https://skupper.io/start/gke.html#cluster-access)
* [IBM Kubernetes Service](https://skupper.io/start/ibmks.html#cluster-access)
* [OpenShift](https://skupper.io/start/openshift.html#cluster-access)

_**Terminal:**_

~~~ shell
export KUBECONFIG=~/.kube/config-west
# Enter your provider-specific login command for cluster 1
export KUBECONFIG=~/.kube/config-east
# Enter your provider-specific login command for cluster 2
~~~

## Step 4: Inspect the Ansible inventory

XXX inventory (sites, links, and services)

XXX <https://docs.ansible.com/ansible/latest/getting_started/get_started_inventory.html>

XXX [ansible/inventory.yml](ansible/inventory.yml)

~~~ yaml
all:
  vars:
    platform: kubernetes
    ansible_connection: local
  hosts:
    west:
      kubeconfig: /tmp/skewer/kubeconfig-west
      namespace: west
    east:
      kubeconfig: /tmp/skewer/kubeconfig-east
      namespace: east
      links:
        - host: west
      services:
        backend:
          ports:
            - 8080
          targets:
            - type: deployment
              name: backend
~~~

XXX Two sites, west and east.

XXX The playbook commands that follow use this inventory definition.

## Step 5: Run the setup playbook

The setup playbook has everything, but let's look at the
important pieces of it before we run it.

XXX setup tasks (with the skupper magic at the end)

_**Terminal:**_

~~~ shell
ansible-playbook -i ansible/inventory.yml ansible/setup.yml
~~~

_Sample output:_

~~~ console
$ ansible-playbook -i ansible/inventory.yml ansible/setup.yml
[...]

PLAY RECAP ***************************************************************************************************************************************
east                       : ok=34   changed=13   unreachable=0    failed=0    skipped=69   rescued=0    ignored=0
west                       : ok=34   changed=12   unreachable=0    failed=0    skipped=69   rescued=0    ignored=0
~~~

## Step 6: Access the frontend

In order to use and test the application, we need external access
to the frontend.

Use `kubectl port-forward` to make the frontend available at
`localhost:8080`.

_**Terminal:**_

~~~ shell
export KUBECONFIG=~/.kube/config-west
kubectl port-forward deployment/frontend 8080:8080
~~~

You can now access the web interface by navigating to
[http://localhost:8080](http://localhost:8080) in your browser.

## Step 7: Run the teardown playbook

XXX The reverse of the setup playbook.

_**Terminal:**_

~~~ shell
ansible-playbook -i ansible/inventory.yml ansible/teardown.yml
~~~

_Sample output:_

~~~ console
$ ansible-playbook -i ansible/inventory.yml ansible/teardown.yml
PLAY RECAP ***************************************************************************************************************************************
east                       : ok=9    changed=2    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
west                       : ok=9    changed=2    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
~~~

## Next steps

Check out the other [examples][examples] on the Skupper website.

## About this example

This example was produced using [Skewer][skewer], a library for
documenting and testing Skupper examples.

[skewer]: https://github.com/skupperproject/skewer

Skewer provides utility functions for generating the README and
running the example steps.  Use the `./plano` command in the project
root to see what is available.

To quickly stand up the example using Minikube, try the `./plano demo`
command.
