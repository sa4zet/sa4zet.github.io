---
title: gcloud
body_classes: title-center title-h1h2
---

# links
- [vm prices](https://cloud.google.com/compute/vm-instance-pricing)
- [roles](https://cloud.google.com/iam/docs/understanding-roles)

# create project
```bash
gcloud projects create 'project-name' --organization='0000000000000'
```

# list available VM images
```bash
gcloud --project='project-name' compute images list
```

# create network
```bash
gcloud compute networks create 'network-eu-w3' \
--project='project-name' \
--bgp-routing-mode='regional' \
--subnet-mode='custom'
```

# create subnet
```bash
gcloud compute networks subnets create 'subnet-project-name' \
--project='project-name' \
--network='network-eu-w3' \
--range='10.5.0.0/16' \
--purpose='PRIVATE' \
--region='europe-west3' \
--role='ACTIVE' \
--stack-type='IPV4_ONLY'
```

# create firewall

```bash
gcloud compute firewall-rules create 'allow-internal' \
--project='project-name' \
--network='network-eu-w3' \
--direction='INGRESS' \
--action='ALLOW' \
--priority='1000' \
--rules='all' \
--source-ranges='10.5.0.0/16'
```

```bash
gcloud compute firewall-rules create 'allow-vpn' \
--project='project-name' \
--network='network-eu-w3' \
--direction='INGRESS' \
--action='ALLOW' \
--priority='65534' \
--rules='udp:28' \
--source-ranges='0.0.0.0/0' \
--target-tags='vpn'
```

# create NAT for private VMs
```bash
gcloud compute routers create 'router-nat-eu-w3' \
--region='europe-west3' \
--network='network-eu-w3'
```

```bash
gcloud compute routers nats create 'nat-eu-w3' \
--router='router-nat-eu-w3' \
--region='europe-west3' \
--auto-allocate-nat-external-ips \
--nat-all-subnet-ip-ranges
```

# create static external IP
```bash
gcloud compute addresses create 'ip-vpn-eu' \
--project='project-name' \
--network-tier='PREMIUM' \
--region='europe-west3'
```

```bash
gcloud compute addresses create 'ip-nginx-eu' \
--project='project-name' \
--network-tier='PREMIUM' \
--region='europe-west3'
```

# create generic regional VM
```bash
gcloud compute instances create 'VM_NAME' \
--project='project-name' \
--zone='europe-west3-b' \
--image-project='debian-cloud' \
--image-family='debian-11' \
--address='ip-vpn' \
--boot-disk-size='10GB' \
--boot-disk-type='pd-balanced' \
--hostname='host' \
--machine-type='e2-small' \
--network-tier='PREMIUM' \
--network='network-eu-w3' \
--private-network-ip='10.5.0.10' \
--provisioning-model='STANDARD' \
--subnet='subnet-project-name' \
--metadata='ssh-keys=user:PUBLIC_KEY' \
--can-ip-forward \
--no-service-account \
--no-scopes
```

# create custom image from VM
```bash
gcloud compute images create 'img-docker-24-0-5' \
--project='oshi-jp' \
--source-disk='shaka-ai' \
--source-disk-zone='asia-northeast1-a' \
--source-disk-project='oshi-jp' \
--storage-location='europe-west3' \
--force
```

# create regional VM from custom image
```bash
gcloud compute instances create 'VM_NAME' \
--project='project-name' \
--zone='europe-west3-b' \
--image-project='project-name' \
--image='img-docker-20-10-21' \
--address='ip-nginx' \
--boot-disk-size='10GB' \
--boot-disk-type='pd-balanced' \
--hostname='host' \
--machine-type='e2-small' \
--network-tier='PREMIUM' \
--network='network-eu-w3' \
--private-network-ip='10.5.0.20' \
--provisioning-model='STANDARD' \
--subnet='subnet-project-name' \
--metadata='ssh-keys=user:PUBLIC_KEY' \
--tags='nginx' \
--can-ip-forward \
--no-service-account \
--no-scopes
```

# create VM without external IP
```bash
gcloud compute instances create 'VM_NAME' \
--project='project-name' \
--zone='europe-west3-b' \
--image-project='project-name' \
--image='img-docker-20-10-21' \
--boot-disk-size='10GB' \
--boot-disk-type='pd-balanced' \
--hostname='host' \
--machine-type='e2-medium' \
--network-tier='PREMIUM' \
--network='network-eu-w3' \
--private-network-ip='10.5.0.30' \
--provisioning-model='STANDARD' \
--subnet='subnet-project-name' \
--metadata='ssh-keys=user:PUBLIC_KEY' \
--can-ip-forward \
--no-address \
--no-service-account \
--no-scopes
```

# add tag to vm
```bash
gcloud compute instances add-tags 'VM_NAME' \
--project='project-name' \
--zone='europe-west3-b' \
--tags='vpn'
```

# create docker registry
```bash
gcloud artifacts repositories create 'NAME' \
--project='project-name' \
--location='europe-west3' \
--repository-format='docker'
```

# auth docker client

create key for service acc

https://console.cloud.google.com/iam-admin/serviceaccounts/details/108505938302647888514/keys?project=project-name&supportedpurview=project


```bash
docker login \
-u _json_key_base64 \
-p "$(base64 service-acc-project-name.json)" \
https://europe-west3-docker.pkg.dev
```


#create disk
```bash
gcloud compute disks create 'nfs-eu' \
--project='project-name' \
--zone='europe-west3-b' \
--type='pd-ssd' \
--size='5120GB' 
```

# change machine type on a stopped VM
```bash
gcloud compute instances set-machine-type 'VM_NAME' \
--project='project-name' \
--zone='europe-west3-b' \
--machine-type='n1-standard-8'
```

```bash
gcloud compute instances set-machine-type 'VM_NAME' \
--project='project-name' \
--zone='europe-west3-b' \
--custom-cpu='8' \
--custom-memory='16' \
--custom-extensions
```

# set service account on a stopped VM
```bash
gcloud compute instances set-service-account 'VM_NAME' \
--service-account='monitor@iam.gserviceaccount.com' \
--scopes='logging-write,monitoring'
```
