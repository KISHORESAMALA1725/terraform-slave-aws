Once you create ubuntu vm in aws cloud, please follow the below steps
step1:- 
------------
sudo -i
ADDUSER siva
set password as siva

step2:-
----
 vi /etc/ssh/sshd_config
Ensure the following lines are configured correctly:
2-1 Disable root login:
PermitRootLogin no ( Please please "#" put this )
2-2 Enable password authentication:
PasswordAuthentication yes ( Please remove "#" this
2-3 Restart the SSH service to apply the changes:
sudo service sshd restart

step3:-
------------------
 install java on this vm
sudo apt update -y
apt install openjdk-17-jdk -y

step4:- Please install terraform on this vm
https://developer.hashicorp.com/terraform/install
--------------------------------------------------------
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
-- cross check # terraform -version


step5:- 
------------------ 
login to your jenkins server >> 
manage genkins >>
nodes >> 
hit "+ add node" >> 
node name [terraform-slave-aws] # you can put anyname, here im using the same name to label the node
select "Permanent Agent" option > create 
name [ terraform-slave-aws ]
description [same as above ]
number of executors [ 2 ] # u can put any number , default is 1
remote root directory [ /home/siva/jenkins ] # please create this path in aws slave vm
labels [ terraform-slave-aws ]
usage [ use this node as much as possibel ]
launch method [ launch agents via ssh ]
 ---- host [ give public ip of ur terraform-slave-aws vm ]
 ---- credentials [ please create new credentials ]
 ---- Host Key Verification Strategy [ Non verifying verification strategy ]
availability [ keep the agent online as much as possible ]
save # this is ur last step 

step6:- [ PLEASE PUT THIS FILES IN TERRAFORM-SLAVE-AWS > /home/siva/jenkins/workspaces <here you once you init using the jenkinsfile parameters , you will get the git branch name , put the files here >
------------------
provider.tf:
terraform {
    required_version = "~>1.10.0"
    required_providers {
        aws = { 
            source = "hashicorp/aws"
            version = "~>5.76.0"
        }
    }
}

network.tf
# creating a virtual Private Network #
resource "aws_vpc" "tf_vpc_new"{
    cidr_block = "10.1.0.0/16"
    tags = {
        Name = "Production-VPC"
    }
}

# creating a SUBNET #
resource "aws_subnet" "tf_public_sunbet_1" {
    cidr_block = "10.1.1.0/24"
    vpc_id = aws_vpc.tf_vpc_new.id
    availability_zone  = "us-east-1a"
    tags ={
        Name = "prod-subnet-1"
    }
}

# CREATING INTERNET GATEWAY #
resource "aws_internet_gateway" "tf_vpc_igw" {
    vpc_id = aws_vpc.tf_vpc_new.id
    tags = {
        Name = "prod-igw"
    }
}
 
# creating routing table #
resource "aws_route_table" "tf_rt_public" {
    vpc_id = aws_vpc.tf_vpc_new.id
    tags = {
        Name = "prod-route-table"
    }
}


