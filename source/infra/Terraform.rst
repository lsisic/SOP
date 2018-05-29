Terraform
=========

Deploying your first terraform configuration
--------------------------------------------

Terraform components:

- Terraform executable written in GO
- Terraform file(s)
- Terraform state file 
- Terraform preferences

**Variables**

``variable "aws_access_key" {}``
``variable "aws_secret_key" {}``

**Provider**

.. code-block:: bash

 provider "aws" {
	access_key = "access_key"
	secret_key = "secret_key"
	region = "us-east-1"
	
 }
 
**Resource**

.. code-block:: bash

 resource "aws_instance" "ex" {
	ami = "ami-c58c1dd3"
	instance_type = "t2.micro"
 }
 
**Output**

.. code-block:: bash

 output "aws_public_ip" {
	value = 
	"${aws_instance.ex.public_dns}"
 }
 

 
Demo time
^^^^^^^^^

- AWS acconut
- Terraform software
- Demo files

``moduleone.tf``

.. code-block:: bash

 ##################################################################################
 # VARIABLES
 ################################################################################## 
 
 variable "aws_access_key" {}
 variable "aws_secret_key" {}
 variable "private_key_path" {}
 variable "key_name" {
   default = "PluralsightKeys"
 } 
 
 ##################################################################################
 # PROVIDERS
 ################################################################################## 
 
 provider "aws" {
   access_key = "${var.aws_access_key}"
   secret_key = "${var.aws_secret_key}"
   region     = "us-east-1"
 } 
 
 ##################################################################################
 # RESOURCES
 ##################################################################################
 
 resource "aws_instance" "nginx" {
   ami           = "ami-c58c1dd3"
   instance_type = "t2.micro"
   key_name        = "${var.key_name}" 
 
   connection {
     user        = "ec2-user"
     private_key = "${file(var.private_key_path)}"
   } 

   provisioner "remote-exec" {
     inline = [
       "sudo yum install nginx -y",
       "sudo service nginx start"
     ]
   }
 }
 
 ##################################################################################
 # OUTPUT
 ##################################################################################
 
 output "aws_instance_public_dns" {
     value = "${aws_instance.nginx.public_dns}"
 }



