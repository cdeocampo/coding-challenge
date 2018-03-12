# DevOps Coding Challenge
This creates an environment in **Singapore** region that auto scales when there's increase or decrease in traffic.
A simple docker container (nginx) runs on several EC2 instances on port `8080` which is spread across availability zones.

## Requirements
- Configured `awscli`
- Permission to create CloudFormation stack and resouces

## Instructions
1. Create S3 bucket where CloudFormation will be stored (e.g. tribe-coding-challenge-1234)
2. (Optional) Create EC2 Key Pair
3. Clone this repo into your local machine
4. Go to the directory in your machine
5. Review `master.cfg` and edit the value for:
    - `S3BucketParam` with the name of the S3 bucket you created in step 1
    - `SSHKeyNameParam` with the name of EC2 key pair you created in step 2 or existing EC2 key pair
6. Upload the three CloudFormation templates in the S3 bucket you created in step 1
    - master.template.yaml
    - vpc.template.yaml
    - web.template.yaml
7. Execute this in your cli relative to the directory of this repo in your local machine replacing the `<profile>`
    - `aws cloudformation create-stack --stack-name Tribe-coding-challenge --template-body file://master.template.yaml --parameters file://master.cfg --profile <profile> --region ap-southeast-1 --capabilities CAPABILITY_IAM`

## Resources
This stack creates the following:

- VPC
    - Internet gateway
    - 2 public subnets
    - 2 private subnets
    - NAT gateway
- Application Load Balancer
- Auto Scaling Group
- Launch Configuration
- ECS Cluster
- ECS Service
- IAM Roles

## Output
- In AWS console, go to CloudFormation service. You should see 3 stacks with `CREATE_COMPLETE` status
- Select _Tribe-coding-challenge_ stack and go to Outputs tab. You should see the value of `ApplicationDnsName`
- Copy this value and paste it in your browser and append `:8080` in it to see the application
- You should see *Welcome to nginx!* message