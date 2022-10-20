The initial steps to work with Terraform and AWS is to create a "main.tf" file inside your root folder.

After populating this file we will run the command:

`terraform init`

> Terraform init should generally be run only once, but in case you make changes to the `terraform {}` then it will be necessary to rerun int.

`terraform plan ` will return a list with a resume of our instance configuration

`terraform apply` will create our instance, it will first show us the whole configuration so we can double check and say "yes", to confirm and actually create our instance

> Everytime we run `terraform apply` it will destroy any machine we have previously created and create a new one, so ATTENTION not to lose any work you might have previously made and remember to use Ansible to avoid this scenario.

# BASIC MAIN.TF FILE EXAMPLE

```HCL
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.27"
    }
  }

  required_version = ">= 0.14.9"
}

provider "aws" {
  profile = "default"
  region  = "us-west-2"
}

resource "aws_instance" "app_server" {
  ami           = "ami-03d5c68bab01f3496"
  instance_type = "t2.micro"
  key_name = "key-pair-name-located-on-your-root-folder"
  tags = {
    Name = "Terraform Ansible Python"
  }
}
```

A `resource` block declares a resource of a given type ("aws\_instance") with a given local name ("app\_server"). The name is used to refer to this resource from elsewhere in the same Terraform module, but has no significance outside that module's scope.

Within the block body (between `{` and `}`) are the configuration arguments for the resource itself. Most arguments in this section depend on the resource type, and indeed in this example both `ami` and `instance_type` are arguments defined specifically for [the `aws_instance` resource type](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance).

## ami:

The AMI can be found in the aws page on EC2>Instances>Launch an Instance

![Screenshot from 2022-10-20 11-33-54.png](:/40c8b87909b64f109f04dd000fda7fbe)

> We have to pay attention as the AMI changes depending on the region

After selecting the type of machine (ubuntu ubuntu for exemple) we will find all the different instances

## Instace types:

![Screenshot from 2022-10-20 11-37-38.png](:/3d53ceb4ed3b4771bd78b0f2a8b7bc2d)

## key_name

We have to add here the name of our AWS Key Pair file which should be located on the root of our project.

To create this Key pair on our EC2 page we have to click on `Key Pairs` which can be found on the left menu under `Network & Security`  , once inside the Key Pair page click on `Create key pair`  give a name for the key pair, select .pem as format, you can now create the key pair, once you conclude the key pair will be automatically downloaded to your machine, you would then have to drag and drop it to the root folder.

## tags = { Name = "something"}

it's used to give a name for this instance, so we can reference later

# Running our first Instance

Once we have created the file above we can then run `terraform plan` to see check our configuration and then `terraform apply` to actually create our instance

We can then access EC2> Instances select the instance we have created and click on `connect` and enter the `SSH client`

## SSH Client

To establish a secure connection between aws and our machine we have to follow the steps provided on the SSH client

> Remember: Ubuntu already has a SSH Client install by default, if using a different OS please make sure you have an SSH client installed 
> 
> IMPORTANT: on step 4 use the long command example

We now follow the instruction on SSH Client page and *bob is your uncle* we are now inside our AWS MACHINE!

### oh no!!! -> I got a connection time out error:

It's most probably cause we haven't configured our Security groups on AWS, so let's do it:

On the EC2 page click on `Security groups` inside `Network & Security` , here we have to configure the `Inbound rules` and `Outbound rules`

- Inbound rules
    - click on edit inbound rules
        - add rule -> select type "All traffic" , origem "Anywhre-IPv4"
        - add rule -> select type "All traffic" , origem "Anywhre-IPv6"

- Outbound rules
    - click on edit outbound rules
        - add rule -> select type "All traffic" , origem "Anywhre-IPv4"
        - add rule -> select type "All traffic" , origem "Anywhre-IPv6"

Once those changes on the Security Group are done we can try once again to access our AWS machina via SSH
