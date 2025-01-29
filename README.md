# Kubernetes Cluster Setup with Vagrant

This guide provides steps to set up a Kubernetes cluster using Vagrant and VirtualBox, including provisioning for both the Master and Worker nodes.

## Prerequisites

Ensure you have the following tools installed on your machine:

Vagrant (https://www.vagrantup.com/downloads)

VirtualBox (https://www.virtualbox.org/wiki/Downloads)

## VM Provisioning

1. Set Up the Vagrantfile

Save the provided Vagrantfile in a directory of your choice.

2. Start the Virtual Machines

Run the following command in the directory where your Vagrantfile is located:

vagrant up

This will provision the Master and Worker nodes.

## Configuring the Kubernetes Cluster

Step 1: Initialize the Kubernetes Master Node

SSH into the master node:

vagrant ssh k8s-master

Run the master setup script:

sudo bash /vagrant/master.sh

At the end of the execution, copy the kubeadm join command displayed in the output. It will look like this:

sudo kubeadm join 192.168.56.10:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>

Save this command for later, as you will need it to connect worker nodes.

Step 2: Generate a New Join Token (If Needed)

If you forgot to save the kubeadm join command or need to create a new token:

SSH into the master node:

vagrant ssh k8s-master

Create a new token:

sudo kubeadm token create

Retrieve the discovery token CA certificate hash:

openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt \
| openssl rsa -pubin -outform der 2>/dev/null \
| openssl dgst -sha256 -hex \
| sed 's/^.* //'

Form the new join command using the token and hash values:

sudo kubeadm join 192.168.56.10:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>

Step 3: Join Worker Nodes to the Cluster

For each worker node:

SSH into the worker node (e.g., k8s-worker1):

vagrant ssh k8s-worker1

Run the worker setup script:

sudo bash /vagrant/worker.sh

Use the saved or newly generated kubeadm join command to connect the worker to the cluster.

Repeat these steps for additional worker nodes (e.g., k8s-worker2).

Step 4: Verify the Cluster

Once all worker nodes have joined the cluster, go back to the master node and check the status:

kubectl get nodes

You should see both the master and worker nodes listed as Ready.

## Conclusion

You now have a fully functional Kubernetes cluster running inside VirtualBox using Vagrant. 🎉

From here, you can:

Deploy applications on your Kubernetes cluster.

Install additional networking solutions or monitoring tools.

Experiment with scaling the cluster.

Enjoy your Kubernetes setup! 🚀