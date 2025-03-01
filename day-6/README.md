# Day 6: Advanced Terraform – Provider Plugins (Managing Keycloak)

## Topics Covered: 
Using Terraform providers for services beyond AWS. Today’s focus is on the Keycloak provider as an example of Terraform’s extensibility. We’ll learn how to use a plugin to configure Keycloak (an open-source Identity and Access Management solution) via Terraform. This demonstrates multi-provider configurations (AWS + Keycloak) and managing application-level settings with Terraform.

## Key Points:

### Terraform Provider Plugins: 
Terraform’s provider ecosystem includes not just cloud providers but many other services (Databases, Kubernetes, DNS, and our case Keycloak). A provider is essentially a plugin that knows how to translate Terraform resources into API calls for a specific system​. By using the Keycloak provider, Terraform can interact with Keycloak’s API to configure realms, clients, roles, etc.​.

### Use Case – Keycloak: 
Keycloak is often configured manually or via scripts. Terraform can automate Keycloak configuration (realms, clients, users) in a repeatable way​. This is powerful for keeping IAM configurations in code. However, note that combining infrastructure provisioning (e.g., creating the server that runs Keycloak) and Keycloak configuration in one Terraform run can be complex. It might be safer to separate them in real projects (to avoid interdependencies issues)​. For learning, we’ll do it together.

### Multi-Provider Config: 
You can use multiple providers in one Terraform project. For example, you might have provider "aws" and provider "keycloak" in the same configuration. You’ll configure the Keycloak provider with the URL of your Keycloak instance and admin credentials to allow Terraform to make changes. Ensure the Keycloak service is running and reachable by Terraform.

### Keycloak Terraform Resources: 
The Keycloak provider (community-supported) offers resources like keycloak_realm, keycloak_openid_client, keycloak_user, etc. You will use these to create or update Keycloak realms and clients. Terraform will handle ordering – e.g., if a client resource depends on a realm, Terraform ensures the realm exists first​.

### Applying Terraform Knowledge: 
Treat Keycloak config just like cloud resources in Terraform. Use variables for names, use outputs if needed, etc. This is a good exercise in seeing Terraform’s HCL apply to any system.

## Hands-On Exercises:
### Set Up Keycloak (Prerequisite): 
If you don’t have a Keycloak instance readily available, start one for testing. The easiest way is via Docker: for example, run docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin bitnami/keycloak:latest. This gives you a local Keycloak at http://localhost:8080 with admin user "admin"/"admin". (Alternatively, if you deployed an EC2 in previous days, you could install Keycloak there, but using Docker locally is quicker for this exercise.)

### Configure the Keycloak Provider: 
In your Terraform config (it could be a separate directory or the same, as you prefer), add the Keycloak provider:
```hcl
provider "keycloak" {
  url      = "http://localhost:8080"         # URL of Keycloak
  client_id = "admin-cli"
  username  = "admin"
  password  = "admin"
  realm     = "master"
}
```

Adjust the credentials to match your Keycloak admin. The above is for a local default setup. Run terraform init to download the Keycloak provider plugin (you may need to specify the provider source, e.g., source = "mrparkers/keycloak" or similar, as per the provider’s docs).

### Terraform Keycloak Resources: 
Write Terraform code to configure Keycloak:
Start by creating a new realm
```hcl
resource "keycloak_realm" "demo_realm" {
  realm   = "demo"    # name of the realm
  enabled = true
}
```
Next, add a client in that realm:
```hcl
resource "keycloak_openid_client" "demo_app" {
  realm_id  = keycloak_realm.demo_realm.id   # reference the realm
  client_id = "demo-app"
  enabled   = true
  direct_access_grants_enabled = true
}
```

Here we see Terraform’s resource referencing: the client resource uses realm_id from the realm we just created, so Terraform knows to create the realm first​
(Optional) Add a user:
```hcl
resource "keycloak_user" "demo_user" {
  realm_id = keycloak_realm.demo_realm.id
  username = "testuser"
  enabled  = true
  initial_password {
    value = "Passw0rd!"
    temporary = false
  }
}
```
This creates a user in the realm with a set password.
### Apply Keycloak Config: 

Run terraform plan and terraform apply. Terraform will connect to Keycloak and create these objects. After apply, log in to the Keycloak Admin Console (or use Keycloak CLI) to verify:
The new realm "demo" should exist.
Inside "demo", a client "demo-app" should be present.
The user "testuser" should be created in the realm. This showcases Terraform managing application configuration just like infrastructure​.

Terraform Destroy (Cleanup): If using a local Keycloak for testing, you can use terraform destroy to remove the realm, client, and user from Keycloak. This is a nice way to clean up your IAM setup. Stop your Keycloak container if you started one.