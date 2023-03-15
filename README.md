Provision Kubernetes cluster using Vagrant
===========================================

# Requirements:

* [Vagrant](https://developer.hashicorp.com/vagrant/docs/installation) installed on your local host.

* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) installed on your local host.

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management) optional.

At the moment the vagrant script requires virtualbox be also installed, but this requirement
can easily be modified on the script, `Vagrantfile`.

# Getting started

```commandline
  vagrant up --provider=virtualbox --provision
```

This should take a short while, depending on your network speed. On successful completion you should have a cluster running. 

On the cluster directory a copy of kube-config will also have been downloaded, which can be used to authenticate on cluster and execute commands.

To check kubernetes status;

```commandline
  kubectl get nodes --kubeconfig=cluster/kubeconfig
```

