# AWS automation with Ansible

This playbook installs and configures a three tier architecture for creating a VPC, subnets, internet_gatway, elb, ec2 instances in Amazon Web services. It contains a web layer, an application layer and a database layer. Assume, There are two EC2 instances running in two different availability zones in the web layer and the same for the application layer as well.

vars_prompt used to ask certaince information for creating AWS services.
The playbook accepts inputs given by the user such as names, CIDR blocks, tags etc and then builds the VPC in AWS. This is still a work in progress.

The main playbook I have created is "arch.yml" which calls all other play-books to configure the different components. Each playbook calls the concerned role to execute the task and configure the required component. The different roles used are:
1) vpc - It takes parameters such as name of the VPC, the CIDR block, etc as inputs from the user and then configures the VPC based on these inputs.

2) igw - This role is used to set-up the internet gateway for the VPC.

3) subnet - The web layer has two subnets and application have two subnets. Each in two different availability zones and database layer has a subnet all of which are private. The load balancer has two subnets in the two availability zones which is same as the zones for web layer.

4) nat- This role is used to create a NAT gateway for the VPC. In which, web and db is connected

5) routetable - This role is used to create the routetable for the different associated subnets in the VPC.

6) rds_subnet_group - This role is used to create a RDS subnet group so that we can configure a realtional database service within the VPC.

7) rds- This role is used to create a RDS instance. The concerned playbook accepts inputs from the user to determine the type of RDS, instance, size, username etc which passes the inputs to the role so that it can configure it accordingly.

8) security_group- The different roles used to create security groups are app_sg, db_sg and wb_sg. The playbooks associated with each role accepts the required inputs and passes it to the concerned role.

9) ec2 - This role is used to create EC2 instances for the web and application layer. As we have seen in the case of "subnet" roles, the different playbooks accept inputs from the user and then they call the "ec2" role which configures the required instance in the VPC.

10) elb- This role is used to create a load balancer to manage the traffic to the web layer.

The roles invoke the AWS modules in Ansible to carry out their respective tasks.

## Running the playbook
Open the terminal and change the path to where you have downloaded the folder.
Then run the following command in the terminal:
> $ anisble_playbook arch.yml

## Running the playbook
Please refer my LAMP_Stack_Ansible to install LAMP Stack

## Requirements
You must have AWS CLI, latest Python module and boto3 installed before running this playbook else it will result in an error saying you need one of these installed.
