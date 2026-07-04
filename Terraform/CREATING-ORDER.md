## Order of creating

1. Terraform.tf
2. Data.tf
3. vpc.tf
4. bastion-ec2.tf
5. eks.tf
6. outputs.tf

# after Setup

1. Please create new user in AWS account (root)
2. Then create the access key and then configure with aws in our local machine
3. Then go to project and in terraform file,  hit the `terraform init` 
4. Then hit the `terraform plan`
5. then hit the `terraform apply`