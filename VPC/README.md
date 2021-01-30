# VPC

A VPC is a virtual data center in the cloud. 

[CIDR.xyz](https://cidr.xyz/)

## Important

### VPC consists of:
* Internet gateway (IGW) (or Virtual Private Gateway)
* Route Tables
* Network Access Control Lists (NACLs)
* Subnets
* Security Groups

1 subnet = 1 availablity zone (AZ)

Security Groups are stateful; NACLs are stateless

No transitive peering

## Create VPC
* No subnets created automatically
* Main Route table created by default and does not have a route to the Internet
* No Internet Gateway created automatically
* Default NACL created
* Default Security Group created allowing all traffic all protocols

## Create Subnet
* Associate to VPC
* 10.0.1.0/16 (largest) - 10.0.1.0/28 (smallest)
* 10.0.2.0/16 (largest) - 10.0.2.0/28 (smallest)
* By default, Auto-assign public IPV4 address set to No, change to Yes if needed
* New subnets by default are associated to the Main route table and don't have a route to the Internet
* **Subnets not explicitly associated to a route table are associated to the Main route table by default**

## Create Internet Gateway
* Associate to new VPC
* Only 1 Internet Gateway per VPC

## Create Public Route Table
* Create new route table and associate to the VPC
* On the Routes tab of the Route Table, add a new IPV4 route 0.0.0.0/0 and IPV6 ::/0 to Internet Gateway respectively
* Now, any subnet associated to this route table will now route traffic to the Internet 

## NAT Instance and NAT Gateway
* Purpose is to enable EC2 instances in private subnets to connect to the internet to download
* Most of the time, use NAT Gateway
* NAT Instances are on the way out but still on the exam
* NAT Instance is a actually an EC2 instance
* NAT Gateway is a highly available global service
* NAT Gateways are redundant inside the AZ
* Create NAT Gateway on the public subnet
* Edit Main route table, add destination 0.0.0.0/0 to target the NAT Gateway

## scp pem file example
```bash
scp.exe -i aCloudGuruPart1.pem  aCloudGuruPart1.pem  ec2-user@3.239.248.207:/home/ec2-user
```

