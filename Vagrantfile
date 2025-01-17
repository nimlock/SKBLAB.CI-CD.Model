$provision_script = <<END

# Установка docker и добавление пользователя в группу
#
apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

apt-get update && apt-get install -y \
	docker-ce \
	docker-ce-cli \
	containerd.io

sudo usermod -aG docker "${USER}"

# Установка docker-compose
#
curl -fsSL "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

# Установка minikube и kubectl
#
sudo curl -o /usr/local/sbin/minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo chmod +x /usr/local/sbin/minikube
sudo curl -o /usr/local/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
sudo chmod +x /usr/local/bin/kubectl

# Установка pip3 и требуемых модулей
#
sudo apt-get install python3-pip -y
pip3 install docker docker-compose

END


Vagrant.configure("2") do |config|

	config.vm.box = "bento/ubuntu-20.04"
	config.vm.hostname = "SKBLAB-mainserver"
	config.vm.synced_folder ".", "/vagrant", disabled: true
	config.vm.provider "hyperv" do |h|
		h.cpus = 4
		h.memory = 5120
		h.linked_clone = true
		h.vmname = "SKBLAB-mainserver"
		h.vm_integration_services = {
			guest_service_interface: true,
			heartbeat: true,
			shutdown: true,
			time_synchronization: true
		}
	end

	config.vm.provision "shell", inline:$provision_script
end
