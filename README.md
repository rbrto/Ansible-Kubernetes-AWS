# Ansible - Kubernetes - AWS (Centos 7)

Use this ansible playbook to set up a Kubernetes cluster from scratch using Ansible and Kubeadm into AWS

## Prerequisites & Config

1. create 3 EC2 instance into AWS with centos7 ami and download the key_pair.pem
2. create security group to master instance and workers instances
3. allow inbound traffic on 6443 port (security group master EC2) to join workers
4. ssh into all EC2 instances and run (sudo yum update -y)

## Example SSH
- ssh -i ~/.ssh/key_pair.pem centos@instance-ip

## Install Playbook

1. ansible-playbook -i hosts initial.yml
2. ansible-playbook -i hosts kube-dependencies.yml
3. ansible-playbook -i hosts master.yml
4. ansible-playbook -i hosts workers.yml

# Running application into cluster

- ssh centos@<ip-master>
- kubectl run nginx --image=nginx --port 80
- kubectl expose deploy nginx --port 80 --target-port 80 --type NodePort

# Check port assigned into workers nodes to connect to nginx port 80
Kubernetes will assign a random port that is greater than 30000 automatically

- kubectl get services

# Allow inbound traffic on port assigned by kubernetes into AWS security group (worker instances) to test nginx connection

To test that everything is working:

curl http://worker-node-ip-1:node-port
curl http://worker-node-ip-2:node-port

You will see Nginx's familiar welcome page.
