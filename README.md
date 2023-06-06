# VMware vSphere Automation

This document describes the VSphere automation in ROME datacenter. This automation client leverages vSphere Automation SDK for Java, Powershell, VMware PowerCLI and bash scripts. The VM will be spun up based on the selected template which is already created with the prereqisites. It automates the following vSphere admin tasks:
Create VM
  -supports to create Windows10, Windows11, Windows Server 16 and CentOS7
Assign Owner/Assignee to the provisioned VM
Configure Hostname
Check the IP availability
Cofigure IP4 properties
   - Configure StaticIP, Gateway, Subnet mask, Preferred DNS and Alternate DNS

## Steps to build the automation client image and Jenkins job that performs vCenter admin tasks.

1. Checkout [bigfix-docker](git@github02.hclpnp.com:besuem/bigfix-docker.git)
   ```bash
      git clone git@github02.hclpnp.com:besuem/bigfix-docker.git
   ```

2. Execute make command to build vCenter automation client image
   ```bash
      make build-vcenter-automation-client
   ```

3. Run the follwoing Jenkins job to build the vCenter automation client image. The resultant image will be pushed into the JFROG repository as part of this job.
   ```bash
      http://bigfix-jenkins.nonprod.hclpnp.com:8080/view/Infrastructure/job/Bigfix_VCenter_Build_Automation_Client/
   ```

4. Run the follwoing Jenkins job to create a VM, configure hostname and configure IP4 properties:
   ```bash
      http://bigfix-jenkins.nonprod.hclpnp.com:8080/view/Infrastructure/job/Bigfix_VCenter_Create_VM/      
   ```
   
5. Run the following job to delete the VM:
   ```bash
      http://bigfix-jenkins.nonprod.hclpnp.com:8080/view/Infrastructure/job/Bigfix_VCenter_Delete_VM/
