# Vagrant

## VagrantFile


## Operation

### Download vagrant box

```
vagrant box add precise32 http://files.vagrantup.com/precise32.box
```

### Spawn a new instance of vagrant box (based on VagrantFile)

```
vagrant up
```

### Destroy box

```
vagrant destroy
```

### SSH into box

```
vagrant ssh
```



### Config

```
$script = <<SCRIPT
echo Installing blahblah
...
SCRIPT

Vagrant.configure("2") do |config|
   config.vm.box = "ubuntu/xenial64"
   config.vm.hostname = "docker"
   config.vm.network :private_network, ip: "192.168.0.42"
   config.vm.provision "file", source: ".bash_profile", destination: ".bash_profile"
   config.vm.provision "shell", inline: $script
end
```
