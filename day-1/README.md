# Day 1: Introduction to Terraform and AWS Setup
Topics Covered: What is Infrastructure as Code (IaC) and Terraform, installing Terraform, basic Terraform syntax (HCL), and setting up AWS credentials. We’ll also run through the Terraform workflow (init, plan, apply) for the first time​.

## Key Points:
### Why Terraform & IaC: 
Terraform is an IaC tool that lets you define cloud infrastructure as code. This means you can use familiar coding practices (version control, code review) to manage infrastructure​
It’s cloud-agnostic, so the same tool works for AWS, Azure, etc., but we'll focus on AWS here.

### Terraform Basics: 
Terraform configurations are written in HashiCorp Configuration Language (HCL). You declare the desired state of infrastructure, and Terraform creates or updates resources to match that state. Key commands are terraform init (initialize project/plugins), terraform plan (preview changes), and terraform apply (execute changes)​

### Providers: 
Terraform uses providers (plugins) to interact with cloud platforms and services​. For AWS, the AWS provider translates your HCL into AWS API calls​. You’ll configure the AWS provider with your credentials so Terraform can create AWS resources.

### State: 
Terraform records information about resources in a state file. The state maps your config to real AWS resource IDs​. This is how Terraform knows what’s already been created and plans only necessary changes.

## Hands-On Exercises: 
(Ensure you have an AWS account and AWS CLI or access keys set up before starting.)
### Install Terraform: 
Follow the official instructions to install Terraform on your system (e.g., using the HashiCorp repository or downloading the binary)​. Verify by running terraform -v.

### AWS Credentials: 
Configure AWS access for Terraform. For example, set environment variables AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY, or set up the AWS CLI and ensure credentials are in ~/.aws/credentials​

### Write First Terraform Config: 
In a new directory, create a main.tf file with a basic configuration:
    * Add a provider block for AWS (specify region, etc.).
    * Add a simple resource. For example, an AWS EC2 instance or S3 bucket. (Tip: start with a resource that’s free-tier, like an S3 bucket, to avoid costs.)
    * Example (for EC2): Define resource "aws_instance" "test" with an AMI ID (e.g., Amazon Linux) and instance type (e.g., t2.micro). For S3: resource "aws_s3_bucket" "test" { bucket = "<unique-name>" acl = "private" }.
### Initialize and Apply: 
Run terraform init to download the AWS provider plugin. Then run terraform plan to review the planned creation. Finally, run terraform apply and type "yes" to create the resource. Terraform will call AWS APIs to provision the infrastructure​.

### Verify in AWS: 
Log in to the AWS Console to see the created EC2 instance or S3 bucket. This confirms Terraform is working. Observe that Terraform created a file terraform.tfstate in your directory – this is the state file recording the resource details.
Terraform Destroy (Cleanup): As a best practice, if this is just a test, run terraform destroy to tear down the resources when you’re done, to avoid ongoing costs.
