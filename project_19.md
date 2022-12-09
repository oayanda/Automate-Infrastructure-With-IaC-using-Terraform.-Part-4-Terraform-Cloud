# PUTTING IT ALL TOGETHER

This tutorial assumes that you are continuing from the [previous tutorials.](https://github.com/oayanda/AUTOMATE-INFRASTRUCTURE-WITH-IAC-USING-TERRAFORM.-PART-3---REFACTORING)

In this articule, we will be puting it all together from part one. I will also introduce what Terraform calls Remote Operations. Simply put, Remote Operations are terraform runs managed by Terraform Cloud.

1. **Create a Terraform Cloud account**

For this project we will use a free account. Register an Terraform account [here](https://app.terraform.io/signup/account)

![terraform account](/images/1.png)

2. **Create an organization and configure workspace**

To create a blank organization, `click on Start from Scratch`

Enter the organization name and confirm email address and click on `create organization`

![workspace](/images/a.png)

Workspaces determine how Terraform Cloud organizes infrastructure. A workspace contains your Terraform configuration (infrastructure as code), shared variable values, your current and historical Terraform state, and run logs.

Next, create a workspace using `version control workflow`

![workspace](/images/2.png)

Select your preferred version control workflow, in this case, I have selected GitLab.

> If this is your first time, you are required to connect your terraform account with your GitLab account. [*Connect your account Here*](https://developer.hashicorp.com/terraform/cloud-docs/vcs/gitlab-com)

![workspace](/images/b.png)

Enter Workspace name and click on `create workspace`

![workspace](/images/5.png)

3. **Configure variables**

Terraform make use of two variable types namely, Terraform variables and Environment variables which needs to be declared or inform terraform cloud.

Terraform or input variables are variables you declare in your terraform code configuration. Luckily, after refactoring our code, we were able to section our variables file to terraform.tfvars. Instead of declaring the variables and vaules manually, we will update the extention to `*.auto.tfvars` to enable terraform cloud to automatically read these values.

Environment variables are available in the Terraform runtime environment. In this case, our environment is AWS.

Next, we need you to configure environment variables.

- Click on configure variables
- Click on Add variables
- Specify environment variable radio button
Enter the name and value ( *`AWS_ACCESS_KEY` and `AWS_SECRET_KEY` from your user `AWS user account` and  make sure **sensitive** checkbox is selected for each entry*)

![workspace](/images/c.png)

4. Packer

Packer is an open source tool for creating identical machine images for multiple platforms from a single source configuration. Packer is lightweight, runs on every major operating system, and is highly performant, creating machine images for multiple platforms in parallel.

```bash
# Install Packer

choco install packer
```

> Ensure Chocolatey package manager is installed.
[Click Here](https://docs.chocolatey.org/en-us/choco/setup)

![workspace](/images/7.png)

5. Create Packer Template and Build AMIs**

A Packer template is a configuration file that defines the image you want to build and how to build it. Packer templates use the Hashicorp Configuration Language (HCL).

Create packer templates for `Bastion`, `nginx` and `web application`(tooling and wordpress) with extention `*.pkr.hcl` and save the required scripts in `.sh` file for each for the templates.

*Make sure the files are available in the same directory*
*Script for the tooling and wordpress*

Get the available ami in your available zone.

```bash
# List available RHEL 8 AMIs in us-east-1 according to Name, ImageId and Creation Date. Select the required one that matches your instance type

aws ec2 describe-images --owners 'amazon' --filters 'Name=name,Values=RHEL-8*' --query 'sort_by(Images, &CreationDate)[*].[Name,ImageId,CreationDate]' --region us-east-1 --output table
```

![amis](/images/d.png)

Packer template for Web application

```bash

variable "region" {
  type    = string
  default = "us-east-1"
}

locals { timestamp = regex_replace(timestamp(), "[- TZ:]", "") }


# source blocks are generated from your builders; a source can be referenced in
# build blocks. A build block runs provisioners and post-processors on a
# source.

source "amazon-ebs" "terraform-web-prj-19" {
  ami_name      = "terraform-web-prj-19-${local.timestamp}"
  instance_type = "t2.micro"
  region        = var.region
  source_ami_filter {
    filters = {
      name                = "RHEL-8.3_HVM-20210209-x86_64-0-Hourly2-GP2"
      root-device-type    = "ebs"
      virtualization-type = "hvm"
    }
    most_recent = true
    owners      = ["amazon"]
  }
  ssh_username = "ec2-user"
  tag {
    key   = "Name"
    value = "terraform-web-prj-19"
  }
}


# a build block invokes sources and runs provisioning steps on them.
build {
  sources = ["source.amazon-ebs.terraform-web-prj-19"]

  provisioner "shell" {
    script = "web.sh"
  }
}
```

Run Packer Build 

```bash
# Run Packer Build to get your Custom AMI ID

packer build web.pkr.hcl
packer build bastion.pkr.hcl
packer build nginx.pkr.hcl
```
![custom-ami](/images/9.png)

Verify in AWS Ami dashboard

![custom-ami](/images/10.png)

Update the AMis in the variable file `terraform.auto.tfvars`

![custom-ami](/images/11.png)


6. **Run terraform plan and terraform apply from web console**

Commit and push updated code to GitLab. This triggers a CI/CD pipeline that runs a Terraform  Plan in Terraform Cloud.

![custom-ami](/images/12.png)

 Automatic Trigger of Terraform Plan on Terraform Cloud Console

![custom-ami](/images/e.png)

7. **Test automated terraform plan**

Confirm and Apply the Plan

