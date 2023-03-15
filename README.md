Provision a Kubernetes cluster with Ansible and Vagrant
===========================================

## Requirements:

* [Vagrant](https://developer.hashicorp.com/vagrant/docs/installation) installed on your local host.

* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) installed on your local host.

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management) optional.

At the moment the vagrant script requires virtualbox be also installed, but this requirement
can easily be changed on the script, `Vagrantfile`.

## Getting started

```commandline
  vagrant up --provider=virtualbox --provision
```

This should take a short while, depending on your network speed. On successful completion you should have a cluster running.

On the cluster directory a copy of kube-config will also have been downloaded. It can be used to authenticate on cluster and execute commands.

For example, to check node status;

```commandline
  kubectl get nodes --kubeconfig=cluster/kubeconfig
```

Alternately you can ssh into the control node and execute commands;

```commandline
  vagrant ssh control01
  kubectl get nodes
```

## Alternatives

* [kind](https://github.com/kubernetes-sigs/kind)
