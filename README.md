# VMware vSphere Automation

## Abstract

This document describes the vSphere automation in the ROME datacenter. The automation client leverages vSphere Automation SDK for Java, PowerShell, VMware PowerCLI, and Bash scripts. It automates various vSphere admin tasks, including creating VMs, assigning owners/assignees, configuring hostnames, checking IP availability, and configuring IP4 properties (static IP, gateway, subnet mask, preferred DNS, and alternate DNS).

## Steps to Build the Automation Client Image and Jenkins Job for vCenter Admin Tasks

### Build Automation Client

1. Run the following Jenkins job to build the vCenter automation client image and push it to the JFROG repository:
   
   Jenkins job URL: [http://sup.jenkins.dev.com/vm/job/Jenkins_job_Automation_Client/](http://sup.jenkins.dev.com/vm/job/Jenkins_job_Automation_Client/)

   This job performs the following tasks:

   1.1. Checkout [bigfix-docker](git@github.com:prog/vm-automation.git) repository:
   
       ```bash
       git clone git@github.com:prog/vm-automation.git
       ```

   1.2. Execute the make command to build the vCenter automation client image:
   
       ```bash
       make build-automation-client
       ```

   1.3. Tag the build number with the Docker image and push it to the JFROG Docker registry.

### Create VM, Configure Hostname, and Configure IP4 Properties

2. Run the following Jenkins job to create a VM, configure the hostname, and configure IP4 properties:
   
   Jenkins job URL: [http://sup.jenkins.dev.com/vm/job/Jenkins_job_Create_VM/](http://sup.jenkins.dev.com/vm/job/Jenkins_job_Create_VM/)

   This job performs the following tasks:
      
   2.1. Remove the automation container if it already exists.
   
   2.2. Pull the latest image from the JFROG Docker registry.
   
   2.3. Spin up the container with the necessary environment properties.
   
   2.4. Check the IP availability. The job will mark it as a failure in case of an IP conflict.
   
   2.5. Create a VM based on the selected template, configure the hostname, and configure IP4 properties (static IP, gateway, subnet mask, preferred DNS, and alternate DNS):
   
       ```bash
       docker exec vcenter /app/scripts/create-vm.sh
       ```

### Delete VM

3. Run the following Jenkins job to delete a VM:
   
   Jenkins job URL: [http://sup.jenkins.dev.com/vm/job/Jenkins_job_Delete_VM/](http://sup.jenkins.dev.com/vm/job/Jenkins_job_Delete_VM/)

### Update Hostname and Static IP

4. Run the following Jenkins job to update the hostname and static IP for a given VM:
   
   Jenkins job URL: [http://sup.jenkins.dev.com/vm/job/Jenkins_job_Update_VM/](http://sup.jenkins.dev.com/vm/job/Jenkins_job_Update_VM/)

