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

-- 
# See the aws account what resources are created

* After hit the `terraform apply` its will see the pem keys

1. access to the bastion public ip -> 
```sh
    ssh -i bastion-key.pem ubuntu@<publi-ip>
```

```sh
    sudo apt update
```

2. Please follow the step from the Set Up Terrform Report Backend(Optional) in README.md file