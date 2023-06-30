---
title: Terraform 
tags: [Terraform, Cheat Sheet, Commands]
style: border
color: success
description: My understanding of Terraform Infrastructure as Code (IAC)Platform
---

## Introduction <!-- omit from toc --> 
Hello. This is my terraform cheat sheet for me to easily refer to. 

Over time I will append links to official documentation. If something does not look right, [please reach out](mailto:brian@waithaka.me).

---

{% capture list_items %}
terraform init
terraform plan
terraform apply 
terraform destroy
terraform fmt
terraform validate
terraform import 
terraform output
terraform refresh
terraform state
  
{% endcapture %}
{% include elements/list.html title="Table of Contents" type="toc" %}


{% capture list_items %}

Notes
Sections of a .tf terraform file 
Types of terraform files 
Credits
  
{% endcapture %}
{% include elements/list.html title="Notes Section" type="toc" %}


#### terraform init
Initializes terraform by downloading required plugins defined in .tf file 

```hcl
terraform init
```

Initializes terraform and skips downloading required plug ins 
```hcl
terraform init -get-plugins=false
```


#### terraform plan

For a fresh start, download the files as a zip and
```hcl
terraform plan
```

#### terraform apply

Creates an execution plan through which you can preview infrastructure changes. Compares it to the prior state
```hcl
terraform apply -auto-approve
```


#### terraform destroy

Tear down all managed resources and infrastructure
```hcl
terraform destroy
```
You can also run apply command but with -destroy flag, 
```hcl
terraform apply -destroy
```

#### terraform fmt
Formats the code in all .tf files as per HCL standards
```hcl
terraform fmt
```

#### terraform validate
Validates code in all .tf files in terms of syntax/correctness
```hcl
terraform validate
```

#### terraform import 
Used to add existing resources to the state file for management by terraform. Avoid usage.
```hcl
terraform import <resource_type>.<resource_name> <resource_id>
```

#### terraform output 
Lists all outputs from the session. Add -json flag for JSON format
```hcl
terraform output && terraform output -json
```

#### terraform refresh
Reads current settings from managed resources and updates Terraform state file
```hcl
terraform refresh

#Carries out the same action as below code
terraform apply -refresh-only -auto-approve
```

#### terraform state
Lists out all the resources currently tracked by the state file
```hcl
terraform state list
```

Allows you to view the attributes of a single resource in the state file 
```hcl
terraform state show <resource>
```

Stops tracking a resource present in the state file
```hcl
terraform state rm  aws_instance.myinstance
```

#### other commands
Check current version and update
```hcl
terraform version && terraform get -update=true 
```


#### Notes
- If using terraform, do not alter resources outside of the platform e.g in portal or cli 
- It is a declarative style of language based on GO
- When resources are created by Terraform through an ARM template, it cannot manage them
- Terraform also automatically loads a number of variable definitions files if they are present:
        - Files named exactly terraform.tfvars or terraform.tfvars.json.
        - Any files with names ending in .auto.tfvars or .auto.tfvars.json.

#### Sections of a .tf terraform file 

1. Provider
         - References the cloud service or other provider
         - Has optional version info

2.  Data
        - References to exisitng resources 

3. Resource
         - Resources managed by terraform

#### Types of terraform files 

1. main.tf file 

2. .tfvars file
         - Allows for seperation of environment specific values 
         - That way the main.tf can be version controlled and reused in various env
         - Can be set as part of environment
         - Specify default values, if none then user is prompted
         - commandline flag takes precedence, then tf files , then env variable, then default variables.tf, then lastly prompt

3. output file 
        - Consists of data and messages to be displayed after terraform apply command. Can also be queried using [terraform output](#terraform-output)

4. .tfstate file
        - Is managed by terraform and consists of a snapshot of managed infrastructure and configuration.
<br>


#### Credits
Brilliant Youtube lesson by John Savill, [here](https://youtu.be/JKVkblsp3cM).
Official Terraform Documentation, [here](https://developer.hashicorp.com/terraform).
