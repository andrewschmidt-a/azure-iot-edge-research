

# Getting started (Azure)
## Pre-requisites
- Azure CLI

```bash 
az vm create --resource-group {{rg}} --name {{name}} --imagemicrosoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest --admin-username azureuser --generate-ssh-keys

az vm run-command invoke -g {{rg}} -n {{name}} --command-idRunShellScript --script"/etc/iotedge/configedge.sh 'device_connection_string'"
```

# Getting started (Multi-pass)
## Pre-requisites
- Mutli-pass
- Virtualbox

```bash
brew cask install multipass
brew cask install virtualbox
sudo multipass set local.driver=virtualbox
```
Edit the init.yaml file to contain a connection string from IoT Hub

```bash
multipass launch --name ubuntu-lts --cloud-init init.yaml
multipass exec ubuntu-lts -- bash
```

NOTE: you may experience problems with blocked protocols in a 3M Corporate network


# First Steps

You can now run a check
```
sudo iotedge check
```

You may need to change some permissions on the management port. I tried putting this in the cloud init script but I dont think it allows me to change owners.

```
chown -R root:iotedge /var/run/iotedge
chmod 770 -R /var/run/iotedge/
systemctl restart iotedge.service
```




List the modules
```
sudo iotedge list
```



### Cleanup
```
multipass delete ubuntu-lts
multiplass purge
```
