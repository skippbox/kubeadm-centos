Ansible playbook to install a development Kubernetes (k8s) cluster using kubeadm
================================================================================

This uses CentOS 7.1

In a CloudStack cloud, I will skip the prereq for Ansible to focus on running the playbook.
If I have time I will modify the plays to run on AWS as well.

Create a Kubernetes cluster
---------------------------

    $ ansible-playbook k8s.yml

Some variables can be edited in the `k8s.yml` file.
This will start a Kubernetes master node and a number of compute nodes.

Check the tasks and templates in `roles/k8s`.

Bootstrap with `kubeadm`
------------------------

    $ ansible-playbook boot.yml

Then:

    $ ansible master -a "kubectl get nodes"
    master_node | SUCCESS | rc=0 >>
    NAME          STATUS    AGE
    kube-head     Ready     44s
    kube-node-1   Ready     5s
    kube-node-2   Ready     5s

Launch your network:

    $ kubectl apply -f https://git.io/weave-kube

And check that your DNS pod gets in a Running state.

