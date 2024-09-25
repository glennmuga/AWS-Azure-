# Terraform :

## create an ec2 instance 
## open the terminal 
## go on install teraform copy and paste the commands 
sudo yum install -y yum-utils shadow-utils

sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo

sudo yum -y install terraform

terraform -v

## Amazon liux
## *install cli on terminal*
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

unzip awscliv2.zip

sudo ./aws/install

## aws configure 

>> aws provider terraform website
mkdir /proj

cd proj 

vim provider.tf

>> paste the provider commands

tree -a

terraform init

l.

cd /proj

ll

> aws_instance resources

## go on vim file 

## my ec2 code 
## paste the aws_instance resources
provider "aws" {
  region     = "us-east-1"
  access_key = ""
  secret_key = ""
 
}
 
resource "aws_instance" "my-server" {
  ami                     = "ami-0ebfd941bbafe70c6"
  instance_type           = "t2.micro"
  key_name                = "terr"
}

name the server 

change ami image copy the ami in new region.

instance type t2.micro

delete host resource group

key_name = "keyname of new region "

## Terraform init  
ll
terraform validate

terraform plan (just gives us what all will be executed )

terraform apply  (will create infra on aws cloud ) 

 >>>> an instance will be created on your aws

>> try to connect it with the terminal

Add ssh in sec groups of the instance

yum install httpd -y

cat provider.tf 

## do aws configure on old terminal

 cd /proj
 
## remove the first { } from vim provider.tf

terraform destroy

ll

add new tag to instance 

terraform get provider.tf

terraform fmt 

terraform plan

terraform apply 

terraform destroy 

## Go on the old terminal again 
 cd /proj

 mkdir /glenn-data

 cd /proj

cp provider.tf /glenn-data

cd /glenn-data

mv my-resource.tf

mv provider.tf my-resource.tf

vim my-resource.tf

## add availabilty zone 

avaialbility_zone = 

vim my-resource.tf

## add secuity group code 

## copy from the aws documentation >> vpc >> aws_secuirty_group  

tags =  
## change port 
from and to = 80

allow_http_ipv4 , allow_tls_ssh

port 22 and 22
web-serever-sg
remove last rule 

comment all cidr

#cidr

wq!

tree -a

terraform init

terraform fmt 

terraform validate 

terraform apply
