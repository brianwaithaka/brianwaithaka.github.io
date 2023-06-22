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
terraform import 
terraform refresh
  
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
Used when beginning work on a new project 

```hcl
terraform init
```

#### terraform init

For a fresh start, download the files as a zip and
```git
git init
```


#### terraform plan

For a fresh start, download the files as a zip and
```git
git init
```

#### terraform apply


For a fresh start, download the files as a zip and
```git
terraform apply -auto-approve
```


#### terraform destroy

For a fresh start, download the files as a zip and
```git
git init
```



#### terraform fmt

For a fresh start, download the files as a zip and
```git
git init
```



#### terraform import 

For a fresh start, download the files as a zip and
```git
git init
```




#### terraform refresh

For a fresh start, download the files as a zip and
```git
git init
```



#### Notes
- If using terraform, do not alter resources outside of the platform e.g in portal or cli 
- It is a declarative style of language based on GO
- When resources are created by Terraform through an ARM template, it cannot manage them

#### Sections of a .tf terraform file 

1. Provider
References the cloud service or other provider
Has optional version info

1.  Data
References to exisitng resources 

1. Resource
Resources managed by terraform

#### Types of terraform files 

1. main.tf file 

   <br>

2. variable.tf file
         - Allows for seperation of environment specific values 
         - That way the main.tf can be version controlled and reused in various env
         - Can be set as part of environment
         - Specify default values, if none user is prompted
         - commandline flag takes precedence, then tf files file, then env variable, then defaultvariables.tf, then lastly prompt

3. output file 
        - Consists of data and messages to be applied post apply command

4. .tfstate file
<br>

#### Credits
Brilliant Youtube lesson by John Savill, [here](https://youtu.be/JKVkblsp3cM).
Official Terraform Documentation, [here](https://developer.hashicorp.com/terraform).
