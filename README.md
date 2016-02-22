# Docker Registry Mirror on Local with Vagrant

## Boot up the Local Docker Registry Mirror

```bash
$ git clone https://github.com/ailispaw/docker-registry-mirror.git
$ cd docker-registry-mirror
$ vagrant up
$ open http://192.168.33.201
```

## Setup Docker Host VMs with Vagrant

Put the following code in your Vagrantfile.

```ruby
REGISTRY_IP = "192.168.33.201"

Vagrant.configure(2) do |config|
  config.vm.box = "ailispaw/docker-root"
.
.
.
  config.vm.provision :shell do |sh|
    sh.inline = <<-EOT
      echo 'DOCKER_EXTRA_ARGS="--userland-proxy=false \
        --registry-mirror=http://#{REGISTRY_IP}:5000 \
        --insecure-registry=#{REGISTRY_IP}:5000"' >> /var/lib/docker-root/profile
      /etc/init.d/docker restart
    EOT
  end
.
.
.
end
```
