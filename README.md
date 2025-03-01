# 7 Days of Terraform
This is my personal plan to quickly learn Terraform for my job.

## Terms
| Term | Description |
|------|------------|
| **Provider** | A plugin that enables Terraform to interact with external APIs (e.g., AWS, Azure, GCP). |
| **Resource** | A declarative representation of infrastructure components (e.g., VM, database, API gateway). |
| **Data Source** | Read-only information retrieved from an external API, used for lookups (e.g., latest AWS AMI). |
| **State** | A JSON file that tracks the infrastructure Terraform manages, stored locally or remotely. |
| **Backend** | Where Terraform stores its state (e.g., local files, S3, Terraform Cloud). |
| **Module** | A reusable collection of Terraform configurations that can be shared and used like a function. |
| **Variable** | User-defined values that make Terraform configurations dynamic and reusable. |
| **Output** | A way to return values from a Terraform configuration (e.g., an IP address of a created VM). |
| **Plan** | A command (`terraform plan`) that shows the changes Terraform will make before applying them. |
| **Apply** | A command (`terraform apply`) that makes the actual changes to the infrastructure. |
| **Destroy** | A command (`terraform destroy`) that removes all managed infrastructure resources. |
| **Workspace** | An isolated state environment that allows managing multiple configurations with the same code. |
| **Locking** | Ensures only one process modifies the Terraform state at a time (important for team environments). |
| **Provisioner** | Runs scripts or commands on a resource after creation (e.g., installing software on a VM). |
| **Lifecycle Rule** | Defines behaviors such as `prevent_destroy`, `ignore_changes`, and `create_before_destroy`. |
| **Meta-Arguments** | Special arguments like `depends_on`, `count`, and `for_each` that modify resource behavior. |
| **Implicit Dependency** | Automatically determined dependencies based on resource references. |
| **Explicit Dependency** | Manually defined dependencies using `depends_on` to ensure correct execution order. |
| **Import** | A command (`terraform import`) that brings an existing resource into Terraform state management. |
| **Refresh** | A command (`terraform refresh`) that updates the Terraform state to match real-world resources. |
| **Drift** | A mismatch between the Terraform state and actual infrastructure due to external changes. |
| **Remote State** | Stores Terraform state in a remote backend (e.g., S3, Terraform Cloud) for team collaboration. |
| **Terraform Cloud** | A hosted service providing remote state management, collaboration, and automation. |
| **Custom Provider** | A user-created provider written in Go to manage non-standard infrastructure. |
| **Graph** | A visual representation of resource dependencies, viewable with `terraform graph`. |


https://chatgpt.com/c/67c1b83a-35a8-8013-9b35-40832b1a1bd1