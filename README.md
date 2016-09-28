Ansible playbook to install a development Kubernetes (k8s) cluster using kubeadm
================================================================================

This uses CentOS 7.1

In a CloudStack cloud, I will skip the prereq for Ansible to focus on running the playbook.
If I have time I will modify the plays to run on AWS as well.

Create a Kubernetes cluster
---------------------------

Copy `vars/local_config.yml.example` to `vars/local_config.yml` and
update to your needs. These will be ignored by git so you can update
as you need. Similarly, copy `vars/secrets.yml.example` to `vars/secrets.yml`
and then use the `ansible-vault` tool to encrypt it, if you need to protect
it (you'll need to pass `--ask-vault-pass` when running the playbook if you
do this).

    $ ansible-playbook k8s.yml

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

