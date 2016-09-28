Ansible playbook to install a development Kubernetes (k8s) cluster using kubeadm on CentOS in CloudStack
========================================================================================================


Prerequisites
-------------

You will need Ansible >= 2.0, sshpubkeys and [cs](https://github.com/exoscale/cs) :)

    $ sudo apt-get install -y python-pip
    $ sudo pip install ansible
    $ sudo pip install cs
    $ sudo pip install sshpubkeys

Setup cloudstack
----------------

Create a `~/.cloudstack.ini` file with your creds and cloudstack endpoint, for example:

    [cloudstack]
    endpoint = https://api.exoscale.ch/compute
    key = <your api access key> 
    secret = <your api secret key> 
    method = post

We need to use the http POST method to pass the userdata to the coreOS instances.

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
