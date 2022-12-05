# PUTTING IT ALL TOGETHER

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

> If this is your first time, you are required to connect your terraform account with your GitLab account. [*Connect your Here*](https://developer.hashicorp.com/terraform/cloud-docs/vcs/gitlab-com)

![workspace](/images/b.png)

Enter Workspace name and click on `create workspace`

![workspace](/images/5.png)

4. **Configure variables**

Terraform make use of two variable types namely, Terraform variables and Environment variables which needs to be declared or inform terraform cloud.

Terraform or input variables are variables you declare in your terraform code configuration. Luckily, after refactoring our code, we were able to section our variables file to terraform.tfvars. Instead of declaring the variables and vaules manually, we will update the extention to `*.auto.tfvars` to enable terraform cloud to automatically read these values.

Environment variables are available in the Terraform runtime environment. In this case, our environment is AWS.

Next, we need you to configure environemnt variables.

- Click on configure variables
- Click on Add variables
- Specify environment variable radio button
Enter the name and value ( *`Access key id` and `secret key` from your user `AWS user account` and  make sure **sensitive** checkbox is selected for each entry*)

![workspace](/images/3.png)



5. **Run Terrafrom scripts with Packer**

6. **Run terraform plan and terraform apply from web console**

7. **Test automated terraform plan**



