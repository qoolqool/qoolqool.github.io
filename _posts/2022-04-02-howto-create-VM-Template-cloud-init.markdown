---
layout: post
title:  "Howto create VM template for proxmox"
date:   2022-04-02 07:42:00 +0800
categories: [proxmox]
---
### Requirement
- Create VM Template (with Cloud-init) in Proxmox

### Method
- Packer
	+ https://www.packer.io/download
	+ https://github.com/dustinrue/proxmox-packer

#### Flow
<img width="451" alt="image" src="https://user-images.githubusercontent.com/64636576/147402316-8ea819d0-f76d-4803-8da5-45eac74a6d66.png">


#### Procedure
1) Download Packer & Clone the Repo

2) Ensure iso image uploaded to proxmox datastore

3) Edit packer.json, ensure the following parameter change to your setting
  ```
    "proxmox_iso_pool": "ISO01:iso",
    "centos_image": "rhel-8.4-x86_64-dvd.iso",
  ```
4) Edit kickstart config to include `cloud-init` package. Preparation for Terraform Automation

5) Execute the command
  ```
  ⠵ PACKER_LOG=1 PACKER_LOG_PATH=packer.log \
      packer build \
       -var proxmox_node=pxm \
       -var proxmox_username="root@pam" \
       -var proxmox_password=password \
       -var proxmox_url=https://192.168.0.175:8006/api2/json \
       rocky/packer.json
  ```
6) Example Output
  ```
  proxmox: output will be in this color.

  ==> proxmox: Creating VM
  ==> proxmox: No VM ID given, getting next free from Proxmox
  ==> proxmox: Starting VM
  ==> proxmox: Starting HTTP server on port 8586
  ==> proxmox: Waiting 10s for boot
  ==> proxmox: Typing the boot command
  ==> proxmox: Waiting for SSH to become available...
  ==> proxmox: Connected to SSH!
  ==> proxmox: Provisioning with shell script: /var/folders/ym/lgx89lls259b8f7d1ystpnj80000gn/T/packer-shell4245256413
  proxmox: Removing password for user root.
  proxmox: passwd: Success
  proxmox: Locking password for user root.
  proxmox: passwd: Success
  ==> proxmox: Stopping VM
  ==> proxmox: Converting VM to template
  Build 'proxmox' finished after 6 minutes 20 seconds.

  ==> Wait completed after 6 minutes 20 seconds

  ==> Builds finished. The artifacts of successful builds are:
  --> proxmox: A template was created: 101
  ```
7) Login to Proxmox UI, Click on the newly created template, click `Hardware`, click `Add` -> `Cloudinit Drive`
   <img width="327" alt="image" src="https://user-images.githubusercontent.com/64636576/147410190-c582c5d2-5990-4adf-912a-46ca15b07cf8.png"> <br>
   Create with the following parameter <br>
   <img width="302" alt="image" src="https://user-images.githubusercontent.com/64636576/147410227-0faa283a-290a-472d-8380-e2d61b5015a3.png">

8) Click on `Cloud-Init`, Edit Username for the user you would like to add ssh public key <br>
   <img width="495" alt="image" src="https://user-images.githubusercontent.com/64636576/147410342-464a58bc-2fc0-4666-b3b5-d41020b84265.png"> <br>
   Click on `Regenerate Image` after all configuration done



### Limitation
- RHEL required registering to Subscription Manager for package installation. Hence, package installation was removed in packer & kickstart configuration file.
- Packer initiator needs to have direct ssh communication with the template to avoid ssh timeout
