
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
# 7️⃣ AWS Fundamentals: ELB + ASG
## 🟪 High Scalability
- Vertical
  - Increase instance size (hardware limit) 
  - Non distributed systems like Database 
  - Services that can scale vertically 
    - RDS 
    - ElastiCache 
- Horizontal
  - Increase number of instances 
  - Auto scaling group 
  - Load balancer 
  - Distributed systems: modern applications
## 🟪 High Availability
- Runs hand in hand with Horizontal Scaling 
- Auto Scaling Group multi AZ: at least 2 data centers == AZ 
- Load Balancer multi AZ 
- Goal is surviving data center loss
## 🟪 Load Balancer
### CLB (_Classic Load Balancer - deprecated_) 
- Supports
  - TCP (layer 4) 
  - HTTP, HTTPS (layer 7) 
- Fixed hostname: xx.region.elb.amazonaws.com 
- One static host name 
### ALB (_Application Load Balancer - Layer 7_) 
- Supports
  - HTTP, HTTPS, HTTP/2 (_layer 7_) 
  - WebSocket 
- **Routing** 
  - Based on path 
    - Example.com/users 
    - Example.com/posts 
  - Based on hostname 
    - One.example.com 
    - Other.example.com 
  - Based on query string / header 
    - Example.com/users?id=123
- Use cases
  - Micro services 
  - Container based apps 
  - Docker 
  - Amazon ECS
- **Target Groups** 
  - EC2 instances (managed by Auto Scaling Group) 
  - ECS tasks (managed by ECS itself) 
  - Lambda Functions 
  - IP Addresses
- Notes
  - Application does not have direct access to **IP of client** instead it has to look for extra header (**X-Forwarded-For**) to find the original client IP 
  - Port mapping allow redirect to dynamic port in ECS 
  - One ALB to multiple applications
  - **One static hostname**
### NLB (_Network Load Balancer - layer 4_)
- Supports
  - TCP (_layer 4_)
  - TLS
  - UDP (_layer 4_)
- **Target Groups** 
  - EC2 Instances 
  - IP Addresses 
    - EC2 instance 
    - On-premise server with static IP 
  - ALB 
    - Have static IP on NLB level and use the features of ALB for HHTP related features
- Notes
  - Lower latency = 100ms (vs 400ms for ALB) 
  - Millions of requests per second (**extreme performance**) 
  - **One static/fixed IP per AZ** 
### GWLB (_Gateway Load Balancer_)
- Supports
  - IP Packets (_layer 3 – network layer_)
- **Target Groups**
  - EC2 Instances
  - IP Addresses 
- Use cases 
  - Use 3rd party  network virtual appliances before passing traffic to app 
  - Firewalls 
  - Prevention systems 
  - Deep packet inspection systems 
  - Payload manipulation 
- **How does it work**
  - Transparent Network Gateway: Single entry single exist 
  - Load Balancer: Distribute traffic to your virtual appliances 
- Notes
  - **GENEVE protocol on port 6081 = GWLB**
### Sticky Sessions (_Session Affinity_)
- Same client always redirected to the same instance
- Supports
  - CLB
  - ALB
- How does it work?
  - Though coockie
  - **Application based cookie**
    - Custom Cookie
      - Generated by target
      - Can include custom attributes
      - Coockie name must be specified for each target group
    - Application Cookie
      - Generated by the LB
  - **Duration based cookie**
    - Generated by the LB
- Reserved cookie keys
  - AWSALB
  - AWSALBAPP
  - AWSALBTG
### Cross-Zone Load Balancing
Distribute load evenly across instances in different zones
- Example
  - Turned OFF
    - Zone#1 
      - Instence#1 – 50% 
      - Instence#2 – 50% 
    - Zone#2 
      - Instence#1 – 10% 
      - Instence#2 – 10% 
      - Instence#3 – 10% 
      - Instence#4 – 10% 
      - Instence#5 – 10% 
  - Turned ON
    - Zone#1 
      - Instence#1 – 14.2% 
      - Instence#2 – 14.2% 
    - Zone#2 
      - Instence#1 – 14.2% 
      - Instence#2 – 14.2% 
      - Instence#3 – 14.2% 
      - Instence#4 – 14.2% 
      - Instence#5 – 14.2%
