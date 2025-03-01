# Day 3: AWS Essentials – EC2, S3, and IAM Basics
## Topics Covered: 
Key AWS services with Terraform – provisioning EC2 instances (compute), managing security groups for networking, creating an S3 bucket, and an introduction to IAM (Identity & Access Management) resources. We’ll build a slightly richer AWS configuration and understand how Terraform handles multiple resources.

## Key Points:
### AWS Provider Recap: 
Ensure your AWS provider is configured (region, profile, etc.) in Terraform. Terraform will manage all specified AWS resources in your config each apply.

### EC2 Instances: 
EC2 is a core AWS service for virtual machines. With Terraform, you can define everything about an instance (AMI ID, instance type, key pair, user data, etc.). You can also define security groups to control inbound/outbound traffic for the instance.

### Security Groups (Networking): 
A security group is like a virtual firewall for your instance. In Terraform, aws_security_group resource can specify rules (e.g., allow SSH on port 22, allow HTTP on port 80) and be attached to your EC2. This covers basic networking needs.

### S3 Buckets: 
S3 provides object storage. Terraform can create buckets easily. You might use S3 for static website hosting, backups, or (in a later day) to store Terraform state. Remember to use globally unique bucket names.

### IAM: 
Terraform can manage IAM users, roles, and policies. We will introduce an IAM role for EC2. An IAM role can grant your EC2 instance permissions (for example, to access S3). Attaching an IAM role to an EC2 instance is done via an instance profile. This ensures any applications on the instance can use AWS APIs securely (avoiding embedding AWS keys on the VM).

## Hands-On Exercises:
### Extend Configuration: 
Build on your config (or start a new one) to include the following AWS resources:
* An AWS Security Group that allows SSH access (port 22) and HTTP (port 80) from your IP or universally (for simplicity in a dev environment). Use aws_security_group and aws_security_group_rule resources.
* An AWS EC2 Instance (if you haven’t already) using the security group above. Reference the security group’s ID in the instance’s vpc_security_group_ids. Also, if you have an existing key pair for SSH, specify it (or create one outside Terraform and reference it via the key_name attribute).
* An AWS S3 Bucket for practice. Give it a unique name (e.g., <your-initials>-tf-state-bucket) because S3 bucket names must be global. We will use this bucket later for remote state, but create it now with Terraform.
* (Optional) IAM Role for EC2: Create an aws_iam_role with a policy that allows read/write to your new S3 bucket. Then create an aws_iam_instance_profile for that role, and attach it to your EC2 instance (use the iam_instance_profile attribute in aws_instance). This way, the EC2 can access the S3 bucket without manual credentials.

### Use Variables for Configurability: 
For any new resource, consider adding variables (for example, a variable for allowed SSH IP, so you can easily change it).

### Apply Incrementally: 
Run terraform plan to see the addition of multiple new resources. Terraform will create all new resources in a single apply, but it determines the correct order based on dependencies (e.g., it will create the IAM role before creating the instance that depends on it, and create the security group before applying it to the instance).

### Test the Setup:
* SSH into the new EC2 instance (if you provided a key name and your IP in the SG). Verify you can connect, which confirms the security group rule.
* If you attached the IAM role and S3, try from the EC2: aws s3 ls s3://<your-bucket> (after installing AWS CLI on the instance) to verify the instance’s IAM role has access.
* For a more visual test, you could add a simple user data script to the EC2 to install a web server (e.g., Apache) and serve a page. This can be done by adding user_data to aws_instance (with a bash script that installs and starts httpd). Then visit the EC2’s public IP in a browser. (This step is optional and demonstrates provisioning software on the VM via Terraform.)
* Review in AWS Console: Check the AWS console for EC2, S3, and IAM. You should see the instance running, the security group attached, the S3 bucket created, and the IAM role in place (if done). This multi-resource deployment shows how Terraform can manage a broad set of AWS services in one configuration.
* Cleanup if Needed: If this is purely a learning exercise and might incur costs, you can terraform destroy these resources. However, note which pieces you might want to keep for tomorrow (e.g., the S3 bucket will be used on Day 5 for remote state, so you might keep that even if you destroy the EC2).