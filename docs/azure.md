---
title: azure
body_classes: title-center title-h1h2
---

```bash
docker run -it --rm --name=azure.cli mcr.microsoft.com/azure-cli:latest
```

# Login

```bash
az login
# To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XXXXXXXX to authenticate.
```

# Set default subscription

```bash
az account set --subscription "Subscription Sponsorship"
```

# Save ssh public key

```bash
az sshkey create \
--name="ssh.key.devop" \
--resource-group="project.eu.gwc" \
--location="germanywestcentral" \
--public-key="ssh-rsa ..."
```

# Create a public static  IP

```bash
az network public-ip create \
--name="ip.vpn.project.eu.gwc" \
--resource-group="project.eu.gwc" \
--location="germanywestcentral" \
--allocation-method="Static" \
--version="IPv4"
```

# Create security group

* only needed when you have a public IP

```bash
az network nsg create \
--name="nsg.vpn.project.eu.gwc" \
--resource-group="project.eu.gwc" \
--location="germanywestcentral"
```

# Create Virtual Machine

## Create Network interface for the VM

```bash
az network nic create \
--name="nic.vpn.project.eu.gwc" \
--resource-group="project.eu.gwc" \
--location="germanywestcentral" \
--subnet="/subscriptions/0c1e2345-841f-4c26-9a43-000000000000/resourceGroups/project.eu.gwc/providers/Microsoft.Network/virtualNetworks/virtual.network.project.eu.gwc/subnets/subnet.project.eu.gwc" \
--private-ip-address="10.5.0.5" \
--public-ip-address="ip.vpn.project.eu.gwc"
```

## Create actual VM

```bash
az vm create \
--name="vm.vpn.project.eu.gwc" \
--os-disk-name="disk.vpn.project.eu.gwc" \
--resource-group="project.eu.gwc" \
--location="germanywestcentral" \
--size="Standard_B2s" \
--ssh-key-name="ssh.key.devop" \
--authentication-type="ssh" \
--admin-username="devop" \
--nics="nic.vpn.project.eu.gwc" \
--enable-agent="true" \
--enable-auto-update="false" \
--enable-hotpatching="false" \
--priority="Regular" \
--image="/subscriptions/0c1e2345-841f-4c26-9a43-000000000000/resourceGroups/project.eu.gwc/providers/Microsoft.Compute/galleries/gallery.project.eu.gwc/images/image.docker/versions/20.10.9"
```

# Create Virtual Machine With Accelerated Networking

## Create Network interface for the VM With Accelerated Networking

```bash
az network nic create \
--name="nic.redis.project.eu.gwc" \
--resource-group="project.eu.gwc" \
--location="germanywestcentral" \
--subnet="/subscriptions/0c1e2345-841f-4c26-9a43-000000000000/resourceGroups/project.eu.gwc/providers/Microsoft.Network/virtualNetworks/virtual.network.project.eu.gwc/subnets/subnet.project.eu.gwc" \
--private-ip-address="10.5.0.6" \
--accelerated-networking="true"
```

## Create actual VM

* The VM size has to support Accelerated Networking!

```bash
az vm create \
--name="vm.redis.project.eu.gwc" \
--os-disk-name="disk.redis.project.eu.gwc" \
--resource-group="project.eu.gwc" \
--location="germanywestcentral" \
--size="Standard_DS2_v2" \
--ssh-key-name="ssh.key.devop" \
--authentication-type="ssh" \
--admin-username="devop" \
--nics="nic.redis.project.eu.gwc" \
--enable-agent="true" \
--enable-auto-update="false" \
--enable-hotpatching="false" \
--priority="Regular" \
--image="/subscriptions/0c1e2345-841f-4c26-9a43-000000000000/resourceGroups/project.eu.gwc/providers/Microsoft.Compute/galleries/gallery.project.eu.gwc/images/image.docker/versions/20.10.9"
```

# Create Image Gallery
```bash
az group deployment create \
--resource-group="project.eu.gwc" \
--template-uri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-sig-create/azuredeploy.json"
```
