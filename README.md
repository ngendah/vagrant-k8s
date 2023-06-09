Provision a Kubernetes cluster with Ansible and Vagrant
==============================================

Why?

* Want to test out different container runtimes.

* Want to test out tools such as IPVS, AppArmor, Falco e.tc.

* Want clear, understandable and extensible Ansible playbooks.

## Requirements:

#### Linux

* [Vagrant](https://developer.hashicorp.com/vagrant/docs/installation) installed on your local host.

  At the moment the vagrant script requires [Virtualbox](https://www.virtualbox.org/wiki/Documentation) be installed. However this can easily be changed on the script, `Vagrantfile`.

* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) version >= 2.10 installed on your local host.


* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management) installed on your local host. This is optional.

#### Windows

Ansible is not supported on Windows and the 'best' solution is to allow [Vagrant and Ansible](https://developer.hashicorp.com/vagrant/docs/provisioning/ansible_local) run the playbooks on the guest virtual machine.

## Getting started

```commandline
  vagrant up --provision --provider virtualbox
```

This should take a short while, but upon successful completion you should have a cluster running, reachable via the assigned private ip, on port 6443.

In addition, a copy of `kubeconfig` will have been downloaded into the cluster directory, `cluster/`. It can be used for authentication on the cluster for execution of commands.

For example, to check node status;

```commandline
  kubectl --kubeconfig ./cluster/kubeconfig get nodes
```

or

```commandline
  export KUBECONFIG=$(pwd)/cluster/kubeconfig
  kubectl get nodes
```

If kubectl is not installed on your local host, you can ssh into the control node and run commands;

```commandline
  vagrant ssh control01
  kubectl get nodes
```

After successful provisioning of the cluster, you can manage the nodes as follows;

* stopping the nodes

```commandline
  vagrant halt
```

* restarting the nodes

```commandline
  vagrant up
```

* destroying the nodes

```commandline
  vagrant destroy
```

* if a node is running to re-provision it

```commandline
  vagrant provision [node name/virtual machine name]
```

For additional details on these commands and others, consult [Vagrant documentation](https://developer.hashicorp.com/vagrant/docs).

## Installed features

#### Kubernetes Dashboard

* [Kubernetes dashboard](https://github.com/kubernetes/dashboard)

  In order to facilitate dashboard access the provisioner will create a dashboard html stub file on the cluster directory, `cluster`, together with a corresponding login token.
  From a file browser you can double-click on the stub file to open the dashboard.

#### Metrics

* [Metrics server](https://github.com/kubernetes-sigs/metrics-server)

#### Container runtimes

* [gVisor](https://gvisor.dev/docs/)

  Its runtime class name is `gvisor`

#### Policy

* [Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/docs/)

#### IPVS

* IPVS is installed by default but not enabled.

  To enable and use [IPVS](https://kubernetes.io/docs/reference/config-api/kube-proxy-config.v1alpha1/):

  1. Edit `kube-proxy` config-map and set its `mode` to `ipvs`:
      
    ```commandline
      kubectl -nkube-system edit cm kube-proxy
    ```

  2. Re-create all the `kube-proxy` pods:

    ```commandline
      kubectl -nkube-system delete po -l k8s-app=kube-proxy
    ```

#### Security

* [AppArmor](https://ubuntu.com/server/docs/security-apparmor)

* [Falco](https://falco.org/docs/)

* [Audit Policy](https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/)


## Alternatives

* [k3d](https://k3d.io/v5.4.9/)

* [kind](https://github.com/kubernetes-sigs/kind)
