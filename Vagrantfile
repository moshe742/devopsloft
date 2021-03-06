# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
	config.vm.define "dev" do |dev|

		dev.vm.box = "ubuntu/bionic64"
		config.vm.provision :ansible_local do |ansible|
			ansible.playbook = "playbooks/bootstrap-infra.yml"
		end
		dev.vm.provision "shell",path: "bootstrap-db.sh"
		dev.vm.provision "shell",path: "bootstrap-app.sh"
		dev.vm.network "forwarded_port", guest: 80, host: 5000

		dev.vm.provider :virtualbox do |virtualbox,override|
			virtualbox.name = "devopsloft_dev"
			virtualbox.memory = 1024
			virtualbox.cpus = 2
		end
	end

	config.vm.define "cicd" do |cicd|

		cicd.vm.box = "ubuntu/bionic64"
		cicd.vm.provision :ansible_local do |ansible|
			ansible.playbook = "playbooks/bootstrap-infra.yml"
		end
		cicd.vm.provision "shell",path: "bootstrap-db.sh"
		cicd.vm.provision "shell",path: "bootstrap-app.sh"
		cicd.vm.network "forwarded_port", guest: 5000, host: 5000

		cicd.vm.provider :virtualbox do |virtualbox,override|
			virtualbox.name = "devopsloft_dev"
			virtualbox.memory = 1024
			virtualbox.cpus = 2
		end
	end

	config.vm.define "stage" do |stage|

		stage.vm.synced_folder ".", "/vagrant", disabled: true
		stage.vm.box = "dummy"
		stage.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
		stage.vm.provision "file", source: ".", destination: "$HOME/devopsloft"
		stage.vm.provision :ansible_local do |ansible|
			ansible.provisioning_path = "/home/ubuntu/devopsloft"
			ansible.playbook = "playbooks/bootstrap-infra.yml"
		end
		stage.vm.provision "shell",path: "bootstrap-db.sh"
		stage.vm.provision "shell",path: "bootstrap-app.sh"

		stage.vm.provider :aws do |aws,override|
		end

	end

	config.vm.define "prod" do |prod|

		prod.vm.synced_folder ".", "/vagrant", disabled: true
		prod.vm.box = "dummy"
		prod.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
		prod.vm.provision "file", source: ".", destination: "$HOME/devopsloft"
		prod.vm.provision :ansible_local do |ansible|
			ansible.provisioning_path = "/home/ubuntu/devopsloft"
			ansible.playbook = "playbooks/bootstrap-infra.yml"
		end
		prod.vm.provision "shell",path: "bootstrap-db.sh"
		prod.vm.provision "shell",path: "bootstrap-app.sh"

		prod.vm.provider :aws do |aws,override|
		end

	end
end

eval IO.read("Vagrantfile.local") if File.file?("Vagrantfile.local")
