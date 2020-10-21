#
# versions numbers are in the following format 1.19.2-00
# Supported CNI plugins are weavenet, calico and flannel 
# calico is the default
#
# k8s version defaults to the latest version if not set
#
# note you should call vagrant up like this if you want a specific setup
# export K8S_VERSION=; export K8S_CNI=flannel; vagrant up
#
# You can also specify the number of worker nodes to create setting
# K8S_WORKER_COUNT env var before calling  vagrant up 
#

IMAGE_NAME = "bento/ubuntu-18.04"

K8S_VERSION_ENV = ENV.has_key?('K8S_VERSION') ? ENV['K8S_VERSION'] : ""
K8S_CNI_ENV     = ENV.has_key?('K8S_CNI') ? ENV['K8S_CNI'] : "calico" 
N = ENV.has_key?('K8S_WORKER_COUNT') ? ENV['K8S_WORKER_COUNT'].to_i : 1

Vagrant.configure("2") do |config|
    # is this to allow us to ssh from one node to the other?
    config.ssh.insert_key = false
    config.vm.box_check_update = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end
      
    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.50.10"
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "192.168.50.10",
                ansible_python_interpreter: "/usr/bin/python3",
                k8s_version: K8S_VERSION_ENV,
                k8s_cni: K8S_CNI_ENV,
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.50.#{i + 10}",
                    ansible_python_interpreter: "/usr/bin/python3",
                    k8s_version: K8S_VERSION_ENV,
                }
            end
        end
    end
end

