Provision a Kubernetes cluster with Ansible and Vagrant
===========================================

## Requirements:

* [Vagrant](https://developer.hashicorp.com/vagrant/docs/installation) installed on your local host.

  At the moment the vagrant script requires virtualbox be also installed, but this requirement
  can easily be changed on the script, `Vagrantfile`.

* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) installed on your local host.

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management) installed on your local host. This is optional.

* Port `6443` is free on your local host, see troubleshooting, port forwarding.


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

If kubectl is not installed you can ssh into the control node and execute commands;

```commandline
  vagrant ssh control01
  kubectl get nodes
```


## Other installed and enabled features

#### Container runtime

* [gVisor](https://gvisor.dev/docs/)

  Can be enabled with `runtimeClassName: gvisor`

#### Policy

* [Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/docs/)

#### Security

* [AppArmor](https://ubuntu.com/server/docs/security-apparmor)

* [Falco](https://falco.org/docs/)


## Troubleshooting

### Port forwarding

* You can change the default localhost forwarded port on the `Vagrantfile`.

* If the forwared port is changed on the vagrant file and provisioning has successfully completed, the port change should be applied to the donwloaded kube-config file, `cluster/kubeconfig`.

## Alternatives

* [kind](https://github.com/kubernetes-sigs/kind)
