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
* [Step 3: Experimenting](#step-3-experimenting)
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

## Step 3: Experimenting

_**Terminal:**_

~~~ shell
ansible-playbook -i ansible/inventory.yml ansible/setup.yml ansible/test.yml ansible/teardown.yml
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
