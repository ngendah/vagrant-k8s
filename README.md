Provision a Kubernetes cluster with Ansible and Vagrant
===========================================

## Requirements:

#### Linux

* [Vagrant](https://developer.hashicorp.com/vagrant/docs/installation) installed on your local host.

  At the moment the vagrant script requires virtualbox be also installed, but this requirement
  can easily be changed on the script, `Vagrantfile`.

* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) installed on your local host.

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management) installed on your local host. This is optional.

* Port `6443` is free on your local host, see troubleshooting, port forwarding.

#### Windows

Ansible is not supported on Windows and the 'best' solution is to allow [Vagrant and Ansible](https://developer.hashicorp.com/vagrant/docs/provisioning/ansible_local) run the playbooks on the guest virtual machine.

## Getting started

```commandline
  vagrant up --provider virtualbox --provision
```

This should take a short while. On successful completion you should have a cluster running, with guest port 6443 forwarded to your localhost 6443.

On the cluster directory a copy of kube-config will also have been downloaded. It can be used to authenticate on cluster and execute commands.

For example, to check node status;

```commandline
  kubectl --kubeconfig ./cluster/kubeconfig get nodes
```

or

```commandline
  export KUBECONFIG=$(pwd)/cluster/kubeconfig
  kubectl get nodes
```

If kubectl is not installed you can ssh into the control node and execute commands;

```commandline
  vagrant ssh control01
  kubectl get nodes
```


## Other installed and enabled features

#### Container runtimes

* [gVisor](https://gvisor.dev/docs/)

  Its runtime class name is `gvisor`

#### Policy

* [Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/docs/)

#### Security

* [AppArmor](https://ubuntu.com/server/docs/security-apparmor)

* [Falco](https://falco.org/docs/)


## Troubleshooting

### Port forwarding

* You can change the default local host forwarded port on the `Vagrantfile` by editing the code fragment.

  ```ruby
  ports: [ ..., host: 6443, ... ],
  ```

* You can also enable vagrant port mapping [auto-correction feature](https://developer.hashicorp.com/vagrant/docs/networking/forwarded_ports).

* If the forwared port is changed or has changed and provisioning is successfull,
  the same change should be applied to the donwloaded kube-config file, `cluster/kubeconfig`.

* You can disable port forwarding by removing the code fragment;

  ```ruby
  ports: [ ... ],
  ```

## Alternatives

* [kind](https://github.com/kubernetes-sigs/kind)
