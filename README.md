# terraform-foreman
Foreman provider for Terraform - !Please note that this is by no means production ready!

** Note, I am a sysops guy and have never touched golang in my life. I started this project to see if I could provision machines in foreman with terraform. 

## What is Terraform
Terraform is an orchestration tool that can be used to deploy and manage the lifecycle of cloud resources such as virtual machines, DNS records etc.
That probably isn't giving it enough credit so check it out at https://terraform.io/

## What is Foreman
Foreman (or more accurately TheForeman) is a Red Hat sponsored open source tool used to manage infrastructure in a Private Cloud. Foreman can provision for instance Docker instances, Virtual Machines in OpenStack or VMWare and it can even deploy to bare metal. Foreman ties into PuppetLabs Puppet infrastructure and provides ENC data regarding the servers it manages. Check it out at http://theforeman.org/

## Usage


Add the following to your ~/.terraformrc
```
providers {
     foreman = "/path/to/bin/terraform-provider-foreman"
}
```

Sample terraform config. Save this as a example.tf or similar
```

resource "foreman_server" "complex_VM" {
    name = "complex.example.com"
    location_id = 1		# ID of foreman location, if locations are enabled
    organization_id = 2		# ID of foreman organization, if orgs are enabled
    environment_id = 4		# ID of foreman environment
    ip = "10.12.2.2"
    mac = "00:50:56:8f:29:73"
    architecture_id = 1 	# x86_64
    domain_id = 1		# "First foreman domain"
    puppet_proxy_id = 2		# ID of your puppet master defined in foreman
    puppet_class_ids = ["351"]  # Puppet class ids
    operatingsystem_id = 1	# ID of a foreman operating system
    medium_id = 7		# ID of a foreman repository
    ptable_id = 3		# ID of a foreman partition table
    subnet_id = 3		# ID of a foreman subnet
    compute_resource_id = 1	# ID of a foreman compute resource, for example vCenter, docker, openstack nova
    model_id = 1		# ID of a foreman compute model
    owner_id = 17		# ID of a foreman user, likely that of the service account used to create it
    puppet_ca_proxy_id = 2	# ID of your puppet CA master defined in foreman
    build = false		# Build mode, determines if the PXE configs are generated by foreman
    enabled = true		# Enabled?? NFI
    provision_method = "build"  
    managed = true		
    comment = ""
}
```

This will interrogate the provider and should output something similar to
**TODO**


if you have already planned the terraform resources you can taint them essentially marking them to be rebuilt

Now you can write your own plan similar to the example. Reference the terraform documentation at https://terraform.io/intro/getting-started/build.html

Once a plan has been created you are ready to apply the plan and actually deploy. 

**So far I have implemented this as a simple call to the hammer command. Terraform providers are typically fully CRUD designed but so far I have only made the create functionality.**

```
# In the directory of your my_custom_terraform.tf file
terraform apply
```
This will interrogate the provider and make the changes. If a server isn't built for instance it will call the create method and the provider will instantiate the resource.

## How to Build the Source
In order to build/install the source, navigate to the checked out directory and ensure your $GOPATH is defined.

Execute
```
go build





