## Install Kubernetes

- ### Install Prereq's (Optionally target Control Plane Node)
1) - `ansible-playbook playbook-00-env-set.yml --limit cps`

- ### Install Packages (Optionally target Control Plane Node)
1) - `ansible-playbook playbook-01-package-installation.yml --limit cps`

---

- ### Check the status of the Kubernetes Services from the Control Plane Node
1) - ssh to 'control-plane-node'
1) - Check the status of our kubelet and our container runtime, containerd.

1) - The kubelet will enter a crashloop until a cluster is created or the node is joined to an existing cluster.
     - `sudo systemctl status kubelet.service`
     - `sudo systemctl status containerd.service`

1) - Ensure both are set to start when the system starts up.
     - `sudo systemctl enable kubelet.service`
     - `sudo systemctl enable containerd.service`

---

- ### Download the Pod Networking Config (Calico) and Install Kubernetes Cluster

1) - `wget https://raw.githubusercontent.com/projectcalico/calico/master/manifests/calico.yaml`

### NOTE : When using Vagrant, check which IP to 'advertise' the API Server on...

1) - Initialise the Cluster - Run Manually on Control Plane Node
     - `sudo kubeadm init --kubernetes-version v1.26.0`
     - or 
     - `sudo kubeadm init --kubernetes-version v1.26.0 --apiserver-advertise-address=192.168.56.21`

1) - Configure our account on the Control Plane Node to have admin access to the API server from a non-privileged account.
     - `mkdir -p $HOME/.kube`
     - `sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`
     - `sudo chown $(id -u):$(id -g) $HOME/.kube/config`

1) - Create the Pod Network (Deploy yaml file)
     - `kubectl apply -f calico.yaml`

1) - Wait for the system pods and calico pods to change to Running.
    - The DNS pod won't start (pending) until the Pod network is deployed and Running.
      - `kubectl get pods --all-namespaces --watch`

1) - Get a list of our current nodes, just the Control Plane Node Node...should be Ready.
     - `kubectl get nodes`

     