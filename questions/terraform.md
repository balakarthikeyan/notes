📍What is a statefile?

The statefile in Terraform is a JSON file that stores the current state of your infrastructure. It helps Terraform track resources it manages and determine what needs to be added, updated, or destroyed during a terraform apply.

📍Where do you store the statefile?

Store it in a secure, centralized location for collaboration. Common options:

✅ S3 bucket (with versioning and encryption).
✅ Terraform Cloud or Enterprise.

Never store the statefile locally in a team environment.

📍What is a null resource in Terraform?

A null resource doesn’t provision real infrastructure but can run provisioners, scripts, or external commands.

🔹Use case: Triggering operations based on a dependency.