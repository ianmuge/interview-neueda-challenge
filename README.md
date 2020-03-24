# Challenge
## Task:
Create Ansible playbook to deploy simple service on Public Cloud

Service can be any application with database - Redmine, Mediawiki, etc.
Public Cloud Infrastructure is AWS (EC2, RDS, CloudFront) or Azure (Azure VM, SQL Database, Azure CDN)

## Environment:
Ansible, AWS/Azure

## Description:
Write Ansible playbook which will:
1. create database
2. create VM instance and deploy service on it
3. configure CDN with VM instance

## Acceptance criteria:
Service running after executing Ansible playbook
It available to open service in browser, sign in, etc.

# Solution
## Prerequisites
- All deployment will be run on the default VPC. If you wish to build a new VPC you can use the repo below
    - https://github.com/muge13/ansible-provision-aws-vpc
- You have ansible installed
- You have your AWS ec2 key pair, access_key and Secret key
- You have a RHEL 7 instance provisioned on AWS with NGINX installed. You can use the repos below to provision and set up the instance(s).
    - https://github.com/muge13/ansible-provision-aws-ec2
    - https://github.com/muge13/ansible-configure-base
    - https://github.com/muge13/ansible-configure-nginx-simple 
## Tasks
- [x] create database
- [x] create VM instance
- [x] configure VM for service deployment
- [x] deploy service
- [x] configure CDN with VM instance
- [x] run service setup
## Guide
### Base
- On your system set your AWS credentials on your environment
```
export AWS_ACCESS_KEY_ID="" &&  export AWS_SECRET_ACCESS_KEY="" && export AWS_REGION=""
```
### Provision
#### RDS
- Run the AWSRDS provisioning script
```
ansible-playbook -i ./ec2.py -T 60 -f 100 --private-key=~/{keypair}.pem AWSRDS.yml -e "username={username} password={password} instance_name={instance_name} db_name={dbname}" -v
```
- you will use the produced RDS endpoint and credentials in the deployment step.
#### EC2
- Provision EC2 Instance
```
ansible-playbook  -u ec2-user  -i ec2.py  -T 60 -f 100 --private-key=~/{keypair}.pem AWSEC2.yml -e "vpc_id={vpc_id} zone=a ec2_Name=test ec2_role=test ec2_instance_type='t2.micro' ec2_keypair={keypair} ec2_image='{RHEL_AMI_id}' storage_size={size}"
```
- You will have to modify the tags on your target hosts based on the definitions in the provisioning scripts
- Configure EC2 Instance to prepare for service. This configures Base configuration, Nginx and PHP.
```
ansible-playbook -i ./ec2.py -T 60 -f 100 --private-key=~/{keypair}.pem Configure.yml
```
#### CDN
- Modify the variables on the `AWSCDN.yml` file to be in line with those in your environment, you can also override them on the command.
- configure the CDN
```
ansible-playbook -i ./ec2.py -T 60 -f 100 --private-key=~/{keypair}.pem AWSCDN.yml
```
### Deploy
- Modify the variables on the `EC2DeployMagento.yml` file to be in line with those in your environment, you can also override them on the command.
- Deploy the magento service
```
ansible-playbook -i ./ec2.py -T 60 -f 100 --private-key=~/{keypair}.pem EC2DeployMagento.yml
```
- Go to the cdn endpoint on the browser and it should provide an interface to perform installation set up for the Magento Service