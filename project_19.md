# PUTTING IT ALL TOGETHER

In this articule, we will be puting it all together from part one. I will also introduce what Terraform calls Remote Operations. Simply put, Remote Operations are terraform runs managed by Terraform Cloud.

1. **Create a Terraform Cloud account**

For this project we will use a free account. Register an Terraform account [here](https://app.terraform.io/signup/account)

![terraform account](/images/1.png)

2. **Create an organization and configure workspace**

To create a blank organization, `click on Start from Scratch`

Enter the organization name and confirm email address and click on `create organization`
![workspace](/images/3.png)

Workspaces determine how Terraform Cloud organizes infrastructure. A workspace contains your Terraform configuration (infrastructure as code), shared variable values, your current and historical Terraform state, and run logs.

Next, create a workspace using `version control workflow`

![workspace](/images/2.png)

Select your preferred version control workflow, in this case, I have selected Gitlab
![workspace](/images/4.png)

4. **Configure variables**

5. **Run Terrafrom scripts with Packer**

6. **Run terraform plan and terraform apply from web console**

7. **Test automated terraform plan**



