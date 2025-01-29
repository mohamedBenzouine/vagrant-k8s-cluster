Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"  # Change to your preferred Ubuntu version or box

  # Master VM configuration
  config.vm.define "master" do |master|
    master.vm.hostname = "k8s-master"
    master.vm.network "private_network", type: "static", ip: "192.168.56.10"
    master.vm.network "forwarded_port", guest: 6443, host: 6443  # Kubernetes API port
    master.vm.provider "virtualbox" do |vb|
      vb.name = "k8s-master"
      vb.memory = "2048"  # 2 GB RAM for the master node
      vb.cpus = 2
    end
  end

  # Worker 1 VM configuration
  config.vm.define "worker1" do |worker1|
    worker1.vm.hostname = "k8s-worker1"
    worker1.vm.network "private_network", type: "static", ip: "192.168.56.11"
    worker1.vm.network "forwarded_port", guest: 10250, host: 10250  # Kubelet port
    worker1.vm.provider "virtualbox" do |vb|
      vb.name = "k8s-worker1"
      vb.memory = "1024"  # 1 GB RAM for worker1
      vb.cpus = 1
    end
  end

  # Worker 2 VM configuration
  config.vm.define "worker2" do |worker2|
    worker2.vm.hostname = "k8s-worker2"
    worker2.vm.network "private_network", type: "static", ip: "192.168.56.12"
    worker2.vm.network "forwarded_port", guest: 10250, host: 10251  # Change port from 10250 to 10251
    worker2.vm.provider "virtualbox" do |vb|
      vb.name = "k8s-worker2"
      vb.memory = "1024"  # 1 GB RAM for worker2
      vb.cpus = 1
    end
  end
end