- Default Behavior
  - CLB
    - Disabled by default 
    - Not charges of inter AZ 
  - ALB
    - Always on (Can't be disabled) 
    - No charges for inter AZ 
  - NLB
    - Disabled by default 
    - Pay charges for inter AZ
### SSL/TLS
- SNI: Server Name Indication 
  - Solves the problem of loading multiple SSL onto one web server 
- CLB: one SSL per CLB 
- ALB: multiple SSL using SNI 
- NLB: multiple SSL using SNI
### Connection Draining 
- CLB – Connection Draining 
- ALB + NLB – Deregistration Delay 
- Duration 
  - 1 to 3600 second  
  - Default: 300 second
### Notes
- Spread load across multiple downstream instances 
- Single point of access (DNS) 
- Handle failures of downstream instances 
- Regular health checks 
  - When you enable ELB Health Checks, your ELB won't send traffic to unhealthy (crashed) EC2 instances. 
- SSL termination 
- Stickiness with cookies 
- High availability across zones 
- Separate public / private traffic 
## 🟪 ASG: Auto Scaling Group
### Attributes
- Launch configuration 
  - AMI + Instance Type 
  - EC2 User Data 
  - EBS Volumes 
  - Security Groups 
  - SSH Key Pair 
- Min Size / Max Size / Initial Capacity (_Target Capacity_)
- Network + Subnets Information 
- Scaling Policies 
  - Based on CloudWatch Alarms
### Rules
- Target Tracking Scaling 
  - Target Average CPU Usage 
  - Number of requests on ELB/Instance 
  - Average Network In 
  - Average Network Out 
- Simple / Step Scaling (_Custom Metric_) 
  - Number of connected users, for ex 
    - 20 < users remove 
    - users > 60 add 
  - Send custom metric from app (through PutMeticAPI) to CloudWatch 
  - Create CloudWatch alarm to react to low/ high values 
  - Use **CloudWatch alarm** as the scaling policy for ASG 
- Scheduled Actions 
  - Based on known usage patterns 
  - Example: increase min capacity from 10AM to 1PM 
- Predictive Scaling (new) 
  - Continuously forecast load and schedule scaling ahead
### Metrics to scale on
- CPUUItilization 
- RequestCountPerTarget 
- Average Network In/Out
### Scaling Cooldowns
- After scaling cooldown period
- Default is 300 seconds
### **Instance Refresh**
Update entire ASG based on a new launch template
- Start instance refresh
- Set a minimum healthy percentage (_how many instances can be deleted over time_)
- As instances are terminated new ones come up
- At the end all instances will have the new launch configuration
### Notes
- Creates new instances if existing ones get terminated for whatever reason like it becomes un healthy
# 8️⃣ RDS + Aurora + ElasticCache
## ⏹️ RDS: Relational Database Service
### Supports
- PostgreSQL 
- MySQL 
- MariaDB 
- Oracle 
- Microsoft SQL Server 
- Aurora (AWS proprietary database)
### Advantages
- Automated provisioning & OS patching 
- Backups 
  - Automated 
    - Daily 
    - Transaction logs every 5 minutes 
    - Can restore to any point in time because of transaction logs 
    - 7 days retention can be increased to 35 days 
  - Snapshots 
    - Manually triggered 
    - Retention of backups as long as you want 
- Monitoring 
- Read replicas (better performance) 
- Multi AZ setup for DR (Disaster Recovery) 
- Scaling vertically and horizontally 
- Storage backed by EBS(gp2 / io1) 
- **CANNOT USE SSH since it is managed**
### RDS + ASG
- Scale automatically, for example if 
  - Free storage less than 10% 
  - Low storage lasts at least 5 minutes 
  - 6 hours have passed since last modification 
- Maximum Storage Threshold (max limit for db storage)
### Read Replicas
- Scale your reads 
- Up to 15 read replicas within 
  - AZ (free) 
  - Cross AZ (free) 
  - Cross Region (costs replication fee) 
- Asynchronous replication 
  - Reads are eventually consistent 
  - Might read old data that is not synced at the exact second of request 
- Use case 
  - Analytics app: reads from the read replica instead of overloading app DB
- Notes 
  - Can be promoted to their own DB 
  - Applications must update connection string 
  - Only for READ, cannot update, insert, delete
### Multi AZ 
- Disaster Recovery 
- Synchronous replication 
  - Replicate every single change  
  - Replicated to other DB to be accepted 
- One DNS name -> Automatic failover 
- Increase availability 
- Notes 
  - Can read replicas be setup as Multi AZ for Disaster Recovery (DR) ? YES 
### Single-AZ to Multi-AZ 
- Zero downtime 
- Click modify > enable multi-AZ
### Encryption
- At Rest 
  - AWS KMS-AES-256  
  - Defined at launch time 
  - If master not encrypted, replicas cannot be encrypted 
  - TDE: Transparent Data Encryption available for Oracle and SQL Server 
- In-Flight 
  - SSL 
  - To enforce SSL 
    - PostgreSQL: 
      - Open AWS RDS Console (Parameter Groups) 
      - rds.force_ssl=1 
    - MySQL: 
      - Within the DB 
      - GRANT USAGE ON *.* TO 'mysqluser'@'%' REQUIRE SSL;
### Encrypt RDS Backup 
- Snapshot of un-encrypted = un-encrypted 
- Snapshot of encrypted = encrypted 
- Solution: Copy Snapshot into encrypted database 
  - Create snapshot of un-encrypted 
  - Copy snapshot 
  - Enable Encryption of snapshot while copying 
  - Restore database from encrypted snapshot 
  - Migrate app from old to new database
### Security 
- Better deployed within private subnet not public ones 
- Uses Security Groups, same as EC2 
- Encryption at rest using KMS 
- Encryption in flight using SSL 
### Access Management 
- IAM policies 
- Username / Password 
- IAM-based authentication 
  - Token based login 
  - Lasts 15 minutes 
  - Works only with 
    - MySQL 
    - PostgreSQL 
  - Benefits 
    - SSL encryption 
    - IAM managed users instead of DB 
    - Leverage IAM Roles and EC2 instance profiles 
### AWS Responsibility 
- No SSH access 
- No manual DB patching 
- No manual OS patching 
- No way to audit underlying instance
## ⏹️ Aurora
- Supports PostgreSQL and MySQL 
- Claim 3x-5x performance improvement 
- Starts at 10GB maxes to 128TB 
- Can have 15 replicas vs MySQL has 5   
- Instant failover, faster than multi-AZ 
- Supports cross-region replication 
- Backtrack: restore data at any point of time without using backups 
- High Availability 
  - 6 copies across 3 AZ (without replicas) 
    - 4/6 needed for writes 
    - 3/6 needed for reads 
    - Self-healing peer-to-peer replication 
    - Storage striped across 100s of volumes 
  - Each Aurora Instance accepts writes (master) 
  - Failover < 30 secs  from Master to Read Replicas 
- DB Cluster 
  - Writer Endpoint 
    - Points to master 
    - On failovers it updates automatically 
  - Reader Endpoint 
    - Connection load balancer 
    - Connects automatically to all Read Replicas 
  - One Master 
  - 1-15 Read Replicas 
  - 10GB-128TB 
- Security 
  - Same as RDS
## ⏹️ RDS Proxy
### Why use it?
- Allow application to pool and share a database 
- Improve database efficiency by reducing stress on database resources (_CPU & RAM_)
- Minimize the open connections and timeout
- Reduce failover time to to 60% 
- Enforce IAM authentication using secrets manager
- Specify percentage of pool to Lambda Functions so the y won't overload connections to database
### Supports
- MysQL
- PosgresSQL
- MariaDB
- Microsoft SQL Server
- Aurora
  - MySQL
  - PostgreSQL
### Notes
- Only accessible from within VPC not to the public internet
## ⏹️ElastiCache 
- Managed Redis or Memcached 
  - In-memory databases with high performance low latency 
- Reduce read loads from database 
- Make app stateless 
- Sample process #1 
  - App > cache >  
    - Found > return 
    - Not found > fetch from db > write to cache 
  - Make sure cache is uptodate 
- Sample process #2 
  - Save user session to cache 
  - User calls another instance 
  - User still logged in cause session is fetched from cache 
- Redis 
  - Multi AZ 
  - Auto failover 
  - Read replicas (HA) : 0-5 
  - Data durability 
  - Backup/restore 
- Memcached 
  - Multi-node partitioning of data (sharding) 
  - No HA (Replication) 
  - No persistence 
  - NO backup/restore 
  - Multi-threaded
### Strategies
- **Lazy loading**: (Cache-aside/Lazy population) 
  - Sample process #1 
    1. Cache hit
    2. Result Returned
  - Sample process #2
    1. Cache miss
    2. Read from DB
    3. Write to cache
  - Pros 
    - Only requested data is cached 
    - Node failures are not fatal 
  - Cons 
    - Cache miss penalty result in 3 round trips 
    - Stale data: data can be outdated 
- **Write Through**: Add/Update cache when db is updated 
  - Process: 
    - Write to db 
      - Write to cache as well 
    - Read from cache > return 
  - Pros: 
    - No stale data = fast reads 
    - Write vs Read penalty (users expect read to take less time) 
  - Cons: 
    - Missing data until it is cached 
    - Cache chrun, a lot of data will never be read 
- **Cache Eviction + TTL** (Time-to-live) 
  - Options 
    - Delete items explicitly 
    - Evict cache on full memory (LRU) 
    - Set an item time-to-live (TTL)
- Types 
  - Cluster Mode Disabled 
    - One primary node, 0-5 replicas 
    - Asynchronous replication 
    - Primary node used for R/W 
    - Other nodes are read-only 
    - One shard, all nodes have all the data 
    - Guard against data loss 
    - Multi-AZ enabled for failover 
  - Cluster Mode Enabled 
    - Data partitioned across shards 
    - Each shard has primary and 0-5 replicas 
    - Multi-AZ enabled for failover 
    - Up to 500 nodes per cluster 
    - 0 replicas: 500 shards  
    - 1 replica: 250 shards 
    - 5 replicas: 83 shards
- Implementation consideration 
  - Data might be outdated 
  - Is it effective? 
    - Pattern: data changing slowly 
    - Anti-patterns: data changing rapidly 
  - Is data  structured well 
    - Key/value? 
    - Caching aggregation results?
## ⏹️ Amazon MemoryDB for Redis
A Database with Redis compatible API
### What is it?
- Ultra fast performance (_over 160M request/sec_)
- In-memory data with multi-AZ transaction log
- Scales seamlessly
### Usecases
- Web/Mobile apps
- Online gaming
- Media streaming
### Example
- You have a lot of microservices and you need access to Redis compatible in-memory database
- Use memoryDB for Redis
- Get ultra fast in-memory speed
- Multiple AZ deployment
- Access to fast recovery and data
# 9️⃣ Route 53
## 🟧 Record Types 
- A: Maps hostname to IPv4 
- AAAA: Maps hostname to IPv6 
- CNAME: maps hostname to another hostname 
  - Target domain must have A or AAAA record 
  - Cannot create CNAME for top node of DNS namespace, example: cannot create for example.com but for www.example.com  
- NS: Name Servers for the hosted zone 
  - Control how traffic is routed for a domain
## 🟧 Hosted Zoned 
- Container of records that defines how to route traffic to a domain and its subdomains 
- Public Hosted Zones: Public domain name 
- Private Hosted Zones: only available within one or more VPC
## 🟧 CNAME vs Alias 
### CNAME 
- Points a hostname to any other hostname (app.domain.com => bla.anything.com) 
- Only for non-root domains (something.domain.com) 
### Alias 
- Points a hostname to an AWS Resource (app.domain.com => bla.amazon.com) 
- For non-root and root domains (domain.com = domain apex) 
- Free 
- Native health check
## 🟧 Routing Policies 
### Simple 
- Point to single resource (even with Alias enabled) 
- Can specify mult values, but client will choose one 
- **No healthchecks**
### Weighted 
- % of requests to  resource 
- Traffic % = weight for record / sum of all weights of all records 
- % does not end at 100 
- **DNS records must have same name and type**
- Can have healthchecks 
- If one record's weight is 0, it will have no traffic 
- If all records' weight is 0, all will have equal traffic 
- Usecases: 
  - Load balancing 
  - Testing new app versions by sending a small amount of traffic 
### Latency 
- Can have healthchecks 
- **Can have failover**
- Usecases: 
  - Helpful when latency for users is priority 
  - Germany users may be directed to US (if that is the lowest latency) 
### Failover 
- Must have primary & secondary types 
- Must be associated with a health check 
- If primary health check fails client will get the secondary (disaster recovery instance) 
### Geolocation 
- Based on user location – NOT LATENCY 
- Specify location by 
  - Continent 
  - Country 
  - US Satet 
- If overlapping exist, most precise location will be selected 
- Can be associated with health checks 
- Use cases 
  - Website localization 
  - Restricting content 
  - Load balancing 
### Geoproximity (_Region Biased_)
- Route traffic based on user location and resources location 
- Bias: shifts traffic based on this number's value 
  - Expand: 1 to 99 
  - Shrink: -1 to –99 
- Example: 
  - AWS resources (specify region) 
  - Non-AWS (specify Lat Lon) 
- Use case: 
  - **Shift traffic from one region to another by increasing the bias**
### IP Based
Define the routing based on the client IP addresses
- Define a list of Siders (_ip ranges_)
- base on the Sider which location traffic should be sent to
- Use cases
  - Optimize performance
  - Reduce network costs
- Example
  - If you know a specific ISP that are using a specific siders of IP Adresses
  - Route users to a specific endpoint
### Multi Value 
- Route traffic to multiple resources 
- Can have health checks, while Simple cannot 
- 1-8  healthy records 
- Only healthy will be returned to client 
- Is not a substitute for ELB, it is a client side load balancing
## 🟧 Traffic Flow 
- GUI editor for complex configuration 
- Configurations saved as Traffic Flow Policy 
- Can be applied to different Hosted Zones (domain names) 
- Supports Versioning 
- Costs money! 
- **Very useful with Geoproximity**
## 🟧 Health Checks  
HTTP health checks are only for public resources 
### Types 
- **Monitors an endpoint**
  - Example:
    - App 
    - Server 
    - Other AWS resource 
  - Process: 
    - 15 Global health checkers  
    - 10 - 30 sec interval (10 sec is higher cost) 
    - **If > 18% report healthy, Route 53 consider it Healthy, else Unhealthy**
    - Can choose locations to user 
  - **Notes**:
    - Endpoint must respond with 2xx or 3xx as healthy 
    - Can pass/fail based on the text in first 5120 bytes of the response 
    - Must allow incoming requests from Route 53 health checkers in router/firewall 
- Monitors other health checks (these are named **calculated healthchecks**) 
  - Combine result of multiple health checks 
  - Can use OR, AND, or NOT 
  - Monitor up to 256 child health checks 
  - Specify number of children passes to make parent pass 
  - Use case: 
    - Perform maintenance withoutt causing all health checks to fail 
- **Monitors CloudWatch Alarms** (Full control) (good for private resources) 
  - Example: 
    - Throttles 
    - DynamoDB 
    - Alarms on RDS 
    - Custom metrics 
  - Problem: 
    - Route 53 health checks are outside VPC 
    - They cannot access private endpoints 
  - Solution: 
    - Create a CloudWatch Metric 
    - Associate a CloudWatch Alarm 
    - Create a Health Check that checks the alarm itself 
### Usecases 
- Automated DNS Failover
## 🟧 3rd Party Domain Registrar 
### Process 
1. Create a Hosted Zone in Route 53 
2. Update NS Records on 3rd party website to user Route 54 Name Servers
# 1️⃣0️⃣ VPC Fundamentals
## 🟦 VPC 
- Virtual Private Cloud 
- A regional resource 
  - Two AWS regions 
  - Have two VPCs 
- **Can have multiple subnets**
## 🟦 Subnets 
- Allows to partition network inside VPC (AZ resouce) 
- To define access between private and public we use **Route Tables**
- Types 
  - Public (_through Internet Gateway_)
    - Can access www 
    - Can be accessed by www 
  - Private 
    - Not accessible from the internet
## 🟦 Internet Gateways 
- Helps VPC connect to the internet 
- Public Subnets have route to internet gateway
## 🟦 NAT Gateways 
- Are AWS-managed 
- Allow instances of Private Subnets to access internet while remaining private
## 🟦 NAT Instances 
- Are Self-managed
## 🟦 Network ACL
- Also called **NACL** 
- Firewall which **ALLOW / DENY** traffic from & to subnet 
- **Rules only include IP addresses**
- Are attached at the **Subnet level**
## 🟦 Security Groups 
- A **firewall** that controls traffic from & to **ENI** (Elastic Network Interface) / EC2 instance 
- Can only have **ALLOW** rules 
- **Rules include IP Addresses and other security groups**
## 🟦 VPC Flow Logs 
- Capture information about IP Traffic  
  - VPC logs 
  - Subnet logs 
  - Elastic Network Interface (ENI) logs 
- Logs data can be sent to S3 / CloudWatch Logs 
- Use cases
  - **Monitor & troubleshoot connectivity**
## 🟦 VPC Peering 
- Two VPC privately use AWS network 
- Make them behave as if they are on same network 
- CIDR (IP Address range) must not have overlapping 
- Is NOT transitive (must be created for each two VPCs that need to be on same network) 
  - A & B: VPC Peering AB 
  - B & C: VPC Peering BC 
  - A & C: VPC Peering AC 
## 🟦 VPC Endpoints (_very Important for test_) 
- Allow to connect to AWS services using a private network instead of www network
  - AWS Services communicate through public internet
  - **When needing private access from within VPC to AWS service**
  - Create a VPC Endpoint
- **Endpoint Gateway** only supports
  - S3 
  - DynamoDB 
- Endpoint Interface
  - Everything else like CloudWatch 
## 🟦 Site to Site VPN 
- Connect an on-premises VPN to AWS 
- Connection is auto encrypted 
- Goes over public internet 
- Cannot access VPC Endpoints
## 🟦 Direct Connect 
- Establish physical connection between on-premises and AWS 
- Connection is private, secure, fast 
- Goes over pricate network 
- Takes a month to establish
- Cannot access VPC Endpoints
## 🟦 3-Tier-Solution Architecture
1. Public Subnet
    - Route 53
    - ELB
2. Private Subnet
    - ALB
3. Data Subnet
    - ElastiCache
      - Session Data
      - Cached Data
    - Amazon RDS
      - Read
      - Write
## 🟦 LAMP Stack on EC2
1. Linux
    - OS for EC2 instances
2. Apache
    - Web Server that runs on Linux (_EC2_)
3. MySQL
    - Database on RDS
4. PHP
    - Application logic (_running on EC2_)
### Optional
- Can add Redist / Memcached (ElastiCache) to include a caching tech
- To store local app data
  - EBS drive (_root_)
## 🟦 Cheat Sheet
### VPC
Virtual Private Cloud
### Subnets
Tied to an AZ, network partition of the VPC
### Internet Gateway
At the VPC level, provide Internet Access
### NAT Gateway / Instances
Give internet access to private subnets
### NACL
Stateless, subnet rules for inbound and outbound
### Security Groups
Stateful, operate at the EC2 instance level or ENI
### VPC Peering
Connect two VPC with non overlapping IP ranges, non transitive
### VPC Endpoints
Provide private access to AWS Services within VPC
### VPC Flow Logs
Network traffic logs
### Site to Site VPN
VPN over public internet between on-premises DC and AWS
### Direct Connect
Direct private connection to a AWS
# 1️⃣1️⃣ Amazon S3 Introduction
## 🟫 Buckets
- Must have globally unique name 
- Defined at region level 
- Naming convention 
  - No uppercase 
  - No underscore 
  - 3-63 characters long 
  - Not an IP 
  - Must start with 
    - Lower case 
    - Number
## 🟫 Objects 
- Key is the full path 
  - EX1: s3://my-buckeet/my_file.txt 
  - EX2: s3://my-bucket/my_folder/other_folder/my_file.txt 
- Key is composed of  
  - Prefix: my_folder/other_folder/ 
  - Object name:my_file.txt 
- **Limits** 
  - Max size is 5000GB 
  - Max upload size is 5GB, for bigger sizes one must use multi-part upload 
- Metadata 
  - List of key/value pairs 
  - Useful for system of user metadata 
- Tags – maximum 10 
  - Unicode key/value pairs 
  - Useful for security / lifecycle 
- Version ID
## 🟫 Security
### User based 
- IAM Policies 
- Can access object if 
  - IAM permission ALLOW 
  - OR Resource policy ALLOW 
  - AND there is no explicit DENY on the user 
### Resource Based 
- **Bucket Policies**: JSON based – allows cross account 
  - Resource: Buckets & Objects 
  - Actions: Set of API to Allow / Deny 
  - Effect: Allow / Deny 
  - Principal: Account of user to apply policy to 
  - Use Case 
    - Grant public access to the bucket 
    - Force objects to be encrypted at upload 
    - Grant access to another account (Cross Account) 
- Object Access Control List – finger grain 
- Bucket Access Control List – less common 
### Block Public Access 
- Can be set at account level 
- Options 
  - New access control lists 
  - Any access control list 
  - New public bucker or access point policies 
- Use case 
  - Prevent company data leaks 
### Other 
- Networking 
  - Supports VPC Endpoints, for instances in VPC without www internet 
- Logging and Audit 
  - S3 Access Logs can be stored in S3 bucket 
  - API calls can be logged in AWS CloudTrail 
- User Security 
  - MFA Delete: multi factor authentication can be required in versioned buckets to delete objects 
  - Pre-signed URLs: only valid for a limited time 
    - Ex: premium video service for logged in users
## 🟫 Websites
- Host static websites 
- URL 
  - `[bucket-name].s3-website-[aws-region].amazonaws.com`
## 🟫 Versioning (_important_)
- Enabled at bucket level 
- Same key overwrite will increment the version 
- Any non-versioned file prior to enabling versioning will have version "null" 
- Suspending versioning does not delete previous versions 
- Use cases 
  - Protects against unintended deletes 
  - Easy roll back to previous version
## 🟫 Replication
### Pre-requisites 
- Enable Versioning in source + destination 
- Give proper IAM permissions to S3 
### Types 
- SCRR – Cross Region Replication 
  - Compliance 
  - Lower Latency Access 
  - Replication Across Accounts 
- SRR – Same Region Replication 
  - Log aggregation 
  - Live replication between production and test accounts 
### Notes 
- Copying is asynchronous 
- Buckets can be in different accounts 
- Chaining or replication is a NOT ALLOWED. Bucket A => Bucket B => Bucket C 
- After activation, only new objects are replicated 
- After activation, can replicate existing objects using S3 Batch Replication 
- Can replication delete markers from source to target 
- Deletions with a version ID are not replicated (to avoid malicious deletes) 
## 🟫 Encryption (_Can be set on bucket level or object level_)
- SSE-S3: using keys & hanAES-256dled by AWS 
  - AES-256 
  - Set Header: "x-amz-server-side-encryption":"AES256" 
- SSE-KMS: AWS Key Management Service to manage encryption keys 
  - User control 
  - Audit Trail 
  - Set Header: `"x-amz-server-side-encryption":"aws:kms"` 
- SSE-C: Manage your own encryption keys 
  - S3 does not store the encryption keys 
  - Key must bbe provided in HTTPS headers for every request 
  - Retrieving need to provide the key used on uploading the object 
- Client Side Encryption 
  - Encrypt before sending to S3 
  - Decrypt after retrieving from S3
## 🟫 Consistency Model 
- After  
  - Successful write of new object (new PUT) 
  - Overwrite or delete of existing object (overwrite PUT or DELETE) 
- Any 
  - Subsequent read requests receives latest version of the object 
  - Subsequent list immediately reflects changes
## 🟫 Replication
- CRR - (_Cross Region Replication_)
  - We have an S3 bucket in one region and a target S3 bucket in another region
  - We want to setup async replication between two buckets
  - Use cases
    - Compliance
    - Lower latency access to date
    - Replication accross accounts
- SRR - (_Same Region Replication_)
  - Use cases
    - Aggregate logs accross multiple S3 buckets
    - Perform live replication between production and test accounts
- Process
  - Enable versioning in 
    - Source buckets
    - Destination buckets
  - Give proper IAM permission to the S3 service to read/write from S3 buckets
- Notes
  - Only new objects will be replicated
  - To replicate existing objects you have to use **S3 Batch Replication** Feature
    - It replicates existing object and previous failed replicate object
  - In case we have delete replications we can choose to include them in replication
  - If there is a deletion with a version ID they will not be replicated to avoid malicous delete happening from one bucket to another
## 🟫 Storage Classes (_to understand not to remember the numbers_)
  ### Amazon S3 Standard
  - General Purpose
    - Pros
      - Sustain 2 concurrent facility failures (_2 AZ can fail and we will still have a running S3 instance_)
      - Low latency
      - High throughput
    - Use cases
      - Frequently accessed data
      - Big Data analytics
      - Mobile & Gaming Apps
      - Content Distribution
  - Infrequent Access (_IA_)
    - Pros
      - Lower cost
      - Cost on retrieval
    - Use cases
      - Disaster recovery
      - Backups
  ### Amazon S3 One Zone
  - Infrequent Access
    - Pros
      - High Durability
    - Cons
      - Single AZ
      - Data lost when AZ is destroyed
    - Use cases
      - Secondary backup copies
      - Data you can recreate (_thumbnails_)
  ### Amazon S3 Glacier
  - Instant Retrieval
    - Use cases
      - Backup but needs to be Milliseconds (_fast_) retrieval
      - Minimum storage duration: 90 days (_if deleted before you will be charged for the 90 days_)
  - Flexible Retrieval
    - Flexibilities
      - Expedited (_1 - 5 minutes_)
      - Standard (_3 - 5 hours_)
      - Bulk (_5 - 12 hours_) (_free_)
    - Minimum storage duration: 90 days
  - Deep Archive (_long term storage_)
    - Flexibilities
      - Standard (_12 hours_)
      - Bulk (_48 hours_)
    - Minimum storage duration: 180 days
  - Pros
    - Low cost
  - Pricing
    - Price for storage
    - Object retrieval cost
  ### Amazon S3 Intelligent Tiering
  - Pros
    - Move Objects automatically between Access Tiers based on usage
    - No retrieval charges
    - Small monthly monitoring and auto-tiering cost
  - Tiers
    - Frequent Access
      - Automatic
      - Default
    - Infrequent Access
      - Automatic
      - Objects not accessed for 30 days
    - Archive Instant Access
      - Automatic
      - Objects not accessed for 90 days
    - Archive Access
      - Optional
      - Configurable from 90-700+ days
    - Deep Archive Access
      - Optional
      - Configurable from 180-700+ days
# 1️⃣2️⃣ AWS CLI, SDK, IAM Roles & Policies
## 🔶 CLI
- Command: `--dry-run` simulates API call to save resources
## 🔶 CLI STS Decode Errors
- Command: `sts decode-authorization-message –encoded-message <message_value>` decodes error messages 
## 🔶 EC2 Instance Metadata 
-  Allows EC2 instances to learn about themselves **without using an IAM Role for that purpose**
- Can retreive IAM role name
- Cannot retrieve IAM Policy 
- IMDSv1
  - **URL: http://169.254.169.254/latest/meta-data/** 
- IMDSv2
  - Get Session Token (_limited validity_) (_using headers & PUT_)
  - Use Session Token in IMDSv2 calls (_using headers_)
- Note
  - Metadata: Info about EC2 instance 
  - Userdata: launch script of the EC2 instance 
  - **When URL ends with a slash / it means there is more to it or it is a value** 
## 🔶 CLI Profiles
- Problem
  - Managing multiple accounts 
- Solution
  - Configure profile to use in CLI instead of the default one 
- Command 
  - Create: `aws configure –profile <profile_name>` 
  - Use: `aws s3 ls –profile <profile_name>`
## 🔶 MFA with CLI
- Create temp session 
- Run **STS GetSessionToken** API (_important_)
- Command: **aws sts get-session-token –serial-number <arn_of_mfa_device> --token-code <code_from_token> --duration-seconds 3600** 
## 🔶 SDK
- Should know when we should use an SDK 
- If you don't specify a region or configure a default region, us-east-1 will be chosen by default 
## 🔶 Limits
### API Rate Limit 
- DescribeInstances: (for EC2) 100/sec 
- GetObject: (S3) 5500/sec/prefix 
- **For Errors**
  - Intermittent: Implement **Exponential Backoff** 
    - **Use when we get a throttling exception**
      - Implemented by default in SDK
      - Have to implement it manually if using AWS API
    - **ONLY implement retries on 5xx server errors** 
    - **DON'T implement on 4xx errors**
  - Consistent: Request API throttling limit increase 
### Service Quotas (_Service LImits_) 
- Running On-Demand Standard Instances: 1152 vCPU 
- Can  
  - Request service limit increase by opening a ticket 
  - Request a service quota increase by using Service Quotas API 
## 🔶 Credentials Provider Chain (_may come in one question_) (_important_)
### CLI: will look for credentials **in this order**
1. Command line options 
    - --region 
    - --output 
    - --profile 
2. Environment variables 
    - AWS_ACCESS_KEY_ID 
    - AWS_SECRET_ACCESS_KEY 
    - AWS_SESSION_TOKEN 
3. CLI credentials file 
    - Max & Linux: aws configure ~./aws/credentials 
    - Windows: C:\Users\user\.aws\credentials 
4. CLI configuration file 
    - Max & Linux: aws configure ~./aws/config 
    - Windows: C:\Users\user\.aws\config 
5. Container credentials 
    - for ECS tasks 
6. Instance profile credentials 
    - for EC2 Intstance Profiles 

### SDK: will look for credentials in this order (Java examples) 
- Java system properties 
  - Aws.accessKeyId 
  - Aws.secretKey 
- Environment variables 
  - AWS_ACCESS_KEY_ID 
  - AWS_SECRET_ACCESS_KEY 
- Default credential profiles file 
  - At ~/.aws/credentials shared by many SDK 
- Amazon ECS container credentials 
  - For ECS containers 
- Instance profile credentials used on EC2 instaces 
## 🔶 Credentials Best Practices
- NEVER STROE AWS CREDENTIALS IN CODE
- Inherit credentials from the credentials chain 
- Working within AWS, use IAM Roles 
  - EC2 Instances = EC2 Instances Roles 
  - ECS tasks = ECS Roles 
  - Lambda functions = Lambda Roles 
- Working outside AWS 
  - Environment variables 
  - Named Profiles 
## 🔶 Signing AWS API Request
- SDK / CLI : Signed by default 
- Other requests should sign the request **using Signature v4 (SigV4)**
- SigV4 signing request options 
  - HTTP Header
    - In Authorization Header
  - Query String
    - Include the signature in the URL
    - query param is `X-Amz-Signature`
# 1️⃣3️⃣ Advanced Amazon S3
## 🔷 Lifecycle Configuration
### Moving objects between storage classes automatically 
### Rules 
- Transition actions = Defines when objects are transitioned to another storage class 
  - EX1: Move to Standard IA after 60 days of creation 
  - EX2: Move to Glacier after 6 months 
- Expiration actions = Configure objects to expire (delete) after some time 
  - EX1: Access log files after 365 days 
  - EX2: Delete old versions of files 
  - EX3: Delete incomplete multi-part uploads 
- Can apply rules to 
  - A certain prefix (ex – s3://mybucket/mp3/*) 
  - A certain objects tags (ex – Department: Finance) 
- 5 actions 
  - Transition current versions  
  - Transition previous versions 
  - Expire current versions 
  - Permanently delete previous versions 
  - Delete expired delete markers or incomplete multipart uplaods 
## 🔷 Event Notifications
### Process 
- Capture events like 
  - S3:ObjectCreated 
  - S3:ObjectRemoved 
  - Etc... 
- Deliver events to Amazon EventBridge 
- Handle event 
### Notes 
- Can create as many S3 Events as desired 
- Delivered in seconds but can take a minute or longer 
### Destinations to remember 
- SNS 
- SQS 
- Lambda Function 
- Or all to EventBridge 
### Use Case 
- Generate thumbnails of submitted images 
## 🔷 Baseline Performance
- Latency = 100-200ms 
- 3,500 req / sec / prefix 
  - PUT 
  - COPY 
  - POST 
  - DELETE 
- 5,500 req / sec / prefix 
  - GET 
  - HEAD 
### KMS Limitation 
- On upload it calls the GenerateDataKey KMS API 
- On download it calls the Decrypt KMS API 
- Limits varies based on region 
  - 5,500 req/s 
  - 10,000 req/s 
  - 30,000 req/s 
  - Can request quota increase using Service Quotas Console 
### Multi-part Upload 
- Recommended for files > 100MB  
- Must use for files > 5GB 
- Helps parallelize upload 
### Transfer Acceleration 
- Compatible with multi-part upload 
- Increase transfer speed by transferring file to AWS edge location  
- Edge location will transfer data to S3 bucket through AWS private network 
### Byte-Range Fetches 
- Parallelize GET by requesting specific byte ranges 
- Speed up download 
- Retrieve only partial data 
## 🔷 S3 Select & Glacier Select
- Retrieve less data using SQL by performing server-side filtering 
- Filter by rows & columns 
- Less network transfer 
- Less CPU cost (client-side) 
## 🔷 S3 Object Tags & Metadata
### User-Defined Object Metadata
- Assign metadata to objects
- Must begin with "`x-amz-meta-`"
- Metadata is stored as (key-value) pairs
### Object Tags
- Assign tags to objects
- Useful for fine-grained permissions
  - Only access specific objects with specific tags
- Useful for analytics
  - Using S3 Analytics to group by tags
### Notes
- Cannot search object metadata or tags
- To search objects
  - Use an external DB as a search index such as DynamoDB
# 1️⃣4️⃣ Amazon S3 Security
## ⭐  Encryption 
- If unencrypted object was uploaded having a default encryption option will encrypt it 
- How to force encryption 
  - Default encryption 
  - Bucket Policies 
- Bucket Policies are evaluated before default encryption 
## ⭐ CORS
- An origin is 
  - A scheme (protocol) 
  - A host (domain) 
  - A Port 
  - EX: https://www.example.com 
- If a client does cross-origin request on our S3 bucket, we need to enable the correct CORS headers
- Have to set the CORS headers on the accessed bucket website not the one requesting it 
## ⭐ MFA Delete
- Needed to 
  - Permanently delete an object version 
  - Suspend versioning on the bucket 
- Not needed to 
  - Enabling versioning 
  - Listing deleted versions 
- Pre-requisites 
  - Enable Versioning on the S3 bucket 
  - Use root account 
  - Enable MFA device on account 
## ⭐ Access Logs
- Any request made to S3 from any account authorized or denied, will be logged into another S3 bucket 
-Always separate target bucket and logging bucket to avoid logging infinite loop 
## ⭐ Pre-signed URLs
- Using SDK or CLI 
- By default valid for 3600 sec 
- Can edit validity time with –expires-in <time_in_seconds> argument 
- Users with pre-signed URL inherit permissions of person who generated URL for GET/PUT 
### Use cases 
- Allow only logged-in users to download premium video 
- ALlo ever changing list of users to download files by generating URLs dynamically 
- Allow temp a user to upload a file to a precise location in the bucket 
## ⭐ Access Points
### Simplify security management for S3 Buckets
- Instead of assigning user groups with a very complicated S3 Bucket Policy, we can give them access to S3 Access Points
- Each Access Point has its own DNS name
  - Internet Origin (public access)
  - VPC Origin (internal private access)
### VPC Origin
- Define them to be privately accessible
- Create VPC Endpoint to access the Access Point
  - Gateway Endpoint
  - Interface Endpoint
- The VPC Endpoint Policy allows access to target Bucket and Access Point
## ⭐ S3 Object Lambda
### What is it?
- Use AWS Lambda Functions to manipulate the object before it is recieved by the caller application
### How it works?
- S3 Bucket ⤵️
- S3 Access Point ⤵️
- Redacting Lambda Function ⤵️
  - Enriches data e.g. from DynamoDB
- S3 Object Lambda Access Point ⤵️
- Caller ✅
### Use Cases
- Encrypt and decrypt objects 
- Enrich Object with external data for analytics
# 1️⃣5️⃣ AWS CloudFront
## 📶 What is it?
- Content Delivery Network (CDN)
## 📶 Why use it?
- Improve read perfomance
- 216 Point of Presence globally (**edge locations**)
- DDos protection
  - Integration with Sheild
  - AWS Web Application Firewall
## 📶 Origins 
### S3 Bucket 
- Distribute files and chache them at the edge 
- Enhanced Security (Origin Access Identity – OAI) 
### Custom Origin (HTTP) 
- EC2 instance 
  - Must Be Public 
  - Security: Must allow all IP addresses of all Edge Locations 
- Application Load Balancer 
  Must Be Public 
  - Security: Must allow all IP addresses of all Edge Locations 
    - But the EC2 Instances can be private 
- S3 website 
- Any HTTP Backend
## 📶 CloudFront vs S3 Cross Region Replication 
### CloudFront 
- Global Network Edge 
- Cached with TTL 
- Can invalidate cached by creating a cache invalidation 
- Great for static content that must be available everywhere 
### S3 Cross Region Replication 
- Must setup every region 
- Update files in real-time 
- Read only 
- Great for dynamic content that is required at low-latency in a few regions
## 📶 Caching
### How does it work?
- Cache lives in CloudFront Edge Location
- CloudFront identifies each object in the cache using (**Cache Key**)
### Cache Key
- A unique identifier for every object in the cache
- Default is the **hostname** + **resource portion of the URL**
### Cache Policy
- Based on
  - HTTP Headers
    - None
    - Whitelist
  - Coockies
    - None
    - Whitelist
    - Include All-Except
    - All
  - Query Strings
    - None
    - Whitelist
    - Include All-Except
    - All
- Control TTL
  - 0 second to 1 year
  - Can be set by the origin using `Cache-Control` header or `Expires` header
### Origin Request Policy
- Specify values you want to include in origin **without including them in Cache Key**
- Can Include
  - HTTP headers
  - Cookies
  - Query Strings
### Good Practice
- Maximize Cache Hit ratio 
- Enhance cache key by adding more information
  - HTTP Headers
  - Cookies
  - Query Strings
- **ALL** Policy is the worst performing policy
### Notes
- All headers, cookies, and query strings included in the **Cache Key** are automatically included in origin requests
## 📶 Cache Invalidations
### Invalidate Cache
- Invalidate cache for a specific path __e.g. (/images/*)__
- Invalidate cache for all objects __e.g. (*)__
### Why use it?
- In case you updated the back-end origin, users will only get the refreshed content after TTL has expired
## 📶 Cache Behaviors
### What is it?
- Configure different settings for different given URL path patterns
### Why use it?
- Specific cache behavior to __images/*.jpg__ files on your origin web server
- Route to different kind of origins groups based on content type of path pattern
  - __/images/*__
    - Directs to S3 Bucket
  - __/api/*__
    - Sends to ALB
  - __/*__ (default cache behavior)
### Use Cases
- Sign In Page
  - Cache behavior for **/login**
    - Directs to App instance that generates signed cookies
  - Cache behavior for default __/*__
    - Set up to only accept if signed cookies are present
- Maximize Cache Hit by seperating static and dynamic distributions
  - Static goes to S3
  - Dynamic goes to ALB + EC2
### Notes
- The Default Cache Behavior is always the last to be processed and is alwasy __/*__
- The most specific behavior is going to be selected
## 📶 ALB / EC2 As Origin
- EC2
  - EC2 instances **must be public**
  - Ec2 instances must allow all IP addresses of Edge Locations
    - There a list of IP addresses that can be used provided by AWS
- ALB
  - EC2 instances can be private
  - Allow Security Group must be attached to EC2 instance to allow ALB access
  - ALB **must be public**
  - ALB must allow all IP addresses of Edge Locations
## 📶 Geo Restriction
### How does it work?
- Allowlist
- Blocklist
### Use Cases
- Control access to content to follow regulations
## 📶 Signed URL vs Signed Cookies 
### Signed URL  
- One URL per file 
- Process 
  - Trusted Key Group (RECOMMENDED) 
    - Create Trusted Key Groups 
      - Any IAM user with sufficient permissions can create Public Keys and Key Groups 
    - Generate own public / private key (2048 bit) 
      - Private key will be used by app to sign URL 
      - Public key will be used by CloudFront to verify URLs 
  - AWS Account that contains AWS Key Pair (BAD) 
### Signed Cookies 
- One Cookie for many files 
### Signed URL vs S3 Pre-Signed URL

| Signed URL vs                                                                             | S3 Pre-Signed URL                                    |
| ----------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| Allow access to path regardless of the origin                                             | Issue a request as the person who pre-signed the URL |
| ~~Account wide key-pair (only root can manage it)~~ Recommended way is Trusted Key Groups | Uses the IAM key of the signing IAM principle        |
| Can filter by IP, path, date, expiration <br>Can levarage caching features                | Limited lifetime                                     |
## 📶 Pricing (_important_)
### Cost varies based on location 
### Price Classes 
- Price Class All = All regions – best performance 
- Price Class 200 = Most regions – excludes most expensive regions 
- Price Class 100 = Only least expensive regions
## 📶 Multiple Origin 
- Route different kind of origins based on content type 
- Based on path pattern 
  - /images/* => S3 Bucket 
  - /api/* => ALB 
  - /* 
## 📶 Origin Groups 
- Increase high-availability and do failover  
- One primary + one secondary origin 
- Primary origin fails > secondary is used
## 📶 Field Level Encryption 
### Why use it?
- Protect sensitive information through application 
- Uses asymmetric encryption 
### Use Cases 
- Specify field level encryption in POST request 
- Specify the public key to encrypt them
## 📶 Real-Time Logs
### What is it?
- Requests are sent in real-time to Kinesis Data Streams
### Why use it?
- Monitor
- Analyze
- Take Actions
- Base on CDN performance
### Sampling Rate
- Percentage of requests to be recieved
- Specific fields
- Specific Cache Behaviors (path patterns)
# 1️⃣6️⃣ ECS, ECR, & Fargate - Docker in AWS
## 🎁 Amazon ECS – Elastic Container Service
### Launching Docker Containers
- Means Launching ECS Tasks on ECS Clusters
### Launch Types
- EC2 
  - Docker containers placed on EC2 instances that we provision in advanced 
  - You must provision & maintain the infrastructure  
  - Each EC2 instance must run in the ECS Cluster 
  - AWS takes care of starting / stopping containers 
- Fargate 
  - No provision 
  - It is serverless 
  - Runs based on CPU / RAM needed 
  - To scale => increase number of tasks 
### IAM Roles
- EC2 Instance Profile 
  - Works on both EC2 Launch Type 
  - Used by the ECS agent 
  - Makes API calls to 
    - ECS service 
    - Send container logs to CloudWatch Logs 
    - Pull Docker image from ECR 
    - Reference sensitive data in Secrets Manager or SSM Parameter Store 
- ECS Task Role 
  - Works on both EC2 & Fargate Launch Types 
  - Allow each task to have a specific role 
  - Use different roles for different ECS Services 
  - Task Role is defined in task definition
### Load Balancer Integrations 
- ALB = Supported and works for most use cases 
- NLB = Recommended only for 
  - High throughput / high performance use cases 
  - Pair it with AWS Private Link 
- ELB = Supported but not recommended 
  - No advanced features like Fargate 
###  Data Volumes 
- EFS 
  - Mount onto ECS tasks 
  - Because it is a network file system it will be compatible with both EC2 & Fargate 
  - Tasks running in any AZ will share same data  
  - Fargate + EFS = Serverless  
- Use cases 
  - Persistent multi-AZ shared storage for containers 
- Note 
  - ESx For Lustre is not supported 
  - S3 cannot be mouinted as a file system
### AWS Application Auto Scaling 
- Automatically increase / decrease desired number of ECS tasks  
- Based on 
  - Average CPU Utilization  
  - Average Memory Utilization – Scale on RAM 
  - ALB Request Count Per Target – Metri coming from the ALB 
- Scaling 
  - Target Tracking = scale based on target value for a sp  - cific CloudWatch metric 
  - Step Scaling = scale based on specified CloudWatch Alarm 
  - Scheduled Scaling = scale based on a specified date/time (predictable changes) 
  - NOTE 
    - ECS Service Auto Scaling (task level) IS NOT EC2 Auto Scaling (instance level) 
    - Fargate Auto Scaling is more easier because Serverless 
- EC2 Auto Scaling 
  - Auto Scaling Group 
    - Scale ASG based on CPU Utilization 
    - Add EC2 instances over time 
  - ECS Cluster Capacity Provider – Recommended 
    - Automatically provision and scale infrastructure of ECS Tasks 
    - Pairs with an AUto Scaling Group 
    - Add EC2 instances when you miss capacity (CPU, RAM,…)
### Rolling Updates 
- When updating from v1 to v2 control 
  - How many tasks can be started and stopped 
  - In what order 
- By choosing 
  - Minimum healthy percent – 0% 
  - Maximum healthy percent – 200% 
### Architectures 
- ECS Tasks invoked by Event Bridge 
  - Client uploads object to S3 Bucket 
  - Event sent to Amazon EventBridge 
  - Rule on EventBridge runs an ECS Task 
  - The new Task processes the file on the S3 Bucket  
  - Then sends the result to Amazon DynamoDB 
- ECS Tasks invoked by Event Bridge Schedule 
  - AmazonBridge runs a rule every 1 hour 
  - The rule runs an ECS Task 
  - The ECS Task batch processes some files on an S3 Bucket 
- ECS SQS Queue 
  - Messages reach SQS Queue 
  - Services poll for messages from SQS Queue 
  - Process them 
  - ECS Service Auto Scaling is enabled 
  - So the more messages we have the more Tasks we have in our Service 
### Task Definition 
- A metadata in JSON for to tell ECS how to run a Docker container 
- Can define up to 10 container in a Task Definition 
- Contains 
  - Image Name 
  - Port Binding for Container and Host 
  - Memory and CPU required 
  - Environment Variables 
  - Networking information 
  - IAM Role 
  - Logging configuration – ex CloudWatch 
- Loading Balancing 
  - EC2 
    - Dynamic Host Port Mapping if we define only the container port in the task defnition and leave  the host port 
    - The ALB finds the right port on the EC2 instances 
    - Must allow on the EC2 instance's Security Group ANY PORT from the ALB Security Group 
  - Fargate 
    - Each task have unique private IP (through ENI) 
    - Only define container post 
    - Host port is not applicable 
    - Example 
      - ECS ENI Security Group – Allow port 80 from ALB 
      - ALB Security Group – Allow port 80/443 from web 
- IAM Role – Assigned per Task Definition 
  - Assign an ECS Task Role for a Task Definition 
  - Creating an ECS Service from this Task Definition 
  - Each ECS Task automatically inherit the ECS Task Role from the Task Definition 
  - All Tasks within a Service will have access to S3 Bucket for example 
- Environment Variables 
  - Hardcoded = e.g URLs 
  - SSM Parameter Store – sensitive variables = e.g. API keys / shared configs 
  - Secrets Manager – sensitive variables = e.g. DB passwords 
- Environment Files (bulk) 
  - Amazon S3 
- Data Volumes 
  - Share data volumes between multiple containers in the same Task Definiton 
  - Works on EC2 & Fargate Tasks 
  - Ec2 Tasks 
    - Using EC2 instance storage 
    - Tied to lifecycle of the instance 
  - Fargate Tasks 
    - Using ephemeral storage  
    - Tied to the containers storage 
    - 20GB-200GB  
    - 20GB default 
  - Use cases 
    - Share ephemeral data between containers 
    - "Sidecar" container to send metrics/logs to other destination 
      - Separation of concerns
### Tasks Placement – only for ECS on EC2
- Task placement strategy 
  - Where to place Task? / What Task to terminate? 
  - Within the constraints of 
    - CPU 
    - RAM 
    - Available Port 
  - Process 
    - Identify instances that satisfy requirements in the deinition 
    - Identify instances that satisfy constraints 
    - Identify instances that satisfy placement strategies 
    - Select the instances for Task Placement 
  - Types 
    - Binpack 
      - Place tasks based on least available amount of CPU / RAM 
      - Consume an instance before moving to another 
      - Very cost saving
      ```JSON
        "placementStrategy":[
          {
          "field" : "memory",
          "type" : "binpack" 
          }
        ] 
      ```
    - Random 
      - Place tasks randomly 
      ```JSON
      "placementSt rategy":[
        {
          "type": " random" 
        }
      ]
      ``` 
    - Spread 
      - Place tasks evenly across multiple EC2 instance 
      - Maximize high-availability 
        - By spreading across multi-AZ for example 
      ```JSON
      "placementSt rategy": [ 
        {
          "field" : "attribute:ecs.availability-zone", 
          "type" : "spread"
        }
      ]
      ```
- Task placement constraints 
  - Types 
    - distinceInstance 
      - Place each task on a different container 
    - memberOf 
      - Place task on instances that satisfy an expression 
      - Uses the Cluster Query Language 
      ```JSON
      "placementConstraints":[
        {
          "expression": "attribute:ecs.instance—type =~ t2.*" , 
          "type": "memberOf" 
        }
      ] 
      ```
### ECS configuration to make request to AWS services 
- Edit [`/etc/ecs/ecs.config`] to allow 
- `ECS_ENABLE_TASK_IAM_ROLE` 
## 🎁 Amazon EKS – Elastic Kubernetes Service 
- Open-source system for automatic deployment 
- An alternative to ECS, similar goal different API 
- ECS Tasks = EKS Pods 
### Launch Types 
- EC2 
- Fargate 
### Use cases 
- Cloud-agnostic – can be used in any cloud 
- If already using on premis or on a different cloud provider 
## 🎁 Amazon ECR (Amazon Elastic Container Registry) 
- Public repository (Amazon ECR public Gallery gallery.ecr.aws) 
- Private repository 
  - Access is controlled through IAM Roles ( permission error => policy) 
    - Assign IAM Role to EC2 Instance 
- Supports 
  - Vulnerability scanning 
  - Versioning 
  - Lifecycle 
### CLI (_important_)
- Login Command (AWS CLI v2)
  - `aws ecr get-login-password --region `**region**` | docker login --username AWS --password-stdin  `**aws_account_id**`.dkr.ecr.`**region**`.amazonaws.com` 
  - Docker Commands
    - Push
      - `docker push `**aws_account_id**`.dkr.ecr.`**region**`.amazonaws.com/demo:latest`
    - Pull
      - `docker pull `**aws_account_id**`.dkr.ecr.`**region**`.amazonaws.com/demo:latest`
- Notes
  - In case an EC2 instance can't pull a Docker image, check IAM permissions
## 🎁 AWS CoPilot
### What is it?
- CLI tool to build, release, and operate production-ready containerized apps
### Why use it?
- Remove the difficulty of running your app on AppRunner, ECS, and Fargate 
- Focus on building the app rather than setting up the infrastructure
- Automated deployments with one command using CodePipeline
- Deploy to multiple environments
- Troublshoot logs, health statuses...