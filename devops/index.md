Hey,


This series is based on my learning on devops.

I have a task to create a server on an environment(isn't prod) in office. This blog is notes for the task.

To solve this I need to follow this procedure.

Final Output:

- 1 EC2 service opened to public(because it isn't prod)
- 1 RDS (shouldn't opened to public)
- Redash service for RDS

Solution:

# VPC(Virtual Private Connection):

- 1 VPC having 2 AZs(Availability Zones) atleast because <b>RDS should have atleast 2 AZs<b>?
- Above VPC should have 2 subnets for public and 2 for private. public -> EC2 and private -> RDS
- Customize the CIDR to have less public

# SGs (Security Groups):

- 3 SGs for RDS,EC2 and Redash

# RDS (Relational Database Service)

- Better to create IAM roles
- Create RDS on above VPC in private subnet
- Add RDS SG

# EC2:

- Create EC2 on above VPC, public subnets and with 1 SG
- Have SG with https anywhere

# Redash:

- Create a EC2 with AMI of redash in Amazon catalogue have t3.medium
- Create a datasource in Redash with above RDS

# DNS entry:

- Created a dns entry using Amazon Certificate Manager with DNS validation

# LoadBalancer/Reverse-proxy:

- As this isn't prod env, I have created a loadbalancer/Reverse-proxy inside EC2 using nginx
- Here nginx will act as reverse-proxy as there is only 1 server/container
- Created and attached SSL certificate to nginx using certbot(free ssl provider)

## Best Practise:

- Prefer script but for the first time create it manually
- Add tags, have a tagwith environment


# Doubts:

### RDS should have atleast 2 AZs ?

### Internet Gateway?

### When to have NAT?

### Todo:

- practical creation of VPC,subnets in local
- RDS,EC2 in docker containers local basically the above setup locally in docker

- Learn more about VPC,gateways,NAT,subnets,Route Table conf,DNS entry

- How certbot,nginx works (implementation)
- How redash works (implementation), go over the docker-compose file


### Amazon Terms/Services and play with those:

- AMI, how to create it and use it
- Amazon Network related VPC,subnets,NAT,IGs etc
- Amazon IAM policies

