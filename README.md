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
- [ ] create VM instance and 
- [ ] deploy service
- [ ] configure CDN with VM instance
## Guide
### Base
- On your system set your AWS credentials on your environment
```
export AWS_ACCESS_KEY_ID="" &&  export AWS_SECRET_ACCESS_KEY="" && export AWS_REGION=""
```
### Provision
- Run the AWSRDS provisioning script
```
ansible-playbook -i ./ec2.py -T 60 -f 100 --private-key=~/ikmuge.pem AWSRDS.yml -e "username={username} password={password} instance_name={instance_name} db_name={dbname}"
```

### Deploy
### Test