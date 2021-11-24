# VPC
## Hometask for Genesis/KMA Highload course

## Steps to complete the hometask.

### 0. Get security keys.
 - [x] Go to the security credentials page.
 - [x] Open Access keys.
 - [x] Save generated AWS Access Key Id, AWS Secret Access Key.

### 1. Configure aws cli account.
 - [x] Enter in terminal command: 'aws configure'.
 - [x] Enter AWS Access Key ID and AWS Secret Access Key.
 - [x] Enter region name. (eu-central-1)

### 2. Create VPC.
 - [x] Enter in terminal command: `aws ec2 create-vpc --cidr-block 10.10.0.0/18 --no-amazon-provided-ipv6-cidr-block --query Vpc.VpcId --output text`.
 - [x] Enter in terminal command: `aws ec2 describe-vpcs | grep "VpcId"`. 
 - [x] As a result, I received VpcId: 'vpc-078c6871f771f25d6' and 'vpc-0ede0a7ea55c30729'.

### 3. Create subnets.

 - [x] Enter command: `aws ec2 create-subnet --vpc-id="$VPC-ID" --cidr-block 10.10.1.0/24`.

Output:
```javascript
{
    "Subnet": {
        "AvailabilityZone": "eu-central-1b",
        "AvailabilityZoneId": "euc1-az3",
        "AvailableIpAddressCount": 251,
        "CidrBlock": "10.10.1.0/24",
        "DefaultForAz": false,
        "MapPublicIpOnLaunch": false,
        "State": "available",
        "SubnetId": "subnet-0559640e57875cfb3",
        "VpcId": "vpc-078c6871f771f25d6",
        "OwnerId": "856989106407",
        "AssignIpv6AddressOnCreation": false,
        "Ipv6CidrBlockAssociationSet": [],
        "SubnetArn": "arn:aws:ec2:eu-central-1:856989106407:subnet/subnet-0559640e57875cfb3"
    }
}
```
 - [x] Enter command: `aws ec2 create-subnet --vpc-id="$VPC-ID" --cidr-block 10.10.2.0/24`.
 - [x] Enter command: `aws ec2 create-subnet --vpc-id="$VPC-ID" --cidr-block 10.10.3.0/24`.


### 4. Create launch template.

 - [x] Input launch template name: 'eu-cental-1-launch-template'.
 - [x] Input version description: 'Test template'.
 - [x] Choose Amazon AMI: 'Amazon Linux 2 AMI (HVM) 64-bit architecture'.
 - [x] Choose Instance Type: 't2.xlarge 4 vCPU 16 GiB Memory'.
 - [x] Generate Key Pair for login. Name: 'secrets.pem'.
 - [x] Setup networking settings: 'VPC, vpc-078c6871f771f25d6'.
 - [x] Submit form: 'network-launch-template (lt-0a7d91f3a3ec8b25c)'.

### 5. Create AWS ASG.

 - [x] Input asg name: 'network-asg'.
 - [x] Select template: 'network-launch-template'.
 - [x] Select VPC: 'vpc-078c6871f771f25d6'.
 - [x] Select Subnets: 'eu-central-1b | subnet-0ac28f020ab9a2932', 'eu-central-1b | subnet-02b84a3be5e3d4578', 'eu-central-1b | subnet-0559640e57875cfb3'.
 - [x] Select group size: minimum capacity: '1', maximum capacity: '3'.
 - [x] Submit form: 'eu-central-1-asg'.

### 6. Edit security groups.

 - [x] Edit inbound groups: 
   1. SSH (22): 'sgr-04f5e59a41cc390bb', My IP.
   2. HTTPS (443): 'sgr-0509bbc3f5c512224', Anywhere IPv4.
   3. HTTP (80): 'sgr-0d6d4d3b6ca32b100', Anywhere IPv4.

 - [x] Edit outbound groups:
   1. SSH (22): 'sgr-07a03dd1b8e9190a3', My IP.
   2. HTTPS (443): 'sgr-0de3319d6f45dd354', Anywhere IPv4.
   3. HTTP (80); 'sgr-09497bea9a70d95c6', Anywhere IPv4.

### 7. Attach ALB.

 - [x] Input ALB name: 'eu-central-1-alb'.
 - [x] Select ALB scheme: 'Internal'.
 - [x] Select VPC: 'vpc-078c6871f771f25d6'.
 - [x] Select Public Subnets and Zones: 'eu-central-1b | subnet-0ac28f020ab9a2932', 'eu-central-1b | subnet-02b84a3be5e3d4578', 'eu-central-1b | subnet-0559640e57875cfb3'.
 - [x] Create target group routing name: 'eu-central-1-alb-target-group'.
 - [x] Select target group port for routing: 80 (TCP).