We are in **GettingStarted/ModuleOne/** and ``ls -la``  show us : moduleone.tf , terraform.tfstate, terraform.tfstate.backup files.

``terraform plan -var-file='../terraform.tfvars'``

Look in to the current aws state and tell us what will be changed.

``terraform apply -var-file='../terraform.tfvars'``

With **apply**  terraform change aws state.

``terraform destroy -var-file='../terraform.tfvars'``

Will delete all managed infrastructure.

Summary:

- Key components of a Terraform file
- Deploying infrastructure:
 - Repeatable
 - Consistent
 
 
Updating Configuration with more resources
------------------------------------------

Terraform state and update
^^^^^^^^^^^^^^^^^^^^^^^^^^

**Terraform State**

JSON format (do not touch!)

Resources mappings and metadata

Locking

Local / Remote

Environments

**Terraform Planing**

Inspect state

Dependency graph

Additions and deletions

Data type and security groups
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Data source**

.. code-block:: bash

 data "aws_availability_zones" 
 "available" {}

Terraform ask aws for availability zones and store all of them in variable "available".

.. code-block:: bash
 
 resource "aws_subnet" "subnet1" {
 cidr_block = 
 "${var.subnet1_address_space}"
 vpc_id = "${aws_vpc.vpc.id}"
 
 availability_zone = 
 "{data.aws_availability_zones.available.names[0]}"
 
 }
 
Terraform select AZ (The first one from the list in variable "available").

**Security group**

.. code-block:: bash

 resource "aws_security_group"
 "elb-sg" {
	name = "nginx_elb_sq"
	vpc_id = "{aws_vpc.vpc.id}"
	
	ingress {}
	egress {}
 }
 
**Load Balancer**

.. code-block:: bash

 resource "aws_elb" "web" {
 name = "nginx-elb"
 
 security_groups = 
 ["${aws_security_group.elb-sg.id}"]
 
 }
 
Demo time
^^^^^^^^^

``moduletwo-start.tf``

.. code-block:: bash

 ##################################################################################
 # VARIABLES
 ################################################################################## 
 
 variable "aws_access_key" {}
 variable "aws_secret_key" {}
 variable "private_key_path" {}
 variable "key_name" {
   default = "PluralsightKeys"
 }
 variable "network_address_space" {
   default = "10.1.0.0/16"
 }
 variable "subnet1_address_space" {
   default = "10.1.0.0/24"
 }
 variable "subnet2_address_space" {
   default = "10.1.1.0/24"
 }
 
 ##################################################################################
 # PROVIDERS
 ##################################################################################
 
 provider "aws" {
   access_key = "${var.aws_access_key}"
   secret_key = "${var.aws_secret_key}"
   region     = "us-east-1"
 } 
 
 ##################################################################################
 # DATA
 ################################################################################## 
 
 data "aws_availability_zones" "available" {} 
 
 ##################################################################################
 # RESOURCES
 ##################################################################################
 
 # NETWORKING #
 resource "aws_vpc" "vpc" {
   cidr_block = "${var.network_address_space}"
   enable_dns_hostnames = "true"
 
 }
 
 resource "aws_internet_gateway" "igw" {
   vpc_id = "${aws_vpc.vpc.id}"
 
 }
 
 resource "aws_subnet" "subnet1" {
   cidr_block        = "${var.subnet1_address_space}"
   vpc_id            = "${aws_vpc.vpc.id}"
   map_public_ip_on_launch = "true"
   availability_zone = "${data.aws_availability_zones.available.names[0]}"
 
 }
 
 resource "aws_subnet" "subnet2" {
   cidr_block        = "${var.subnet2_address_space}"
   vpc_id            = "${aws_vpc.vpc.id}"
   map_public_ip_on_launch = "true"
   availability_zone = "${data.aws_availability_zones.available.names[1]}"
 
 }
 
 # ROUTING #
 resource "aws_route_table" "rtb" {
   vpc_id = "${aws_vpc.vpc.id}" 
 
   route {
     cidr_block = "0.0.0.0/0"
     gateway_id = "${aws_internet_gateway.igw.id}"
   }
 }
 
 resource "aws_route_table_association" "rta-subnet1" {
   subnet_id      = "${aws_subnet.subnet1.id}"
   route_table_id = "${aws_route_table.rtb.id}"
 }
 
 resource "aws_route_table_association" "rta-subnet2" {
   subnet_id      = "${aws_subnet.subnet2.id}"
   route_table_id = "${aws_route_table.rtb.id}"
 } 
 
 # SECURITY GROUPS #
 # Nginx security group 
 resource "aws_security_group" "nginx-sg" {
   name        = "nginx_sg"
   vpc_id      = "${aws_vpc.vpc.id}"
 
   # SSH access from anywhere
   ingress {
     from_port   = 22
     to_port     = 22
     protocol    = "tcp"
     cidr_blocks = ["0.0.0.0/0"]
   }
 
   # HTTP access from anywhere
   ingress {
     from_port   = 80
     to_port     = 80
     protocol    = "tcp"
     cidr_blocks = ["0.0.0.0/0"]
   }
 
   # outbound internet access
   egress {
     from_port   = 0
     to_port     = 0
     protocol    = "-1"
     cidr_blocks = ["0.0.0.0/0"]
   }
 }

 #  INSTANCES #
 resource "aws_instance" "nginx1" {
   ami           = "ami-c58c1dd3"
   instance_type = "t2.micro"
   subnet_id     = "${aws_subnet.subnet1.id}"
   vpc_security_group_ids = ["${aws_security_group.nginx-sg.id}"]
   key_name        = "${var.key_name}"
 
   connection {
     user        = "ec2-user"
     private_key = "${file(var.private_key_path)}"
   }
 
   provisioner "remote-exec" {
     inline = [
       "sudo yum install nginx -y",
       "sudo service nginx start",
       "echo '<html><head><title>Blue Team Server</title></head><body style=\"background-color:#1F778D\"><p style=\"text-align: center;\"><span style=\"color:#FFFFFF;\"><span style=\"font-size:28px;\">Blue Team</span></span></p></body></html>' | sudo tee /usr/share/nginx/html/index.html"
     ]
   }
 }

 ##################################################################################
 # OUTPUT
 ##################################################################################
 
 output "aws_instance_public_dns" {
     value = "${aws_instance.nginx1.public_dns}"
 }

 
 
``terraform plan ...``

``terraform apply ...`` will create terraform.tfstate and terraform.tfstate.lock.info (while running and applaying changes) file(s).

``terraform destroy ...``


``moduletwo-update.tf``

.. code-block:: bash

 ##################################################################################
 # VARIABLES
 ##################################################################################
 
 variable "aws_access_key" {}
 variable "aws_secret_key" {}
 variable "private_key_path" {}
 variable "key_name" {
   default = "PluralsightKeys"
 }
 variable "network_address_space" {
   default = "10.1.0.0/16"
 }
 variable "subnet1_address_space" {
   default = "10.1.0.0/24"
 }
 variable "subnet2_address_space" {
   default = "10.1.1.0/24"
 }
 
 ##################################################################################
 # PROVIDERS
 ##################################################################################
 
 provider "aws" {
   access_key = "${var.aws_access_key}"
   secret_key = "${var.aws_secret_key}"
   region     = "us-east-1"
 }
 
 ##################################################################################
 # DATA
 ################################################################################## 
 
 data "aws_availability_zones" "available" {} 
 
 ##################################################################################
 # RESOURCES
 ################################################################################## 
 
 # NETWORKING #
 resource "aws_vpc" "vpc" {
   cidr_block = "${var.network_address_space}"
 
 }
 
 resource "aws_internet_gateway" "igw" {
   vpc_id = "${aws_vpc.vpc.id}"
 
 }
 
 resource "aws_subnet" "subnet1" {
   cidr_block        = "${var.subnet1_address_space}"
   vpc_id            = "${aws_vpc.vpc.id}"
   map_public_ip_on_launch = "true"
   availability_zone = "${data.aws_availability_zones.available.names[0]}"
 
 }
 
 resource "aws_subnet" "subnet2" {
   cidr_block        = "${var.subnet2_address_space}"
   vpc_id            = "${aws_vpc.vpc.id}"
   map_public_ip_on_launch = "true"
   availability_zone = "${data.aws_availability_zones.available.names[1]}"
 
 }

 #  ROUTING #
 resource "aws_route_table" "rtb" {
   vpc_id = "${aws_vpc.vpc.id}" 
 
   route {
     cidr_block = "0.0.0.0/0"
     gateway_id = "${aws_internet_gateway.igw.id}"
   }
 }
 
 resource "aws_route_table_association" "rta-subnet1" {
   subnet_id      = "${aws_subnet.subnet1.id}"
   route_table_id = "${aws_route_table.rtb.id}"
 }

 resource "aws_route_table_association" "rta-subnet2" {
   subnet_id      = "${aws_subnet.subnet2.id}"
   route_table_id = "${aws_route_table.rtb.id}"
 }
 
 # SECURITY GROUPS #
 resource "aws_security_group" "elb-sg" {
   name        = "nginx_elb_sg"
   vpc_id      = "${aws_vpc.vpc.id}"

   #Allow HTTP from anywhere
   ingress {
     from_port   = 80
     to_port     = 80
     protocol    = "tcp"
     cidr_blocks = ["0.0.0.0/0"]
   }
 
   #allow all outbound
   egress {
     from_port   = 0
     to_port     = 0
     protocol    = "-1"
     cidr_blocks = ["0.0.0.0/0"]
   }
 }
 
 # Nginx security group 
 resource "aws_security_group" "nginx-sg" {
   name        = "nginx_sg"
   vpc_id      = "${aws_vpc.vpc.id}"
 
   # SSH access from anywhere
   ingress {
     from_port   = 22
     to_port     = 22
     protocol    = "tcp"
     cidr_blocks = ["0.0.0.0/0"]
   }
 
   # HTTP access from the VPC
   ingress {
     from_port   = 80
     to_port     = 80
     protocol    = "tcp"
     cidr_blocks = ["${var.network_address_space}"]
   }
 
   # outbound internet access
   egress {
     from_port   = 0
     to_port     = 0
     protocol    = "-1"
     cidr_blocks = ["0.0.0.0/0"]
   }
 }
 
 # LOAD BALANCER #
 resource "aws_elb" "web" {
   name = "nginx-elb"
 
   subnets         = ["${aws_subnet.subnet1.id}", "${aws_subnet.subnet2.id}"]
   security_groups = ["${aws_security_group.elb-sg.id}"]
   instances       = ["${aws_instance.nginx1.id}", "${aws_instance.nginx2.id}"]
 
   listener {
     instance_port     = 80
     instance_protocol = "http"
     lb_port           = 80
     lb_protocol       = "http"
   }
 }

 # INSTANCES #
 resource "aws_instance" "nginx1" {
   ami           = "ami-c58c1dd3"
   instance_type = "t2.micro"
   subnet_id     = "${aws_subnet.subnet1.id}"
   vpc_security_group_ids = ["${aws_security_group.nginx-sg.id}"]
   key_name        = "${var.key_name}" 
 
   connection {
     user        = "ec2-user"
     private_key = "${file(var.private_key_path)}"
   }
 
   provisioner "remote-exec" {
     inline = [
       "sudo yum install nginx -y",
       "sudo service nginx start",
       "echo '<html><head><title>Blue Team Server</title></head><body style=\"background-color:#1F778D\"><p style=\"text-align: center;\"><span style=\"color:#FFFFFF;\"><span style=\"font-size:28px;\">Blue Team</span></span></p></body></html>' | sudo tee /usr/share/nginx/html/index.html"
     ]
   }
 }

 resource "aws_instance" "nginx2" {
   ami           = "ami-c58c1dd3"
   instance_type = "t2.micro"
   subnet_id     = "${aws_subnet.subnet2.id}"
   vpc_security_group_ids = ["${aws_security_group.nginx-sg.id}"]
   key_name        = "${var.key_name}"
 
   connection {
     user        = "ec2-user"
     private_key = "${file(var.private_key_path)}"
   }

   provisioner "remote-exec" {
     inline = [
       "sudo yum install nginx -y",
       "sudo service nginx start",
       "echo '<html><head><title>Green Team Server</title></head><body style=\"background-color:#77A032\"><p style=\"text-align: center;\"><span style=\"color:#FFFFFF;\"><span style=\"font-size:28px;\">Green Team</span></span></p></body></html>' | sudo tee /usr/share/nginx/html/index.html"
     ]
   }
 }
 
 ##################################################################################
 # OUTPUT
 ##################################################################################
 
 output "aws_elb_public_dns" {
     value = "${aws_elb.web.dns_name}"
 }

 

Create a ``terraform.tfstate.backup`` if something go wrong.































