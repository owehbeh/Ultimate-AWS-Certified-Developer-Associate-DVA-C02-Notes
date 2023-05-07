
___
<br>

## Notes for the Udemy Course "**[Ultimate AWS Certified Developer Associate DVA-C02](https://www.udemy.com/share/101WgC3@DRbJZZoAoUIMoYUmM3u5Ke78QEJ9BP5f8qPUuyMpR6phqEGLaAqnz7zAl-GsFUH-/)**"
<br>

___

# 4️⃣ IAM & AWS CLI 
### IAM = Identity and Access Management (_Global Service_)
## 🟨 Consists of
- Root account (_Shouldn't be used or shared_)
- Users
- Groups
  - Can only contain users, not other groups 
  - Users can belong to multiple groups 
## 🟨 Policies (_Permissions_)
A JSON document that defines what a user/group is allowed to do
- Least privilege principle
  - Don't give more permission than a user needs 
- Inline policies
  - User-specific policies 
- Structure
  - Sid
    - Identifier for the statement (_optional_)
  - Effect
    - Whether to allow or deny access
      - Allow
      - Deny
  - Principle
    - To what this policy applies to
      - Account
      - User
      - Role
  - Action
    - List of actions allowed or denied
  - Resources
    - List of resources the actions are applied to
  - Condition
    - Conditions for when this policy is in effect (_optional_)
```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "iam:GetContextKeysForCustomPolicy",
                "iam:GetContextKeysForPrincipalPolicy",
                "iam:SimulateCustomPolicy",
                "iam:SimulatePrincipalPolicy"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```
## 🟨 IAM Password Policy
- Minimum length 
- Require specific char types 
  - Include uppercase 
  - Include lowercase 
  - Numbers 
  - Non-alphanumeric  
- Allow all IAM users to change their own password 
- Password Expiration: Require users to change their password 
- Prevent password re-use
## 🟨 MFA (Multi Factor Authentication)
- Virtual MFA Device 
  - Google Authenticator 
  - Authy 
- U2F = Universal 2nd factor Security Key 
  - YubiKey by Yubico 3rd party 
- Hardware Key Fob MFA Device 
  - Provided by Gemalto 3rd party 
- Hardware Key Fob MFA Device for AWS GovCloud (US) 
  - Provided by SurePassID 3rd party
## 🟨 CLI = Command Line Interface 
- Access Keys are generated through AWS Console 
- Users manage their own access keys 
- Don't share access keys! 
  - Access Key ID ~ username 
  - Secret Access Key ~ Password 
## 🟨 CloudShell 
- A browser-based shell with AWS CLI access from the AWS Management Console
- Not available everywhere
## 🟨 IAM Roles 
- To be used by AWS Servies to perform actions 
- Common Roles 
  - EC2 Instance Roles 
  - Lambda Function Roles 
  - Roles for CloudFormation 
- Only Roles for AWS Services is required to know for the exam
## 🟨 IAM Security Tools 
- IAM Credentials Report (_account level_)
  - A report that lists all accounts users and the status of various credentials 
- IAM Access Advisor (_user level_)
  - Shows the service permissions granted to a user and when they were last accessed 
  - Use information to revise policies 
## 🟨 Best Practices
- Don't use the root account except for AWS account setup
- One physical user **==** One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce MFA
- Use Access Keys for programmatic access (_CLI/SDK_)
- Audit permissions of your account using
  - IAM Credentials Report
  - IAM Access Advisor
- Never share IAM users & access keys
# 5️⃣ EC2 Fundamentals
## 🟥 OS
- Linux 
- Windows 
- Mac OS
## 🟥 Storage 
- Network attached 
  - EBS 
  - EFS 
- Hardware 
  - EC2 Instance Store
## 🟥 User Data (scripts) 
- Bootstrap instances – launch commands when machine starts 
  - Install updates 
  - Install software 
  - Download common files 
- Runs as root user
## 🟥 Instance Types (_Focus on names not examples_) 
### Naming Convention
- m5.2xlarg
  - m (_instance class_)
  - 5 (_generation, AWS imrpoves over time_)
  - 2xlarge (_size within the instance class_)
### Types
- General Purpose 
  - Great for diversity of workloads such as web servers 
  - Balance between 
    - Compute 
    - Memory 
    - Networking 
- Compute Optimized 
  - Batch processing workloads 
  - Media transcoding 
  - High performance web servers 
  - High performance computing (HPC) 
  - Scientific modeling & machine learning 
  - Dedicated gaming servers 
- Memory Optimized 
  - High performance for in-memory databases (Business intelligence BI) 
  - Distributed web scale cache stores 
  - Real-time processing of big unstructured data 
- Storage Optimized 
  - High frequency online transaction processing (OLTP) 
  - Data warehousing applications 
  - Distributed systems
## 🟥 Security Groups
- Network security in AWS 
- Control how traffic is allowed into or out of EC2 Instances 
- Only contains "allow" keys 
- Security groups can reference by IP or by security groups 
- They regulate 
  - Ports 
  - IP ranges 
  - Inbound network 
  - Outbound network 
- Components 
  - Type – ex = HTTP 
  - Protocol – ex = TCP 
  - Port Range – ex = 80 
  - Source – ex = 0.0.0.0/0 
  - Description – ex = test http page 
- Good to know 
  - Can be attached to multiple instances 
  - Locked down to a region / VPC 
  - Good to maintain one separate security group for SSH access 
  - All inbound is blocked by default 
  - All outbound is authorized by default
  - Any time you face a timeout trying to access an EC2 instance, check the Security Group
  - Never enter your IAM security keys into an EC2 instance, anyone with access to the instance can retreive the keys
    - Instead use IAM Roles
- Classic Ports to know (_Important for exam_) 
  - **3389** = RDP to login from a Windows instance
  - **22** = SSH log into Linux instance + SFTP 
  - **21** = FTP File Transfer Protocol 
  - **80** = HTTP  
  - **443** = HTTPS 
- **NEVER** USE KEYS IN Instance Connect 
  - Instead use/attach IAM Role to EC2 Instances
## 🟥 EC2 Instance Purchasing Options 
- On-Demand 
  - Pay by second/hour 
- Reserved 
  - Reserve Instance Attributes 
    - Type 
    - Region 
    - Tenancy 
    - OS 
  - Long Workloads 
  - Specify period 
  - Payment options 
    - No upfront 
    - Partial upfront 
    - All upfront 
  - Best for steady-stat (_database_) 
- Convertible Reserved  
  - Long workloads 
  - Flexible instances 
  - Change Instance Attributes 
  - Less discount 
- Saving 
  - Discount based on type of usage (10$/hours for 3 years) 
- Spot 
  - Not suitable for critical jobs or databases (_important for exam_)
  - Can lose instance at any point 
  - Suitable for workloads with flexible start and end time 
- Dedicated Host 
  - A physical server with EC2 instance capacity fully dedicated to your use
  - Address compliance requirements 
  - Use existing server-bound software licenses 
- Dedicated Instances 
  - Instances run on hardware that is not shared 
- Capacity Reservations 
  - Reserve On-Demand instances so you always have access to capacity with no surprises 
  - No time commitment and no discounts
### 🏨 Hotel Purchasing Decision Examples
- On-Demand
  - Coming in and staying in resort whenever we like
  - We pay the full price
- Reserved
  - Like planning ahead
  - If we plan to stay for a long time, we may get a good discount
- Saving Plans
  - Pay a certain amount per hour for certain period
  - Stay in any room type
    - King
    - Suit
    - Sea View
- Spot Instances
  - Hotel allows people to bid for the empty rooms
  - Highest bidder keeps the room
  - Can get kicked out at any time
- Diedicated Hosts
  - We book an entire building of the resort
- Capacity Reservations
  - You book a room for a period with full price even you don't stay in it
# 6️⃣ EC2 Instance Storage
## 🟩 EBS (_Network Drives_)
- Each volume can be attached to one instance except io1/io2 
- Volume Types 
  - Gp2/gp3 (SSD) balanced price/performance 
    - **EBS IOPS peaks at 16,000 IOPS** 
    - gp3 Can increase IOPS and Throughput **independantly** while in gp2 they are **linked together**
  - io1/io2 (SSD) highest performance 
    - Suits critical business applications 
    - Databases workloads 
    - **For over 32,000 to 64,000 IOPS we need Nitro EC2** 
    - **EBS Muti-Attach**
      - Attach same EBS to multiple EC2 **instances in the same AZ**
      - **Up to 16 EC2 instances at the same time**
      - Higher application availability 
      - Application must manage concurrent write ops 
  - st1 (HDD) low cost 
  - sc1 (HDD) lowest cost 
  - NOTE: **Only SSD can be used as boot volumes**
- Snapshots
  - Transfer EBS Volume from one AZ to another
## 🟩 EC2 Instance Store (_Hardware Disk_) 
- **Better I/O performance (IOPS of 310,000)**
- **Ephemeral: lost on instance stop**
- Good for buffer cache temporary  
- Risk of Data loss 
## 🟩 EFS: Elastic File System (_Managed File System_) 
- **Works with EC2 instances in muti-AZ**
- Highly available, scalable, expensive 
- Use cases 
  - CMS 
  - Data sharing 
  - WordPress 
- Uses Security group to control access 
- **Only Linux based AMI**
- **Encryption at rest through KMS**
- Standard file API 
- **Scale**
  - 1000s of concurrent NFS clients 
  - Grow to petabyte-scale network file system, automatically 
- **Performance Mode (_set at creation time_)**
  - General Purpose: latency-sensitive use cases (web server, cms, etc..) 
  - Max I/O: higher latency, throughput, highly parallel (big data, media processing) 
- **Throughput mode**
  - Bursting (1TB = 50MiB/s + burst of up to 100MiB/s) Scale with system file size 
  - Provisioned: set throughput regardless of storage size 
  - Elastic: automatically scale throughput up and down based on workload
- **Storage Classes**
  - Storage Tiers 
    - Standard 
    - Infrequent: (EFS-IA) 
      - Lifecycle policy: if not used for n days, move to EFS-IA 
  - Availability & durability 
    - Regional: Multi-AZ, great for production 
    - One Zone: great for dev, backup, compatible with IA
## 🟩 AMI
- AMIs are for specific regions, cannot use AMI in region A for region B
### AMI Creation Process
1. Start an EC2 instance
2. Customize it
3. Stop the instance (_for data integrity_)
4. Build an AMI from this instance (_this will also create EBS snapshots_)
5. Launch other EC2 instances from the created AMI