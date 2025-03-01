# Day 4: Reusability with Terraform Modules and AWS Architecture
## Topics Covered: 
Organizing Terraform code with modules for reusability and clarity, using the Terraform Registry to leverage community modules, and building a more complex AWS network (VPC) using modules. We will modularize our configuration and introduce a custom VPC instead of relying on default settings.

## Key Points:
### Terraform Modules: 
Modules in Terraform are like functions or libraries in programming – they package a set of resources for reuse​. Even your root configuration is a module (the "root module"). Using modules encourages DRY code and makes complex configs easier to manage by splitting into logical components​. For example, you might have a module for a VPC, a module for an EC2 + related resources, etc. Terraform Registry offers many pre-built modules by the community.

### Why Modules: 
Modules provide consistency and enforce best practices. You can share modules across projects or with your team. They help encapsulate details – for instance, a well-written VPC module can set up subnets, route tables, and networking basics without you writing it all yourself​.

### AWS VPC (Networking): 
Thus far, we may have been using the default VPC. Today, you’ll create a custom VPC to understand AWS networking. This includes subnets, an internet gateway, route tables, etc. Rather than writing all that from scratch, you can use an official AWS VPC module to simplify the process.

### Module Use vs. Creation: 
You will practice both using a published module (for VPC) and structuring your own configuration as modules. For example, turning your EC2 + security group into a re-usable module might be a good exercise.
Output/Inputs in Modules: Modules take input variables and produce outputs. You pass parameters into a module (e.g., CIDR ranges for VPC, number of subnets) and get outputs (e.g., subnet IDs) that you can use in your root config. We’ll see how to connect modules together using these.

## Hands-On Exercises:

### Use the AWS VPC Module: 
Go to the Terraform Registry and find the AWS VPC module (e.g., terraform-aws-vpc by Terraform AWS modules). In your configuration, instead of manually writing VPC resources:

Add a module "network" block that sources the module (e.g., source = "terraform-aws-modules/vpc/aws" and specify a version).
Provide necessary inputs like VPC CIDR (e.g., 10.0.0.0/16), number of public subnets, etc. Use the module’s documentation as a guide.
The module will create the VPC and subnets. Capture important outputs, e.g., module.network.vpc_id or subnet IDs, to use for other resources.

### Adjust Resources to use VPC Outputs: 
If you still have an EC2 instance from before (or define a new one), modify it to launch in the new VPC. Use the output from the VPC module for subnet_id (perhaps a public subnet) and attach an internet gateway (the module likely did that) so it has internet access. Also, create a new security group in this VPC (or the module may have an output for a default SG).

### Refactor into Modules: 
Try converting part of your configuration into a custom module:
For instance, create a folder ec2-module with its own main.tf, variables.tf, outputs.tf that contains the EC2 instance and its security group configuration. Parameterize it (variables for instance count, AMI, etc.). Then, in your root config, call this module multiple times (you could simulate deploying two similar EC2 setups by calling the module twice with different params, if desired).
This exercise helps understand how to structure Terraform code for reuse. Even if you don’t fully use the module elsewhere this week, the practice is valuable.

### Plan and Apply: 
Run terraform init after adding modules (to download the VPC module). Then terraform plan. The first time using a new module, Terraform will show lots of resources to add (all those inside the module). Apply the changes. Verify that the new VPC and related components were created in AWS (check the VPC section in AWS console for a new VPC ID). Also ensure your EC2 moved into the new VPC/subnet.

### Module Verification: 
If you created a custom EC2 module and used it multiple times, verify that Terraform created multiple instances as expected, each with its own resources. This demonstrates how one module can be instantiated with different configs.

### Clean Up (Selective): 
Decide which infrastructure to keep for later. It might be useful to keep the VPC and one EC2 for the final project. If everything is for learning only, you can destroy and will recreate as needed. However, consider preserving your module code (in version control) for future use.
