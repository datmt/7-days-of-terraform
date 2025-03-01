# Day 2: Terraform Fundamentals – State, Variables, and Outputs
Topics Covered: Deep dive into Terraform state and how Terraform tracks resources, using variables for configuration, and output values. We’ll refactor yesterday’s config to use variables, and explore the state file content.

## Key Points:
### Terraform State: 
The state file is Terraform’s database of your infrastructure​. It maps resource definitions to actual AWS resource IDs (e.g., mapping your aws_instance.test to an EC2 instance ID)​. This allows Terraform to know what exists so it can create, update, or destroy resources as needed. Never edit state manually; use Terraform commands to manipulate it if required.

### Variables: 
Instead of hardcoding values (like AMI IDs or bucket names), Terraform supports input variables. Variables make configurations reusable and flexible. You can define variables in a variables.tf file and provide their values via .tfvars files or CLI. For example, a variable for AWS region or instance type can be declared and then referenced as var.region in your config​.

### Outputs: 
Output values let Terraform expose information from your config after apply (e.g., the EC2 instance’s public IP or the S3 bucket URL). Outputs are useful for quickly retrieving resource attributes or passing data to other modules/stages​.

### Terraform Plan/Apply Cycle: 
Reiterate how plan shows changes. If you change a variable value or resource config, terraform plan will show the diff. Terraform’s execution plan will create/update only what's needed. For example, adding a new resource yields a plan to add it without affecting existing ones, thanks to state tracking.

## Hands-On Exercises:
### Examine State: 
Open the terraform.tfstate file generated on Day 1 (or use terraform show). Notice it’s JSON containing your resource and its attributes. Understand that this file represents the current state of your infrastructure as Terraform sees it.

### Introduce Variables: 
Refactor your Day 1 configuration to use variables:
* Create a file variables.tf. Define variables for values like region, instance_type, or any repeated values in your config.
* In main.tf, replace hardcoded values with references to var.<name>. For example, ami = var.ami_id and define variable "ami_id" { type = string } in variables.tf.
* Provide variable values. Create a terraform.tfvars or use -var flags when running plan/apply.

### Add Outputs: 
In a new file outputs.tf, define some outputs to get useful info. For an EC2 instance, output its public IP:

```hcl
output "instance_ip" {
  value = aws_instance.test.public_ip
}
```

For S3, output the bucket ARN or name. After apply, test terraform output instance_ip to see the value.

### Apply Changes: 
Run terraform plan – you should see no changes except perhaps reordering if any (because you only refactored to use vars/outputs without changing actual values). Then run terraform apply to reflect any refactoring (Terraform might refresh state). The resources should remain, and you should get the defined outputs printed.

### Resource Dependencies: 
Try adding a second resource and see Terraform handle dependencies. For example, add an EIP (Elastic IP) and reference it in your EC2 (or add an S3 bucket now if you did EC2 on Day 1). Terraform will automatically create the independent resource first if needed and then the dependent one. You can observe this in the plan ordering​.

### State Changes: 
Experiment by manually changing something in AWS (e.g., rename the S3 bucket via console) and then run terraform plan. Terraform will detect the drift and plan to correct it. (Be cautious: external changes can lead to differences Terraform will want to fix or could confuse things if not managed.)
