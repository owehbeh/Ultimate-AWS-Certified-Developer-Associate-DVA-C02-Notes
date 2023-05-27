
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
# 1️⃣7️⃣ AWS Elastic Beanstalk
## 🎋 Tiers
### Web Server Tier
- ELB
  - ASG
    - EC2 Cluster
### Worker Environment Tier
- SQS Queue
  - ASG
    - EC2 Cluster
## 🎋 Deployment Modes / Presets 
### Single Instance 
- Greate for dev 
### High Availability 
- Great for production
## 🎋 Deployment Strategies 
### All-at-Once: Performs in place deployment on all instances. 
- Fastest 
- Downtime 
- No Cost 
- Greate for quick iterations in dev environment
### Rolling: Splits the instances into batches and deploys to one batch at a time. 
- App run below capacity but no downtime 
- No Cost 
- Both versions run simultaneously  
- Can set bucket size 
### Rolling with Additional Batch: Splits the deployments into batches but for the first batch creates new EC2 instances instead of deploying on the existing EC2 instances. 
- App run at capacity 
- Both versions run simultaneously  
- Small added cost 
- Can set bucket size 
- Longer deployment
### Immutable: If you need to deploy with a new instance instead of using an existing instance. 
- Zero downtime 
- High cost 
- Double capacity 
- Longest deployment 
- Quick rollback in case of failures – terminate new ASG 
- Great for prod
### Traffic Splitting: Performs immutable deployment and then forwards percentage of traffic to the new instances for a pre-determined duration of time. If the instances stay healthy, then forward all traffic to new instances and shut down old instances. 
- Canary Testing 
- No app downtime 
- New app version deployed to a temp ASG with same capacity 
- Small % of traffic sent to temp ASG for a time 
- Health is monitored 
- If fail happen automated rollback exec 
- New versions migrated from temp ASG to original 
- Old versions then get terminated 
### Blue/Green
- Because AWS Elastic Beanstalk performs an in-place update when you update your application versions, your application might become unavailable to users for a short period of time. To avoid this, perform a blue/green deployment. To do this, deploy the new version to a separate environment, and then swap the CNAMEs of the two environments to redirect traffic to the new version instantly. 
## 🎋 Lifecycle Policy 
- Elastic Beanstalk can store at most 1000 apps versions 
- To phase out old versions use Lifecycle Policy 
  - Based on time – old versions removed 
  - Based on space – too many verions 
- Versions in use don't get removed 
- Retention: Option not to delete source bundle in S3 to prevent data loss
## 🎋 Extensions 
- In the .ebextensions/ directory in the root of the source code 
- YAML / JSON Format 
- .config extensions – ex= logging.config 
- Can modify some default settings using: option_settings 
- Can add resources such as 
  - RDS 
  - ElastiCache 
  - DynamoDB 
- Resources manage by .ebextensions get deleted if env goes away!
## 🎋 Migration
### Load Balancer 
- Since after creating a Beanstalk env you cannot change the Load Balancer type 
- To migrate 
  1. Create a new env with same config – not clone since cloning copies all config including LB ones 
  2. Deploy app on new env 
  3. Perform a CNAME swap or Route53 update  to shift traffic from old env to new one 
### Decouple RDS
- RDS provisioned with Beanstalk is good for dev / test ONLY but BAD for prod because it is tied to env lifecycle 
- Best for prod is to separate RDS database from Beanstalk configuration and configure through .ebextensions to connect to RDS through connection string 
- To migrate 
  - Create a snapshot of RDS DB – as a safegaurd 
  - Protect RDS from deletion – through RDS console 
  - Create a new Elastic Beanstalk env without RDS 
  - Point app to existing RDS 
  - Perform a [blue/green] CNAME swap or Route 53 update and confirm it is working 
  - Terminate old env – RDS won't be deleted 
  - Delete CloudFormation stack that will be in DELETED_FAILED state since we protected our RDS from deletion
## 🎋 HTTPS  
- Load SSL onto the Load Balancer 
  - From the Console => EB console but Load Balancer configuration 
  - From the Code => .ebextiontions/securelistener-lb.config 
  - SSL certs can be provisioned using ACM (AWS Certificate Manager) or CLI 
  - Must configure a security group rule to allow incoming port 443 HTTPS port 
- Redirect HTTP to HTTPS 
  - Configure instances to redirect HTTP to HTTPS 
  - OR configure ALB with a rule 
  - Health checks should not redirect they should return 200 OK
### Web Server VS Worker Environment 
- Web Server = Application responding to request 
- Worker Environment = Performs tasks that are long to complete 
  - Should offload these tasks to a dedicated worker environment 
  - Decoupling application into two tiers 
- Example 
  - Processing a video 
  - Generating a zip file 
  - Processing an audio 
- Can define periodic tasks in a file cron.yaml
## 🎋 Custom Platform – Advanced 
### Use case 
- App language is not compatible wi Beanstalk 
- App does not use docker 
### Process 
1. Define an AMI using Platform.yaml file 
2. Build that platform using Packer Software – open source tool to create AMIs 
### Custom Platform vs Custom Image (AMI) 
- Platform – create new Beanstalk Platform 
- Image – tweak existing Beanstalk Platform
# 1️⃣8️⃣ AWS CloudFormation
## 🌨️ Infrastructure as code (in a declarative way) 
- No resources are manually created – great for control 
- The code can be version controlled using git 
- Changes to the infrastructure are reviewed through code 
## 🌨️ Cost 
- Resources within the stack are stagged with an id 
  - This enables us to see how much a stack costs 
- We can estimate the costs using the CloudFormation template 
- Savings strategy – in Dev we can automate deletion of templates at 5PM and recreate them at 8AM 
## 🌨️ Productivity 
- Destroy + Re-create infrastructure on the fly 
- Automated generation of Diagrams – good for presentations 😉  
- Declarative programming – no need to figure out ordering & orchestration 
- Leverage existing templates on the web 
- Leverage the documentation 
## 🌨️ Separation of concern 
- VPC stacks 
- Network stacks 
- App stacks 
## 🌨️ Deployment 
### Manual 
- CloudFormation Designer 
- Use console to input params, etc.. 
### Automated – recommended  
- Edit templates in YAML file 
- Use QWS CLI to deploy the templates 
## 🌨️ Templates Components 
### Resources – mandatory  
- AWS::aws-product-name::data-type-name 
- Cannot be dynamic, everything has to be declared 
- Almost all AWS services are supported but not all 
  - Workaround => AWS LAmbda Custom Resources 
### Parameters 
- Input to AWS CloudFormation template 
- FN::Ref / !Ref => to reference a parameter  
  - Can reference parameter or resources 
- AWS offers pseudo params 
  - AWS::AccountId 
  - AWS::NotificationARNs 
  - AWS::NoValue 
  - AWS::Region 
  - AWS::StackId 
  - AWS::StackName 
### Mappings 
- Fixed variables within CloudFormation template 
- Handy to differentiate between dev/prod, regions, AMIs 
- Values are hardcoded 
- Safer control over the template 
- Mapping Example 
  ```YAML
  RegionMap: 
    us-east-1: 
      "32":"ami-35235234" 
      "64":"ami-32531241" 
    us-west-1: 
      "32":"ami-37775234" 
      "64":"ami-36661241" 
  ```
- Access in template example 
  - `Fn::FindInMap` or `!FindInMap [MapName,TopLevelKey,SecondLevelKey] `
  - Example 
    - `ImageId: !FindInMap[RegionMap, !Ref "AWS::Region", 32] `
### Outputs 
- Optional outputs values you can import into other stacks 
- Should be exported first of course 
- Enables collaboration cross stack 
- Cannot delete a CloudFormation stack if it has outputs that are being referenced in another stack
- Example
  - Export
    ```YAML
    Outputs:
      StackSSHSecurityGroup:
        Desctription: Security group of company
        Value: !Ref MyCompanyWideSSHSecurityGroup ⬅️
        Export:
          Name: SSHSecurityGroup ⬅️
    ```
  - Import
      ```YAML
      Resources:
        MySecureInstance:
          Type: AWS::EC2::Instance
          Properties:
            AvailibilityZone: us-east-1a
            ImageId: ami-a4c7edb2
            InstanceType: t2.micro
            SecurityGroups:
              - !ImportValue SSHSecurityGroup ⬅️
      ```
### Conditionals 
- Create resources/outputs based on conditions 
- Can be whatever you want 
- Common ones are 
  - Environment 
    - Dev 
    - Test 
    - Prod 
  - AWS Region 
  - Any param value 
- Each condition can reference 
  - Another condition 
  - Param value 
  - Mapping 
- Intrinsic Function / Logical 
  - Fn::And 
  - Fn::Equals 
  - Fn::If 
  - Fn::Not 
  - Fn::Not 
  - Fn::Or
- Example
  - Create Condition
    ```YAML
    Conditions:
      CreateProdResources: !Equals [!Ref EnvType, prod] ⬅️
    ```
  - Use Condition
    ```YAML
    Resources:
      MountPoint:
        Type: "AWS::EC2::VolumeAttachment"
        Condition: CreateProdResources ⬅️
    ```
## 🌨️ Intrisic Functions 
### `Fn::Ref`
- Reference a 
  - Parameter – Returns the value  
  - Resource – Referencing a resource returns its ID 
### `Fn::GetAtt` 
- Returns a value for a specified attribute  
- Example
  ```YAML
  !GetAtt logicalNameOfResource.attributeName
  ```
### `Fn::FindInMap` 
- Mentioned above 
### `Fn::ImportValue` 
- Mentioned above = Import values that are exported In other templates 
### `Fn::Join`
- Join values with a delimiter 
- !Join [delimiter, [list of values] ] 
- Example 
  ```YAML
  !Join [ ":",  [ a, b, c] ] => "a:b:c"
  ``` 
### `Fn::Sub` 
- Substitutes variables in an input string with values that you specify 
- Example 
  - ```YAML
    Fn::Sub:
      - String
      - Var1Name: Var1Value
        Var2Name: Var2Value
    ```
  - ```YAML
    Fn::Sub: String
    ```  
    - If you're substituting only template parameters, resource logical IDs, or resource attributes in the String parameter, don't specify a variable map. 
### Condition Functions 
- `Fn::And`
- `Fn::Equals` 
- `Fn::If`
- `Fn::Not` 
- `Fn::Not`
- `Fn::Or`
## 🌨️ Rollbacks 
### Stack creation fails 
- Default: everything rolls back (gets deleted). 
  - Check the logs to debug 
- Option to disable rollback and debug what happened 
### Stack Update Fails 
- The stack automatically rolls back to previous known working state  
- Ability to see in the log what happened and error messages 
## 🌨️ Stack Notifications
### What is it?
- Allows you to send stack events to SNS Topic (Email, Lambda, ...)
### How it works?
- Enable SNS integration using stack options
- From that point, any event will be sent to the SNS topic of your choice
- In order to filter for specific events, you can
  - Create a Lambda function that filters specific events
  - The Lambda function calls SNS or any other Service to handle the event
## 🌨️ ChangeSets 
- Provides information about what changes will occur on update
- Does not detect if update will be successful 
## 🌨️ Nested Stacks – Best Practices / Recommended 
### What is it?
- Stacks that ar part of other stacks 
- Allow isolation of repeated patterns 
  - Common components in separate stacks 
### Example 
- Load Balancer configuration that is re-used 
- Security Group that is re-used 
- To update nested stack, always update the parent 
### Nested VS Cross 
- Cross Stacks 
  - Use outputs Export and !ImportValue 
  - Used when in need to pass export to many stacks 
  - Helpful when stacks have different lifecycles
- Nested Stacks 
- Re-use properly configured ALB 
- Important only to the higher level stack – not shared 
- Helpful when components must be re-used
## 🌨️ StackSets 
### What is it?
- Apply stack operation to multiple accounts and region with a single operation 
  - Create 
  - Update 
  - Delete 
### How it works? 
- Admin account creates StackSets 
- Trusted accounts create/update/delete stack instances from StackSets 
### Notes
- When a stack is updated, all associated stack instances are update throughout all accounts and regions 
## 🌨️ Drift 
### What is it?
- Manual configuration changes can be harmful to our stack 
- Drift is used to know if our resources have been drifted 
### Process 
- Detect Drift 
- View Drift 
### Note
- Not all resources are supported yet 
## 🌨️ Stack Policies
### What is it?
- Protect stack against updates
- Define what update actions are allowed on specific resources during stack updates
# 1️⃣9️⃣ AWS Monitoring & Audit: CloudWatch, X-Ray, CloudTrail
## 😎 CloudWatch Metrics
- Every service has metrics 
### Metric 
- Is a variable to monitor 
- Belong to namespaces 
- Have timestamps 
- Examples 
  - CPUUtilization 
  - NetworkIn 
  - Etc... 
### Dimension is an attribute of a metric 
- Instance id 
- Environment 
- Etc... 
- Up to 10 dimensions / metric 
### EC2 Instances 
- Have metrics every 5 minutes 
- Can enable detailed monitoring to make interval 1 minute 
  - This enables scaling ASG faster 
- Ec2 RAM usage is noy pushed by default  and must be pushed using custom metric 
### Custom Metrics 
- Use API call PutMetricData 
- Can use dimensions to segment metrics 
  - Instance.id 
  - Environment.name 
- Metric Resolution 
  - Standard = 1 minute  
  - High Resolution – higher cost 
    - 1s 
    - 5s 
    - 10s 
    - 30s 
- Timestamps 
  - Accepts data points 2 weeks back (past) and 2 hours forward (future) 
  - For example your application might want to report an action/event inside the app itself that occurred 1 week ago but is confirmed at the moment. So the timestamp of the metric datapoint is 1 week ago but the data is created when it is confirmed, now. This is what I figured out after trying to apply some scenarios. 
### Notes
- Can create CloudWatch dashboards of metrics 
## 😎 CloudWatch Logs
- Collect, monitor, analyze and store log files 
- Log groups: arbitrary name representing an app 
- Log stream: instances within app / lop files / containers 
- Log expiration Policies  
  - Never 
  - 30 days 
  - Etc 
- Export logs 
  - Amazon S3 (exports) 
    - Can take up to 12 hourse 
    - API call is CreateExportTask 
    - Not real-time 
  - Subscription Filter (real-time / near real-time) 
    - Kinesis Data Streams 
    - Kinesis Data Firehose 
    - AWS Lambda 
    - ElasticSearch 
## 😎 CloudWatch Alarms
- React in real-time to metrics/events 
- Trigger notifications  for any metric 
### States 
- OK 
- INSUFFICIENT_DATA 
- ALARM 
### Period 
- How long do you want the alarm to evaluate on the metric? 
- Can be ONLY 
  - 10 seconds 
  - 30 seconds 
  - N of 60 seconds 
### Targets 
- EC2 Instance 
  - Stop 
  - Terminate 
  - Reboot 
  - Recover 
- ASG 
  - Trigger 
- SNS 
  - Send notification, then you can do anything 
### Test 
- To test alarms set the state of Alarm using CLI 
  - `aws cloudwatch set-alam-state –alarm-name "myalarm" --state-value ALARM –state-reason "testing"` 
## 😎 CloudWatch Sources
- SDK 
- CloudWatch Agents  
  - No logs from EC2 will go by default to CloudWatch 
  - Must have an IAM Role that allows it to send the log to CloudWatch logs 
  - CloudWatch log agent can be setup on-premises 
  - Types 
    - CloudWatch Logs Agent (depricated) 
      - Only logs  
    - CloudWatch Unified Agent 
      - Additional system-level metrics – While EC2 only have CPU, Disk, and network, no memory no swap. And only at high level! 
        - CPU – active | guest | idle | system | user | steal 
        - Disk – free | used | total 
        - Disk IO – writes | reads | bytes | iops 
        - RAM – free | inactive | used | total | cached 
        - Processes – total | dead | blocked | idle | running | sleep 
        - Swap Space – free | used | used % 
      - Logs 
      - Centralized config using SSM Parameter Store 
- Elastic Beanstalk: logs from app 
- ECS: logs from containers 
- AWS Lambda: logs from functions 
- VPC Flow Logs: VPC Specific logs 
- API Gateway 
- CloudTrail based on filter 
- Route54: logs from DNS queries 
## 😎 CloudWatch Synthetics Canary
### What is it?
- A configurable script that monitors APIs, URLs, Websites...
- Reproduces what customers do programmatically to find issues before customers are impacted
- Take screenshots of the UI
- In case it fails it triggers an alarm that we can handle
### Script Language
- Node.js
- Python
### Blueprints
- Heartbeat monitor
  - Load URL
  - Store screenshot
- API Canary
  - Test basic read/write functions of REST API
- Broken Link Checker
  - Checks all links inside the URL
- Visual Monitoring
  - Compare a screenshot take during canary with a baseline screenshot
- Canary Recorder
  - used with CloudWatch Synthetics Recorder
  - Record actions on website
  - Dynamically generates a script for that
- GUI Workflow Builder
  - Verifies actions can be take on webpage
  - Example
    - Test a webpage with a login form
### Notes
- Has access to headless chromiom browser
- Can run once or on a regular schedule
## 😎 CloudWatch Metric Filters
- Can use filter expressions 
  - Find specific IP 
  - Count occurrences of "error" 
- Can be used to trigger CloudWatch alarms 
- Can add queries to CloudWatch Dashboards 
- Does not filter data, only publishes metric data points for events that happen after the filter was created 
## 😎 Amazon EventBridge
### What is it?
- Send notifications when certain events happen  
### Source 
- Intercept events from services 
  - Ec2 Instance Start 
  - CodeBuild Failure 
  - S3 
  - Trusted Advisor 
  - Etc.. 
- Schedule or Cron 
### Target – JSON payload 
- Compute 
  - Lambda 
  - Batch 
  - ECS task 
- Integration 
  - SQS 
  - SNS 
  - Kinesis Data Streams 
  - Kinesis Data Firehose 
- Orchestration 
  - tep Functions 
  - CodePipeline 
  - CodeBuild 
- Maintenance 
  - SSM 
  - EC2 Actions 
### Event Bus 
- Types 
  - Default – generated by AWS services 
  - Partner – received from SaaS apps like Zendesk, Auth0, …  
  - Custom – own application 
- Can 
  - Be accessed by other AWS accounts 
  - Archive events 
    - All 
    - Filter some 
  - Replay archived events 
### Schema Registry 
- For devs it analyzes events and infer the schema 
- Generates code for the app so devs know how data is structured 
### Resource-based Policy 
- Allow deny events from other accounts / regions 
- Example = EventBridge Bus named (centra-event-bus) has a resource-based policy that allows another account to put events into it 
## 😎 Amazon EventBridge - Multi Account Aggregation
### What is it?
- Centrally mange multiple accounts in AWS
### How does it work?
- Define Event Patter in one of the accounts [a]
- Create a rule for it
- The target in first account [a] is the EventBus in another account [e]
- Repeat for all other accounts [b],[c], and [d]
## 😎 X-Ray
### What is it?
- A service that helps developers analyze and debug distributed applications
### Benefits 
- Troubleshooting app performance and errors 
- Distributed tracing of microservices 
- Understand dependencies in a microservice architecture 
- Pinpoint service issues 
- Review request behavior 
- Find errors and exceptions 
- Check if app is meeting time SLA (latency, time to process a request) 
- Find where I am throttled 
- Identify users that are impacted 
### Tracing 
- Trace every request 
- Trace % or rate per minute 
### Security 
- IAM for authorization 
- KMS for encryption at rest 
### How to enable? (_important_) 
- Import AWS X-Ray SDK 
- Change some code  
- SDK will capture calls 
- Install X-Ray daemon | or | enable X-Ray AWS integration 
  - If using AWS Lambda or other services that already run the X-Ray daemon no need to run it 
- Grant IAM rights to app to write data to X-Ray 
## 😎 X-Ray Instrumentation
### Measure of product 
- Performance 
- Errors 
- Trace information 
## 😎 X-Ray Concepts
### Segments
- Each app / will send them 
### Subsegments
- For more details inside segments 
### Trace
- Segments collected together to form end-to-end trace 
### Sampling
- Decrease amount of requests sent to X-Ray 
- Reservoir – default 1 second 
  - How many request per second 
- Rate – default 5% 
  - Percentage of request after the reservoir is full capacity 
### Annotations
- Key/value pairs used to index traces and use with filters 
  - Good for searching  
### Metadata
- Key/value pairs not indexed not used for searching 
## 😎 X-Ray API
### WRITE 
- `PutTraceSegments` – Uploads segment documents to X-Ray 
- `PutTelemetryRecords` – Used by X-Ray daemon to upload telemetry 
  - SegmentsReceivedCount 
  - SegmentsRejectedCounts 
  - BackendConnectionErrors 
- `GetSamplingRules` – Retrieve all sampling rules 
### READ 
- `GetServiceGraph` – Main Graph 
- `BatchGetTraces` – Retrieves list of traces specified by ID. 
  - Each trace is a collection of segment documents that originates from a single request 
- `GetTraceSummaries` – Retrieves IDs and annotations for traces available for specified time frame  
- `GetTraceGraph` - Retrieves a service graph for one or more specific trace IDs 
## 😎 X-Ray Troubleshoot
### EC2 
- Ensure EC2 IAM Role has proper permissions 
- Ensure EC2 is running X-Ray daemon 
### Elastic Beanstalk 
1. .ebextensions/x-ray-daemon.config 
2.  Option_settings: 
3. Aws:elasticbeanstalk:xray: 
4. XRayEnabled: true 
### AWS Lambda 
- Ensure IAM execution role with proper policy (AWSX-RayWriteOnlyAccess)  
- Ensure X-Ray is imported in the code 
### ECS 
- ECS Cluster – X-Ray container as a Daemon 
  - EC2 
    - App Container 
    - X-Ray Deamon Container 
- ECS Cluster – X-Ray container as a "Side Car" 
  1. EC2 
  2. App Container 
  3. X-Ray Side Car 
- Fargate Cluster – X-Ray container as a "Side Car" 
  1. ECS Cluster 
  2. Fargate Task 
  3. App Container 
  4. X-Ray Side Car 
## 😎 AWS Distro for _OpenTelemetry_
### What is it?
- An AWS-supported distribution of the open-source project OpenTelemetry project
### Why use it?
- It is very similar to X-Ray but open-source
- If a company wants to standardize with open-source APIs 
- Send traces data to multiple destinations simultaneously
### How it works? 
- Auto-instrumentation Agents collect traces without you changing the code
- Send traces and metrics to multiple AWS services
  - X-Ray
  - CloudWatch
  - Prometheus
## 😎 CloudTrail
### What is it?
- Governance, compliance and audit for account 
- Internal monitoring of API calls being made by 
  - Console 
  - SDK 
  - CLI 
  - AWS Services 
- Audit changes to AWS Resources by users 
- A trail can be applied to all regions (default) or a single region 
## 😎 CloudTrail Events
### Management 
- Operations performed on resources, trails are configured to log these by default 
- Can separate Read Events from Write Events 
### Data 
- Not logged by default because of high volume operations 
- Ex 
  - Amazon S3 object-level activity 
      - GetObject 
      - DeleteObject 
      - PutObject 
  - AWS Lambda fn execution activity 
      - Invoke API 
### Insights 
- Detect unusual activity 
    - Inaccurate resource provisioning  
    - Hitting service limits 
    - Bursts of AWS IAM actions 
    - Gaps in periodic maintenance activity 
- How? 
  - Analyzes normal management events to create a baseline 
  - Continuously analyzes write events to detect unusual patterns
- Retention 
  - Default is 90 days in CloudTrail 
  - To keep events beyond 90 days > log them to S3 and use Athena 
## 😎 CloudTrail vs EventBridge vs X-Ray

| CloudTrail                                                   | CloudWatch                                                                       | X-Ray                                                                     |
| ------------------------------------------------------------ | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Audit API calls made by [users, services, AWS console]       | Metrics for monitoring, Logs, and Alarms to send notifications                   | Automated Trace Analysis & Central Service Map Visualization              |
| Useful to detect unauthorized calls or root cause of changes | Useful to track app performance for optimization and handle unexpected behaviors | Useful for Fault Analysis and request tracking across distributed systems |
# 2️⃣0️⃣ AWS Integration & Messaging: SQS, SNS, & Kinesis
## 🎙️ SQS
- Simple Queuing Service – used to decoupling applications
### Attributes
- Unlimited throughput – we can send as many messages per second 
- Unlimited number of messages in queue 
- **Message life**: 4 days default. Maximum 14 days. Minimum 1 minute 
- Low latency = < 10ms on publish and receive 
- Message size: **256KB max**
- Can have duplicate messages 
- Can have out of order messages 
### Producer
- send messages to SQS using SDK 
- API = **SendMessage** 
- Message is persisted until a consumer deletes it 
- Message retention: 4 days, max 14 days 
### Consumer
- Runs on 
  - EC2  
  - On premise server 
  - AWS Lambda 
- Receives up to 10 messages at a time 
- Processes the message – ex: insert into RDS database 
- Delete the messages using the DeleteMessage API 
### SQS With ASG
           +-----------------+
           |                 |
           |   EC2 Instances |--------+
           |                 |        |
           +-------+---------+        |
                   |                  |
                   |                  |
                   |                  |
         +---------+---------+        |
         |                   |        |
         |   Auto Scaling    |        |
         |    Group (ASG)    |        |
         |                   |        |
         +---------+---------+        |
                   |                  |
                   |                  |
                   |                  | Polling Messages
         +---------+---------+        |
         |                   |        |
         |   CloudWatch      |        |
         |    Alarm          |        |
         |                   |        |
         +---------+---------+        |
                   |                  |
                   |                  |
                   |                  |
         +---------+---------+        |
         |                   |        |
         |   SQS Queue       |<-------+
         |                   |
         +---------+---------+

### Encryption
- HTTPS – In-flight 
- KMS keys – At-rest 
- Client-sided – client handles encryption/decryption 
### Access Policies
- IAM policies to regulate access to SQS API
- Access Policies to enable cross-account access 
  - Similar to S3 bucket policies 
  - Useful for cross-account access to SQS 
  - Useful for allowing other services like (SNS, S3...) to write to an SQS queue 
    ```JSON
    +-------------------------------------+
    |            Account A                |
    +-------------------------------------+
    |                                     |
    |            SQS Queue                |
    |                                     |
    +-------------------------------------+
                    ||
                    ||
                    \/
    +-------------------------------------+
    |            Account B                |
    +-------------------------------------+
    |                                     |
    |            EC2 Instance             |
    |                                     |
    |    Polling for messages from        |
    |    Account A's SQS Queue            |
    |                                     |
    +-------------------------------------+
    {
      "Version": "2012-10-17",
      "Statement": [{
          "Effect": "Allow",
          "Action": "sqs:*",
          "Principle":{"AWS":["B"]}
          "Resource": "arn:aws:sqs:*:A:my_queue_name*"
      }]
    }
    ```
### Visibility Timeout
- Message gets polled by consumer 
- Message is invisible for **30 seconds (default)**
- Message is processed and deleted by consumer 
- If message is not deleted it will be read by other or same consumer again 
- If first consumer is not done processing the message within the visibility timeout other consumers will poll it and it will be processed twice 
- We can change default Visibility time from management console 
- We can call **ChangeMessageVisibiliity** API to get more time 
### Dead Letter Queue
- Useful for debugging! 
- After **MaximumReceives** threshold is exceeded 
- Message goes into a dead letter queue (DLQ)  
- Messages in DLQ expire after 1minute - 14 days (14 days is best option) 
### Delay Queue
- Default = 0 seconds 
- Can be set up to = 15 minutes 
- Can set a per message delay using the DelaySeconds parameter 
### Certified Developer Concepts (_important_)
- Long Polling (_recommended_)
  - Wait for messages to arrive if there are none in the queue 
  - Decreases the number of API calls made to SQS 
  - Increases efficiency 
  - Lowers latency 
  - Wait time = 1 second to 20 seconds 
  - Can be set 
    - On queue level 
    - API level using WaitTimeSeconds 
- Extended Client
  - Send large files above 256kb size 
  - Using SQS Extended Client (Java Library)
    ```
    +----------+      +--------------+      +--------------+
    | Producer |----->|  SQS Queue   |      |  Consumer    |
    +----------+      +--------------+      +--------------+
          |                  |                  |
          v                  |                  |
    Small metadata message -->|----------------->|
          |                  |                  |
          v                  |                  |
          |                  |                  |
          v                  |                  |
    Large message  ---------->|  S3 Bucket       |
                              |                  |
                              |                  |
                              |                  |
                              |<----- Retrieve large message
    ```
- Must Know APIs (_important_)
  - `CreateQueue` 
    - MessageRetentionPeriod 
  - `DeleteQueue` 
  - `PurgeQueue` – Delete all messages 
  - `SendMessage` 
    - DelaySeconds 
  - `ReceiveMessage` 
    - MaxNumberOfMessages – default 1, max 10  
  - `DeleteMessage` 
  - `ReceiveMessageWaitTimeSeconds` – Long Polling 
  - `ChangeMessageVisibility` – change message timeout in case need more time to process a message 
  - **Batch APIs** help decrease costs, they exist for 
    - `SendMessage` 
    - `DeleteMessage` 
    - `ChangeMessageVisibility` 
### Redrive to Source
- After fixing the reason of the failed to process message 
- We can redrive messages from DQL back to source queue without writing any code 
### FIFO Queue
- Limited throughput 
  - No batching = 300 messages/second 
  - With batching = 3000 messages / seconds 
- Exactly-once send – no duplicates 
- Messages are processed in order by the consumer 
- Name has to end with .fifo 
- Deduplication 
  - Interval 5 minutes 
  - Methods 
    - Content-based – will do a SHA-256 hash of message body 
    - Explicitly provide a Message Duplication ID 
- Message Grouping 
  -  All messages will be in order for the same consumer 
  - Have to specify MessageGroupID 
  - Different Group ID can have different consumer – parallel processing 
  - Ordering across groups is not gauranteed 
### SQS + SNS - Fan Out Pattern
```
                                                                     +---------------------+
                                                                     |                     |
                                                                    /| SQS Queue 1         |
                                                                   / |                     |
                                                                  /  +---------------------+
                                                                 /                          
                                                               /-                           
                                                              /      +---------------------+
                                                             /       |                     |
                                                            /        | SQS Queue 2         |
                +---------------+          +----------------       /-|                     |
                |               |          |               |     /-  +---------------------+
                |               |          |               |   /-                           
 S3 Object      |   S3 Bucket   |          |    SNS        | /-                             
 Created        |               | ---------+    TOPIC      --                               
 Event          |               |          |               |                                
                |               |          |               |             +--------------+   
                +---------------+          +---------------+--\          |              |   
                                                               ---\      |              |   
                                                                   ---\  |   Lambda     |   
                                                                       --|   Function   |   
                                                                         |              |   
                                                                         |              |   
                                                                         +--------------+   
```
## 🎙️ SNS
- Pub/Sub with Topic param 
### Subscribers
- SQS 
- Lambda 
- Kinesis Data Firehose 
- Emails 
- SMS 
- Mobile notifications 
- HTTPS Endpoints 
### Publishers
- CloudWatch Alarms 
- AWS budgets 
- Lambda 
- ASG Notifications 
- S3 Bucket events 
### Process to Publish
- SDK 
  - Create topic 
  - Subscribe 
  - Publish to topic 
- Direct Publish – for mobile apps SDK 
  - Create platform app 
  - Create platform endpoint 
  - Publish to platform endpoint 
  - Works with 
    - GCM – Google  
    - APNS – Apple  
    - ADM – Amazon  
### Encryption
- HTTPS 
- KSM 
- Client-side 
### Access Controls 
- IAM policies 
### Access Policies
- Similar to S3 bucket policies 
- Useful for cross-account access to topics 
- Useful for allowing other services like S3 to write to an SNS topic 
### Message Filtering
- JSON policy used to filter mesages sent to SNS topic's subscription
- If a subscription does not have a filter policy, it receives every message
## 🎙️ Kinesis (_important_)
### Data Streams
- Stream big data 
- Streams are made of Shards 
### Producers
- Send data into Kinesis Data Streams 
- API = `PutRecord` 
- Types 
  - Application 
  - Client 
  - SDK – Simple producer 
  - KPL – Kinesis Producer Library – **Enables**  
    - Batch – **use** with `PutRecords` to **reduce costs**
    - Compression 
    - Retries 
  - Kinesis Agent – monitor log files 
- Record – Sent by producers 
  - Partition Key – define which shard will the record go to 
  - Data Blob – up to 1MB  
  - **Speed** = 1MB/second or 1000 message/second 
- `ProvisionedThroughputExceeded` Error 
  - When one shard is overwhelmed with throughput = more than 1 records/sec it will throw this error 
  - **Solutions** 
    - Use highly distributed partition key – like user_id 
    - Exponential Backoff Retries   
    - Increase shards – scaling  
### Consumers
- Apps (KCL – Kinesis Client Library / SDK) 
- Lambda  
- Kinesis Data Firehose 
- Kinesis Data Analytics 
- Record – Received by consumer 
  - Partition Key – define which shard will the record go to 
  - Sequence no. - where the record was in the shard 
  - Data Blob 
  - Speed 
    - Classic Fan-out– 2MB/second/shard for all consumers 
      - API – GetRecords() 
      - Pull approach 
      - Max 5 GetRecords API calls / second 
      - Latency 200ms 
      - Minimize Cost  
    - Enhanced Fan-out – 2MB/second/shard/consumers 
      - API – SubscribeToShard() 
      - Push approach by shard 
      - Latency 70ms because of the subscribe approach 
      - 5 Consumers apps (KCL) per data stream 
        - Can be upgraded by submitting a ticket to AWS  
    - Lambda 
      - Supports both Classic & Enhanced 
      - Read records in batches 
        - Configure batch size 
        - Process up to 10 batches per Shard simultaneously 
      - Errors = Lambda retries until succeeds or data expired
### Retention
- 1 day – 365 days 
- Can reprocess / replay data
- Data can't be deleted – **immutable**
- Data that shares the same partition key go to same shard (key based ordering) 
### Capacity Modes 
- Provisioned – historic > choose if you know capacity in advanced 
  - Choose number of shards – scale manually or using API 
  - Data in = Each shard gets 1MB/second or 1000 record /second 
  - Data out 
    - Classic = Each shard gets 2MB/second  
    - Enhanced = Fan-out consumer (SNS => SQS) 
  - Cost = Pay per Shard-provisioned/hour 
- On-demand – new > choose if you don't know capacity in advanced 
  - Capacity will be adjusted on demand 
  - Default capacity provisioned 
    - 4MB/second 
    - 4000 record/second 
  - Scales automatically based on observed throughput peak during last 30 days 
  - Cost = Pay per stream/hour & data in or out per GB 
### Client Library (KC)
- A Java Library that helps read record from Kinesis Data Stream with distributed sharing the read workload
- Each shard is to be read by only one KCL instance
  - 4 shards = max 4 KCL instance (EC2 instances running KCL)
  - 6 shards = max 6 KCL instance (EC2 instances running KCL)
### Operations
- Shard Splitting
  - Used to increase the Stream capacity
  - Used to divide a "**hot shard**"
  - 1MB/s data per shard > 2MB/s per two shards
  - Cannot split a shard into more than two shard in a single operation
- Shard Merging
  - Decrease the Stream capacity
  - Decrease the cost
  - Group two shards with low traffic "**cold shards**"
### Data Firehose
- Near RealTime
  - 60 seconds latency minimum for non full batches
  - Or minimum 1MB of datat at a time
- Can take data from producers
  - Kenises Data Streams (_Most Common_)
  - Applications
  - Client (mobile, browser, pc)
  - SDK
  - KPL
  - Kinesis Agent
  - AWS CloudWatch (logs & event)
  - AWS IoT
- Destinations
  - Amazon S3
  - Amazon Redshift
    - Writes data to Amazon S3
    - Data Firehose issues a request to copy data from S3 to Redshift
  - Amazon OpenSearch
  - HTTP custom destination
  - 3rd party 
    - mongoDB
    - Datadog
    - etc...
- Faild Data
  - Can be sent to a backup S3 Bucket
### Data Streams vs Data Firehose (_important_)

| Data Streams                                                                                 | Data Firehose                                                       |
| -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| -- Streaming Service for ingesting data at scale<br>-- Write custom code (producer/consumer) | Load streaming data into S3/Redshit/OpenSearch/3rd pary/Custom HTTP |
| Manage Scaling (shard splitting/merging)                                                     | Fully Managed                                                       |
| Real-time (200ms)                                                                            | Near real-time (buffer time min. 60sec)                             |
| Data storage 1-365 days                                                                      | No data storage                                                     |
| Supports replay capability                                                                   | No replay capability                                                |
### Data Analytics
- For **SQL application**
  - Sources
    - Kenises Data Streams
    - Kenises Data Firehose
  - SQL Statement to perform real-time analytics
    - Or join some reference data from S3 
  - Sinks
    - Kinesis Data Streams
      - AWS Lambda
      - Applications
    - Kinesis Firehose
      - Amazon S3
      - Amazon Redshift
      - etc...
- For **Apache Flink**
  - A lot more powerful than just standard SQL
  - Sources
    - Kinesis Data Streams
    - Amazon MSK
### Data Ordering for Kinesis vs SQS FIFO
- Kinesis
  - Use a unique partition key different data distributes equally on shards
- SQS FIFO
  - Use Group ID so each group will maintain the same order within this group
## 🎙️ SQS vs SNS vs Kinesis
| Attribute     | SQS                                                                                         | SNS                                                                                                       | Kinesis                                                                                                  |
| ------------- | ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **DATA**      | Consumer pulls data<br>Data is deleted after being consumed                                 | Pub/Sub<br>Push data to many subscribers<br>Data is not persisted (lost if not delivered)                 | Standard: pull data (2MB/shard)<br>Enhanced-fan out: push data (2MB/shard/consumer)<br>Can replay data   |
| **Provision** | Managed Service<br>No need to provision throughput<br>Can have as many consumers as we want | Managed Service<br>No need to provision throughput<br>Up to 12,500,00 subscribers<br>Up to 100,000 topics | Promisioned Mode OR on-demand capacity mode                                                              |
| **Other**     | Ordering gaurantees only on FIFO queues<br>Individual message delay capability              | Integrates with SQS for fan-out architecture<br>FIFO capability for SQS FIFO                              | Ordering at shard level<br>Data expires after X days<br>Meant for real-time big data, analytics, and ETL |
# 2️⃣1️⃣ AWS Serverless: Lambda
## 🧮 What is it?
- Virtual functions, deploy and run, no need to manage servers
- Run on-demand
- Scales automatically
## 🧮 Why use it?
- Easy pricing
- Easy monitoring
- Easy integration with whole AWS suite of services
## 🧮 Language Support
- Node.js
- Python
- Java
- C#
- Golang
- Ruby
- Custom Runtime
## 🧮 Synchronous Invocations
- Results returned right away
- Client-side error handling
### Services
- User Invoked
  - CLI
  - SDK
  - API Gateway
  - Application Load Balancer
- Service Invoked
  - Amazon Cognito
  - AWS Step Functions
## 🧮 Application Load Balancer
- Expose Lambda Function as HTTP(S) endpoint
- Lambda function must be registered in a **target group**
### Multi-Header Values
- When headers contain same keys, each with a different value
- If enabled, values are delivered as an array to the event
### Permissions
- Create Lambda Resource Policy to allow invocation from ALB
## 🧮 Asynchronous Invocations & DLQ
- Allows speeding up processing
- Does not wait for the function to finish
### Services
- S3
- SNS
- CloudWatch Events
- EventBridge
### Use Case
- Need to process 1000 file
- Generate thumbnails (S3)
  - Enable versioning to ensure event notification is sent for every successful write
### Notes
- On failure, multiple log entries will show in CloudWatch log events with the same invoke ID
- Can define DLQ (SNS/SQS)
  - Requires IAM permissions
## 🧮 Event Source Mapping
- Lambda function invokes synchronously
### Services
- Kinesis Data & DynamoDB Streams
  - Creates an iterator for each shard
  - Processes items in order
  - Processed items are not removed from stream (other consumers can read them)
  - Can process multiple batches in parallel
    - Up to 10 batches / shard
    - Order gauranteed for each partition key
  - Error Handling
    - Entire batch is reprocessed until function succeeds or item expires
    - **Discarded events can go to a Destination**
    - Can configure to
      - Discard old events
      - Restrict number of retries
- SQS & SQS FIFO Queue
  - Long Polling automatically applied
  - Batch size (1-10)
  - Can use DLQ
    - Set-up on SQS not Lambda (DLQ for Lambda is Async only)
    - Or use Lambda Destination for failures
  - FIFO Lambda scales up to the number of **active message groups**
### Scaling
- Kinesis Data & DynamoDB Streams
  - One Lambda invocation per stream shard
  - Up to 10 batches if using parallelization
- SQS Standard
  - 60 added instances per minute to scale up
  - Up to 1000 batches simultaneous processing
- SQS FIFO
  - Messages with same GroupID processed in order
  - Lambda scales to the number of active message groups
## 🧮 Event & Context Objects
### Event Object
- Data for the function to process
### Context Object
- Information about the invocation, function, and runtime environment
## 🧮 Destinations
- Define destinations for success and failed events
### Services
- Async
  - SQS
  - SNS
  - Lambda
  - EventBridge
- Sync
  - SQS
  - SNS
### Recommended
- Use destination instead of DLQ
### Note
- Can send events from SQS to DLQ directly
## 🧮 Permissions - IAM Roles & Resource Policies
### Lambda Execution Role
- Grant permission to AWS Services / Resources
### Example
- When using Event Source Mapping, Lambda uses execution role to read event data
### Resource Based Policies
- Use to give another AWS account & services permission
## 🧮 Environment Variables
### What is it?
- Key/Value pair passed to event and available in Function code
### Why use it?
- Adjust function behavior without updating code
### Use Cases
- DynamoDB URL
- API Key
## 🧮 Monitoring & X-Ray Tracing
### CloudWatch Logs
- Requires IAM Policy to authorize Lambda to write to CloudWatch Logs
### CloudWatch Metrics
- Information about
  - Invocations
  - Durations
  - Concurrent Executions
  - Erro Count
  - Success Rate
  - Throttles
  - Async Delivery Failure
  - Iterator Age (Kinesis & DynamoDB Streams)
### X-Ray Tracing
- Enable in Lambda (**Active Tracing**)
- Use AWS X-Ray SDK in code for customization
- Requires IAM Execution Role
  - `AWSXRayDaemonWriteAccess`
- Environment variables passed to Lambda
  - `_X_AMZN_TRACE_ID`: contains the tracing header
  - `AWS_XRAY_CONTEXT_MISSING`: by default, LOG_ERROR
  - `AWS_XRAY_DAEMON_ADDRESS`: the X-Ray Daemon IP_ADDRESS:PORT
## 🧮 Lambda@Edge & CloudFront Functions
### CloudFront Functions
- Written in JS
- Used to change
  - Viewer Request
    - After it's received in CloudFront
  - Viewer Response
    - Before sent to viewer
- Millions req/sec
- <1ms
### Lambda@Edge
- Written in Node.js / Python
- Used to change
  - Viewer Request
    - After it's received in CloudFront
  - Origin Request
    - Before forwarded to origin (Lambda)
  - Origin Response
    - After received from origin (Lambda)
  - Viewer Response
    - Before sent to viewer
- 5-10sec
### Use Case
- CloudFront Functions
  - Customize CDN content
  - SEO
  - Intelligent Route
  - User Auth
- Lambda@Edge
  - Real-time Image Transformation
    - Send images in different resolution based on devices
  - Modify headers
  - Generate new HTTP response
## 🧮 VPC
### Give Access to Services in a VPC
- Define
  - VPC ID
  - Subnets
  - Security Groups
- Lambda Creates an ENI in your subnets
- Lambda adds `AWSLambdaVPCAccessExecutionRole`
- Use **VPC endpoints** if in a different VPC
### Give Access to Internet
- Lambda in public subnet **does not have internet access**
- How to?
  - Deploy in a Private subnet
  - Link it to a NAT Gateway in a Public subnet with internet access
## 🧮 Function Perfromance
- Ram: 128MB - 10GB
  - CPU bounded to RAM, more RAM = more CPU
- Timeout: 1sec-15min
- Execution context
  - Reserved as long as the Function is warm
  - DB connection should be outside the handler to re-use accross invocations
- /tmp space: 10GB Max Size
  - Remains as context remains
  - For permanent persistence, use S3
## 🧮 Layers
### Use Cases
- Externalize Dependencies to re-use them
- Custom Runtimes
  - Rust
  - C++
## 🧮 File System Mounting
- Use EFS
- Configure Lambda to mount EFS to local directory during initialization 
- Leverage EFS Access Points
### Note
- EFS has connection limits
- One Function = one connection
## 🧮 Concurrency
- Up to 1000 concurrent executions
- Can open support ticket to request higher limit
### Throttle Behavior
- Sync
  - return `ThrottleError` - `429`
- Async
  - Retry automatically
  - DLQ Finally
### Tricky Behavior
- A Lambda Function causing throttling causes other Lambda Functions to fail
- Example
  - ALB + Function A reached limit
  - API Gateway + Function B fails
### Provisioned Concurrency
- Allows heating functions in advance
- No cold starts
- Application Auto Scaling can manage concurrency
## 🧮 External Dependencies
- Install packages alongside code
- Zip it together
- Upload file to Lambda if < 50MB
- Upload to S3 if > 50MB
## 🧮 CloudFormation
### Inline
- Very simple
- Use `Code.ZipFile`
- Cannot include dependencies
### Through S3
- Store Lambda code in S3 as `.zip` file
- Refer S3 zip location in CloudFormation template
  - `S3Bucket`
  - `S3Key`: Full path to zip
  - `S3ObjectVersion`: If versioned bucket
## 🧮 Container Images
### What is it?
- Deploy Lambda Functions as containers up to 10GB size from ECR
### Base Images
- Node.js
- Java
- .NET
- Go
- Ruby
### Custom Images
- Must implement Lambda Runtime API
### Best Practices
- Use AWS-provided Base Images
- Use Multi-Stage Builds
  - Discard preliminary steps
  - Only copy final artifacts needed in final container image
- Use single repository for Functions with Large Layers
## 🧮 Versions & Aliases
### Versions
- Immutable
### Aliases
- Mutable
- Pointers to Versions
- Use Cases
  - Enable Canary deployment
  - Enable stable configuration
    - Dev
    - Test
    - Prod
- Notes
  - Have their own ARN
  - Cannot reference other Aliases
## 🧮 CodeDeploye
- Help automate traffic shift for Lambda Aliases
### How?
- Linear: Grow traffic every N minutes till 100%
- Canary: Send X% then 100% when we are sure
- AllAtOnce: immediate (_not recommended_)
### AppSpec.yml Configuration
- `Name` (required) – the name of the Lambda function to deploy
- `Alias` (required) – the name of the alias to the Lambda function
- `CurrentVersion` (required) – the version of the Lambda function traffic currently points to
- `TargetVersion` (required) – the version of the Lambda function traffic is shifted to
## 🧮 Function URL
### What is it?
- Dedicated HTTP(S) endpoint
- Can call Alias / $LATEST
### Security
- CORS
- Resource-based Policy
  - `NONE`
  - `AWS_IAM`
    - Same Account
      - Identity based **OR** Resource based = ALLOW
    - Cross Account
      - Identity based **AND** Resource based = ALLOW
## 🧮 CodeGuru Integration
- Gain insights on runtime performance
- Creates a Profiler Group for Function
### Languages
- Java
- Python
## 🧮 Limits
### Execution
- Memory: 128MB - 10GB
- Time: 15min < 
- Environment Variable: 4KB <
- Disk Capacity: /tmp folder: 512MB - 10GB
- Concurrency Executions: 1000 (can be increased)
### Deployment
- Compressed .Zip: 50MB
- Uncompressed Deployment (Code + Dependencies): 250MB
- Environment Variable: 4KB <
## 🧮 Best Practices
- Perform heavy-duty work outside of your function handler
  - Connect to databases outside of your function handler
  - Initialize the AWS SDK outside of your function handler
  - Pull in dependencies or datasets outside of your function handler
- Use environment variables for:
  - Database Connection Strings, S3 bucket, etc… don’t put these values in your code
  - Passwords, sensitive values… they can be encrypted using KMS
- Minimize your deployment package size to its runtime necessities.
  - Break down the function if need be
  - Remember the AWS Lambda limits
  - Use Layers where necessary
- Avoid using recursive code, never have a Lambda function call itself
# 2️⃣2️⃣ AWS Serverless: DynamoDB
## 🧱 What is it?
- Fully managed NoSQL databases are 
  - Non-relational databases and are distributed
  - Do not support query joins
  - Scales Horizontally
## 🧱 How it works?
DynamoDB is made of Tables
- Each table has a Primary Key (must be decided at creation time)
- Each table can have an infinite number of items (= rows)
- Each item has attributes (can be added over time – can be null)
- Maximum size of an item is 400KB
## 🧱 Supported Data Types
- Scalar Types
  - String
  - Number
  - Binary
  - Boolean
  - Null
- Document Types
  - List
  - Map
- Set Types
  - String Set
  - Number Set
  - Binary Set
## 🧱 Primary Keys
### Partition Key (HASH)
- Must be unique
### Partition Key + Sort Key (HASH + RANGE)
- Combination must be unique
- Data is groupred by partition key
## 🧱 Capacity Modes
### Provisioned Mode (default)
- Specify number of reads/writes per second
- Provide RCU & WCU
- Can tap to **Burst Capacity**
- Can setup auto-scaling to meet demand
- If Burst Capacity is consumed it throws `ProvisionedThroughputExceededException`
- Best Practice
  - Exponential Backoff retry
### On-Demand Mode
- Automatically scales
- Pay for what you use
- More expensive
### WCU
- 1 WCU = 1 Write / Second = 1KB size
- Rounded to 1, so 2.5KB requires 3WCU
### RCU
- Strongly Consistent Reads
  - 1 RCU = 2 Strongly Consistent Reads / Second = 4KB
- Eventually Consistent Reads
  - 1 RCU = 1 Strongly Consistent Reads / Second = 4KB
- Rounded to 4, so 6KB requires 2RCU 
### Throttling
- Reasons
  - Hot Keys - One partition key is being read too many times (e.g. popular item)
  - Hot Partition - When partition key is very unique, since every single partition key always goes to same partition
  - Very Large Items
- **Solutions**
  - Exponential Backoff
  - Distribute partition keys
  - If RCU - Use DynamoDB Accelerator (**DAX**)
### Autoscaling
- Minimum
- Maximum
- Target Utilization %
## 🧱 Operations
### Writing Data
- `PutItem`
  - New item or fully replace (Same primary key)
- `UpdateItem`
  - Edit existing item's attributes OR adds new item if primary key not found
- Conditional Writes
  - Accept a write/update/delete only conditionally
  - Otherwise returns an error
  - Why use it?
    - Helps with concurrent access to items
### Read Data
- `GetItem`
  - Read based on Primary key
  - Primary Key can be HASH or HASH+RANGE
  - Eventually Consistent Read (default)
  - Option to use Strongly Consistent Reads (more RCU - might take longer)
  - `ProjectionExpression` can be specified to retrieve only certain attributes
- `Query` - returns items based on:
  - KeyConditionExpression
    - Partition Key value (must be = operator) – required
    - Sort Key value (=, <, <=, >, >=, Between, Begins with) – optional
  - FilterExpression
    - Additional filtering after the Query operation (before data returned to you)
    - Use only with non-key attributes (does not allow HASH or RANGE attributes)
  - Returns:
    - The number of items specified in Limit
    - Or up to 1 MB of data
- `Scan`
  - Export entire table
  - Consumes a lot of RCU
- `DeleteItem`
  - Delete an individual item
  - Ability to perform a conditional delete
- `DeleteTable`
  - Delete a whole table and all its items
  - Much quicker deletion than calling DeleteItem on all items
### Batch Operations
- `BatchWriteItem`
  - Up to
    - 25 PutItem and/or DeleteItem in one call
    - 16 MB of data written
    - 400 KB of data per item
  - Can’t update items (use UpdateItem)
  - **UnprocessedItems** for failed write operations (exponential backoff or add WCU)
- `BatchGetItem`
  - Return items from one or more tables
  - Up to
    - 100 items
    - 16 MB of data
  - Items are retrieved in parallel to minimize latency
  - **UnprocessedKeys** for failed read operations (exponential backoff or add RCU)
### PartiQL
- SQL-compatible query language for DynamoDB
- Can
  - Select
  - Insert
  - Update
  - Delete
- Cannot 
  - Join
  - Run on multiple tables
## 🧱 Conditional Writes
### Supported Operations
- PutItem
- UpdateItem
- DeleteItem
- BatchWriteItem
### Conditions
- `attribute_exists`
- `attribute_not_exists`
- `attribute_type`
- `contains` (for string)
- `begins_with` (for string)
- ProductCategory `IN` (:cat1, :cat2) and Price `between` :low and :high
- `size` (string length)
### Note
- _Filter Expression_ filters the results of read queries
- _Condition Expressions_ are for write operations
### Use Cases
- Make sure item is not overwritten
- Make sure partition / sort key is not overwritten
- Remove all HTTP urls from our db for security
## 🧱 Indexing
### Local Secondary Index (LSI)
- Alternative Sort Key
- Supports
  - String
  - Number
  - Binary
- Max of 5 per table
- Defined at table creation time
### Global Secondary Index (GSI)
- Alternative Primary Key
- Index Key Supports
  - String
  - Number
  - Binary
- Can be added/modified after table creation
- **Use Case** 
  - Having a table with Sort Key where **you want to query sort key**
  - Create a **GSI** and set the **Sort Key** as **Partition Key**
### Throttling
- LSI
  - Uses WCUs and RCUs of main table
- GSI
  - If writes are throttled on GSI > main table will be throttled
  - Must provision RCUs & WCUs for the index
## 🧱 Conditional Locking
### What is it?
- A strategy to ensure an item hasn’t changed before you update/delete it
### How it works?
- Each item has an attribute that acts as a version number
- Using **Conditional Writes**
- Check if version number is still the same e.g. version=1
- Only if true perform operation
## 🧱 Accelerator (DAX)
### What is it?
- In-memory cache for DynamoDB
### Why use it?
- Doesn’t require application logic modification
- Solves the “Hot Key” problem (too many reads / throttling) 
### How it works?
- Create a DAX Cluster
- Client interacts with DAX Cluster
- DAX Cluster interacts with DynamoDB
### Limits
- 5 minutes TTL for cache (default)
- Up to 10 nodes in the cluster
### DAX vs ElastiCache (Use them together)
- Use DAX for simple queries
  - Individual objects cache
  - Query & Scan cache
- Use ElastiCache for logic application (scan > sum > etc..)
  - Store aggeration result
## 🧱 DynamoDB Streams
### What is it?
- Item level modifications
  - Create
  - Update 
  - Delete
### Use Cases
  - React to changes in real-time
    - Welcome email
  - Analytics
  - Cross-region replication
### Integrations
- Sent To
  - Kinesis Data Streams
- Read By
  - AWS Lambda
    - Define an **Event Source Mapping** to read from DynamoDB Streams
    - Provide permissions fro Lambda
    - Lambda Function invokes synchronously
  - Kinesis Client Library apps
### Information to be Written
- `KEYS_ONLY` – only the key attributes of the modified item
- `NEW_IMAGE` – the entire item, as it appears after it was modified
- `OLD_IMAGE` – the entire item, as it appeared before it was modified
- `NEW_AND_OLD_IMAGES` – both the new and the old images of the item
### Notes
- Records are not retroactively populated in a stream after enabling it, only new ones will
## 🧱 Time To Live (TTL)
### What is it?
- Similar to Redis cache TTL
### How use it?
- Enable it on Table level
- TTL attribute must be a **Number**
- Expired items deleted within 48 hours
- Expired items are also deleted from LSI & GSI
### Use Cases
- **Session Storage** is perfect example
- Reduce storage cost
- Adhere to regulatory obligations
## 🧱 DynamoDB CLI
- `--projection-expression`: one or more attributes to retrieve
- `--filter-expression`: filter items before returned to you
- General AWS CLI Pagination options (e.g., DynamoDB, S3, …)
  - `--page-size`: specify that AWS CLI retrieves the full list of items but with a larger number of API calls instead of one API call (default: 1000 items)
  - `--max-items`: max. number of items to show in the CLI (returns NextToken)
  - `--starting-token`: specify the last NextToken to retrieve the next set of items
## 🧱 DynamoDB Transactions
### What is it?
- All-Or-Nothing operations
  - Add
  - Update
  - Delete
### Why use it?
- Provides (ACID)
  - Atomicity
  - Consistency
  - Isolation
  - Durability
### Costs
- Consumes 2x WCUs & RCUs
### Operations
- TransactGetItems
  - One or more GetItem operations
- TransactWriteItems - One or more operations
  - PutItem
  - UpdateItem
  - DeleteItem
### Use Cases
- Financial transactions
- Managing orders
- Multiplayer games
## 🧱 DynamoDB vs ElastiCache for Session State Cache
- DynamoDB
  - Serverless auto scaling **YES PLEASE SERVERLESS**
- ElastiCache
  - In-memory **YES PLEASE**
- EFS
  - Must be attached to EC2 instances as a network drive **OKAY**
- EBS & Instance Store
  - Can only be used for local caching so **NO**
- S3
  - Has high latency
  - Not meant for small objects so **NO**
## 🧱 DynamoDB Write Sharding (Hot Shards)
- Add a **suffix/prefix** to **Partition Key** value
## 🧱 Write Types
### Concurrent Writes (_Optimistic Locking_)
- Use **Conditional Writes** to only update is item is untouched
### Atomic Writes
- Increase value relative to the current value
  - Current value = 1
  - User A INCREASE value by +1
  - User B INCREASE value by +1
  - New current value = 3
### Batch Writes
- When a user write/updates many items at a time
## 🧱 DynamoDB + S3
### Large Objects Pattern
- Client uploads image
- Store object in S3 and get ObjectKey
- Store this metadata in DynamoDB with image URL pointing to S3
- Client reads metadata from DynamoDB then gets data from S3
### Index S3 Objects Metadaata (Search Capabilities)
- Client uploads file to S3
- S3 Events invokes Lambda
- Lambda stores Object's metadata in DynamoDB Table
- Client later can search for items in DynamoDB and fetch the ones he wants from S3
## 🧱 DynamoDB Operations (_important_)
### Table Cleanup
- Option 1 (good)
  - `Scan` + `DeleteItem`
  - VERY Slow
  - Consumes RCU & WCU
  - EXPENSIVE
- Option 2 (bad)
  - `DropTable` + `Recreate` table
  - Fast
  - Efficient
  - Cheap
### Copy DynamoDB Table
- Option 1
  - AWS Data Pipeline copies data to S3 then writes to new table
- Option 2 (good but takes some time)
  - Backup
  - Restore
## 🧱 Backup & Restore
- Point-in-time (PITR) like RDS
- No performance impact
# 2️⃣3️⃣ AWS Serverless: API Gateway
## 🎛️ Integrations
### Lambda Function
- Easy way to expose REST API backed by AWS Lambda
### HTTP
- Expose HTTP endpoints in the backend
- Example
  - Application Load Balancer
- Why? Add rate limiting, caching, user authentications, API keys, etc…
### AWS Service
- Expose any AWS API through the API Gateway
## 🎛️ Endpoint Types
### Edge-Optimized (default): For global clients
- Requests are routed through the CloudFront Edge locations (improves latency)
- The API Gateway still lives in only one region
### Regional
- For clients within the same region
- Could manually combine with CloudFront (more control over the caching strategies and the distribution)
### Private
- Can only be accessed from your VPC using an interface VPC endpoint (ENI)
- Use a resource policy to define access
## 🎛️ Authentication
### User Authentication
- IAM Roles
- Cognito
- Custom Authorizer
### Custom Domain Name HTTPS
- Must setup CNAME or A-alias record in Route 53
## 🎛️ Deployment
### Deployment Stages
- Use the naming you like for stages (dev, test, prod)
- Each stage has its own configuration parameters
### Stage Variables
- Are like environment variables for API Gateway
- Use them to change often changing configuration values
- Can be used in
  - Lambda function ARN
  - HTTP Endpoint
  - Parameter mapping templates
- Use Cases
  - Configure HTTP endpoints your stages talk to (dev, test, prod…)
  - Pass configuration parameters to AWS Lambda through mapping templates
- Format
  - `${stageVariables.variableName}`
### Stage Variables + Lambda Aliases
- Create a stage variable to indicate the corresponding Lambda alias
- Our API gateway will automatically invoke the right Lambda function!
- No need to update API Gateway
### Canary Deployment
- Enabled on stage level
- Choose % of traffic to newly deployed stage
- Blue/Green Deployment
## 🎛️ Integration Types
### MOCK
- Returns a response without sending the request to the backend
### HTTP / AWS (Lambda & AWS Services)
- Setup data mapping using **mapping templates** for the request & response
### AWS_PROXY (Lambda Proxy)
- Incoming request from the client is forwarded as input to Lambda
- Lambda responsible for the logic of request / response
- No mapping template
- Headers, query strings, etc.. are passed as arguments
### HTTP_PROXY
- Usually to AWS Service
- HTTP Headers can be added and passed to the backend
## 🎛️ Mapping Templates
### What is it?
- Used to modify request / responses
- Used to Rename / Modify query string parameters
- Used to Modify body content
- Used to Add headers
- Remove unnecesary data
### How it works?
- Content-Type must
  - application/json
  - OR application/xml
### Use Cases
- Convert JSON to XML for our SOAP API in the backend (_important_)
- Change the values of the request to match our code
## 🎛️ Open API
### What is it?
- Common way of defining REST APIs, using API definition as code
### Why use it?
- Import/Export existing OpenAPI 3.0 spec to API Gateway
- Generate SDK
## 🎛️ Request Validation
### What is it?
- Can configure API Gateway to perform basic validation of an API request before proceeding with the integration request
### How it works?
- Checks if the required parameters in an incoming request are included and non-blank
  - URI
  - Query string
  - Headers 
- Checks if the request payload adheres to the configured JSON Schema request model of the method
### Why use it?
- Reduces unnecessary calls to the backend
## 🎛️ Caching
### Options
- Default TTL (time to live) is 300 seconds (min: 0s, max: 3600s)
- Defined per stage
- Can Override cache settings per method
### Invalidations
- Clients can invalidate the cache with **header**: `Cache-Control: max-age=0` (with proper IAM authorization)
## 🎛️ Usage Plans
- Who can access one or more deployed API stages and methods
- How much and how fast they can access them
- Uses API keys to identify API clients and meter access
- Configure throttling limits and quota limits that are enforced on individual client
## 🎛️ API Keys
- alphanumeric string values to distribute to your customers
- Can use with usage plans to control access
- **Throttling** limits are applied to the API keys
- **Quotas** limits is the overall number of maximum requests
## 🎛️ Usage Plans + API Keys
### How it works?
1. Create one or more APIs, configure the methods to require an API key, and deploy the APIs to stages.
2. Generate or import API keys to distribute to application developers (your customers) who will be using your API.
3. Create the usage plan with the desired throttle and quota limits.
4. Associate API stages and API keys with the usage plan.
- **Callers** of the API must **supply an assigned API key** in the `x-api-key` header in requests to the API
## 🎛️ Loggin & Tracing
### CLoudWatch Logs
- Information about request/response
- Enabled at stage level with log level
  - ERROR
  - DEBUG
  - INFO
- Can override per api
### X-Ray
- Tracing for extra information about performance and latency
### CloudWatch Metrics (Detailed Metrics)
- Enabled at stage level
- Provides information about
  - CacheHitCount - Cache efficiency
  - CacheMissCount - Cache efficiency
  - Count - API requests / period
  - IntegrationLatency - Time of API Gateway > backend > API Gateway
  - Latency - Time of Client > API Gateway > Integration ... > API Gateway > Client
  - 4XXError - Client Side
    - 400 - Bad Request
    - 403 - Access Denied / WAF
    - 429 - Quota Exceeded / Throttle = Too Many Requests
  - 5XXError - Server Side
    - 502 - Incompatible output from Lambda
    - 503 - Service Unavailable Exception
    - 504 - Integration Failure
      - Endpoint Request Timed-out Exception
## 🎛️ Throttling
### Account Limit
- Default 10,000/sec accross all API
## 🎛️ CORS
### How it works?
- Enabled at api level
- The OPTIONS pre-flight request must contain the following headers
  - `Access-Control-Allow-Methods`
  - `Access-Control-Allow-Headers`
  - `Access-Control-Allow-Origin`
## 🎛️ Security
### Resource Policy
- Allow for cross account access (with IAM Security)
- Allow for a specific source IP address
- Allow for a VPC Endpoint
### IAM
- Great for users / roles already within your AWS account, + resource policy for cross account
- Handle authentication + authorization
- Leverages Signature v4
### Cognito User Pools
- Authentication = Cognito User Pools
- Authorization = API Gateway Methods
- No custom implementation required
### Lambda Authorizer
- Token-based authorizer (bearer token)
- Authentication = External
- Authorization = Lambda Function
## 🎛️ WebSocket API
### Why use it?
- Enables stateful application use cases
- Real-time applications
### Works With
- Lambda
- DynamoDB
- HTTP Endoints
### Connecting to the API
- WebSocket URL
  - `wss://[some-uniqueid].execute-api.[region].amazonaws.com/[stage-name]`
### Client to Server Messaging
- WebSocket URL
  - `wss://abcdef.execute-api.us-west-1.amazonaws.com/dev`
- **ConnectionID is re-used**
### Server to Client Messaging
- WebSocket URL
  - `wss://abcdef.execute-api.us-west-1.amazonaws.com/dev`
### Connection URL Operations

| Operation | Action                                                       |
| --------- | ------------------------------------------------------------ |
| POST      | Sends a message from the Server to the connected WS Client   |
| GET       | Gets the latest connection status of the connected WS Client |
| DELETE    | Disconnect the connected Client from the WS connection       |
- Connection URL
  - `wss://abcdef.execute-api.us-west-1.amazonaws.com/dev/@connections/connectionId`
### WebSocket API – Routing
- You request a route selection expression to select the field on JSON to route from
- Sample expression: `$request.body.action`
- The result is evaluated against the route keys available in your API Gateway
- The route is then connected to the backend you’ve setup through API Gateway
- If no routes => sent to `$default`
- ROUTE KEY TABLE (API Gateway)
  - `$connect`
  - `$disconnect`
  - `$default`
  - `join`
  - `quit`
  - `delete`
# 2️⃣4️⃣ AWS CICD: CodeCommit, CodePipeline, CodeBuild, CodeDeploy
## 🎡 Overview
### AWS CodeCommit
- Storing our code
### AWS CodePipeline
- Automating our pipeline from code to Elastic Beanstalk
### AWS CodeBuild
- Building and testing our code
### AWS CodeDeploy
- Deploying the code to EC2 instances (not Elastic Beanstalk)
### AWS CodeStar
- Manage software development activities in one place
### AWS CodeArtifact
- Store, publish, and share software packages
### AWS CodeGuru
- Automated code reviews using Machine Learning
## 🎡 AWS CodeCommit
### What is it?
- A Git repository
### Why use it?
- Increased security and compliance
- Integrated with Jenkins, AWS CodeBuild, and other CI tools
### Security
- Authentication
  - SSH Keys – AWS Users can configure SSH keys in their IAM Console
  - HTTPS – with AWS CLI Credential helper or Git Credentials for IAM user
- Authorization
  - IAM policies to manage users/roles permissions to repositories
- Encryption
  - Repositories are automatically encrypted at rest using AWS KMS
  - Encrypted in transit (can only use HTTPS or SSH – both secure)
- Cross-account Access
  - Do NOT share your SSH keys or your AWS credentials
  - Use an IAM Role in your AWS account and use AWS STS (AssumeRole API)
## 🎡 AWS CodePipeline
### What is it?
- Visual Workflow to orchestrate your CICD
### How it works?
- Source – CodeCommit, ECR, S3, Bitbucket, GitHub
- Build – CodeBuild, Jenkins, CloudBees, TeamCity
- Test – CodeBuild, AWS Device Farm, 3rd party tools, …
- Deploy – CodeDeploy, Elastic Beanstalk, CloudFormation, ECS, S3, …
- Invoke – Lambda, Step Functions
- Consists of stages:
  - Each **stage** can have sequential actions and/or parallel actions
    - Each stage can have **multiple action groups**
  - Example: Build > Test > Deploy > Load Testing > …
  - Manual approval can be defined at any stage
  - Each pipeline stage can create artifacts
  - **Artifacts** stored in an S3 bucket and passed on to the next stage
### Troubleshooting
- Use CloudWatch Events (Amazon EventBridge)
  - You can create events for failed pipelines
  - You can create events for cancelled stages
- Use AWS CloudTrail can be used to audit AWS API calls
### Events vs Webhooks vs Polling
- Events
  - CodeCommit ⤵️
    1. EventBridge ⤵️ 
    2. CodePipeline
  - GitHub ⤵️
    1. CodeStar Source Connection (GitHub App) ⤵️
    2. CodePipeline
- Webhooks
  - Script ⤵️
    - CodePipeline
- Polling
  - CodePipeline checks regularly for updates (bad approach)
### Manual Approval Stage
- Onwer of "**Action**" is "**AWS**" (_important_)
- Manuall approving user must have required IAM Permissions for the actions
  - `codepipeline:GetPipeline`
  - `codepipeline:PutApprovalResult`
### CloudFormation Integration
- CodePipeline
  1. CodeBuild - Builds app
  2. `CREATE_UPDATE` - Create or update an existing CloudFormation stack
  3. CodeBuild - Test app
  4. `DELETE_ONLY` - Delete test CloudFormation stack
  5. Deploy - CloudFormation production stack
## 🎡 AWS CodeBuild
### What is it?
- A fully managed continuous integration (CI) service
### Why use it?
- Continuous scaling (no servers to manage or provision – no build queue)
- Compile source code
- Run tests
- Produce software packages
### How it works?
- Source – CodeCommit, S3, Bitbucket, GitHub
- Build instructions: Code file buildspec.yml or insert manually in Console
- Output logs can be stored in Amazon S3 & CloudWatch Logs
- **buildspec.yml**
  - **env** – define environment variables
    - **variables** – plaintext variables
    - **parameter-store** – variables stored in SSM Parameter Store
    - **secrets-manager** – variables stored in AWS Secrets Manager
- **phases** – specify commands to run:
  - **install** – install dependencies you may need for your build
  - **pre_build** – final commands to execute before build
  - **Build** – actual build commands
  - **post_build** – finishing touches (e.g., zip output)
- **artifacts** – what to upload to S3 (encrypted with KMS)
  - **cache** – files to cache (usually dependencies) to S3 for future build speedup
### CodeBuild – Inside VPC
-  By default, your CodeBuild containers are launched outside your VPC
   - It cannot access resources in a VPC
- You can specify a VPC configuration:
  - VPC ID
  - Subnet IDs
  - Security Group IDs
- Then your build can access resources in your VPC e.g.
  - RDS
  - ElastiCache
  - EC2
  - ALB
## 🎡 AWS CodeDeploy
### What is it?
- Deploy new applications versions to
  - EC2 Instances
  - On-premises servers
  - Lambda functions
  - ECS Services
### Why use it?
- Automated Rollback capability in case of failed deployments, or trigger CloudWatch Alarm
- Gradual deployment control
### How it works?
- A file named **appspec.yml** defines how the deployment happens
- **EC2/On-premises**
  - Types
    - **AllAtOnce**: most downtime
    - **HalfAtATime**: reduced capacity by 50%
    - **OneAtATime**: slowest, lowest availability impact
    - **Custom**: define your %
  - **CodeDeploy Agent**
    - Must run on the target instances
    - The EC2 Instances must have sufficient permissions to access Amazon S3 to get deployment bundles
      | Metric                 | In-Place Deployment                                        | Blue/Green Deployment                                       |
      | ---------------------- | ---------------------------------------------------------- | ----------------------------------------------------------- |
      | Deployment Approach    | Updates the existing environment                           | Creates a new environment                                   |
      | Availability           | Application downtime during deployment                     | Zero application downtime                                   |
      | Rollback Capability    | Limited rollback options                                   | Easy rollback by switching environments                     |
      | Infrastructure Cost    | No additional cost                                         | Additional cost for the new environment                     |
      | Testing                | Limited testing opportunities before deployment            | Full testing on the new environment                         |
      | Environment Separation | Single environment used for both deployment and production | Separate environments for deployment and production         |
      | Release Confidence     | Higher risk due to limited testing and rollback options    | Higher confidence due to thorough testing and easy rollback |
      | Traffic Switching      | Traffic is not switched until the deployment is complete   | Traffic can be easily switched between environments         |
      | Configuration Drift    | May encounter configuration drift over time                | Reduced configuration drift by recreating the environment   |
      | Scaling                | Limited scalability during deployment                      | Scalability can be improved in the new environment          |
      | Resource Utilization   | Higher resource utilization during deployment              | Optimized resource utilization in the new environment       |
- **Lambda Platform**
  - Types
    -  **Linear**: grow traffic every N minutes until 100%
       - LambdaLinear10PercentEvery3Minutes
       - LambdaLinear10PercentEvery10Minutes
    - **Canary**: try X percent then 100%
      - LambdaCanary10Percent5Minutes
      - LambdaCanary10Percent30Minutes
    - **AllAtOnce**: immediate
- **ECS Platform**
  - Only Blue/Green Deployments
  - Types
    -  **Linear**: grow traffic every N minutes until 100%
       - ECSLinear10PercentEvery3Minutes
       - ECSLinear10PercentEvery10Minutes
    - **Canary**: try X percent then 100%
      - ECSCanary10Percent5Minutes
      - ECSCanary10Percent30Minutes
    - **AllAtOnce**: immediate
- **Auto Scaling Group (ASG)**
  - Types
    - In-Place Deployment
    - Blue/Green Deployment
### Rollbacks
- Redeploy a previously deployed revision of your application
- Can be Automatic/Manual
-  If a roll back happens, CodeDeploy redeploys **the last known good revision as a new deployment** (not a restored version)
### Troubleshooting
- Time on EC2 instances and CodeDeploy must match or an `InvalidSignatureException – Signature expired: [time] is now earlier than [time]` error will occure
## 🎡 AWS CodeStar
### What is it?
- An integrated solution that groups
  - GitHub
  - CodeCommit
  - CodeBuild
  - CodeDeploy
  - CloudFormation
  - CodePipeline
  - CloudWatch
### Why use it?
- Quickly create “CICD-ready” projects for
  - EC2
  - Lambda
  - Elastic Beanstalk
### Note
- Limited Customization
## 🎡 AWS CodeArtifact
### What is it?
- A secure, scalable, and cost-effective artifact management for software development
- Store and retrieve dependencies
### Why use it?
- Developers and CodeBuild can then retrieve dependencies straight from CodeArtifact
- Developers can build, push, and share artifacts
### EventBridge Integration
- Event is created when a Package version is
  - Created
  - Modified
  - Deleted
### Cross Account Access
- Use Resource Policy
- A given principal can either read **all the** packages in a repository or none of them
### Upstream Repositories
- Allows a package manager client to access the packages that are contained in more than one repository using a single repository endpoint
- Up to 10 Upstream Repositories
- Only one external connection
### Storage
- Deduplicated Storage
  - Asset only needs to be stored once in a domain
  - Even if it's available in many repositories (only pay once for storage)
- Fast Copying
  - Only metadata record are updated when you pull packages from an Upstream CodeArtifact Repository into a Downstream
- Easy Sharing Across Repositories and Teams
  - All the assets and metadata in a domain are encrypted with a single AWS KMS Key
- Apply Policy Across Multiple Repositories – 
  - Domain administrator can apply policy across the domain such as:
    - Restricting which accounts have access to repositories in the domain
    - Who can configure connections to public repositories to use as sources of packages
## 🎡 Amazon CodeGuru
### CodeGuru Reviewer
- Automated code reviews for static code analysis (development)
### CodeGuru Profiler
- Visibility/recommendations about application performance during runtime (production)
# 2️⃣5️⃣ AWS Serverless: SAM (Serverless Application Model)
## 🪄 What is it?
- Framework for developing and deploying serverless applications
- All the configuration is YAML code
- Generate complex CloudFormation from simple SAM YAML file
- Built on CloudFormation
- Requires the Transform and Resources sections
## 🪄 Recipe / Template
- Converted to CloudFormation Template
### Transform Header indicates it’s SAM template
- Transform: 'AWS::Serverless-2016-10-31'
### Write Code
- AWS::Serverless::Function
- AWS::Serverless::Api
- AWS::Serverless::SimpleTable
### Package & Deploy (_important_)
- aws cloudformation package = `sam package`
- aws cloudformation deploy = `sam deploy`
## 🪄 Deployment
1. SAM Template + Application Code > Build app locally using `sam build` (_transform_) ⤵️ 
2. CloudFormation Template + Application Code > Package the app  (_zip & upload `sam package`OR `aws cloudformation package`_) ⤵️
3. S3 Bucket > Deploy the app (_create/execute ChangeSet `sam deploy` OR `aws cloudformation deploy`_)
4. CloudFormationStack (_created_)
## 🪄 CLI Debugging
### Run Locally
- Provides Lambda-like execution env
- Can
  - Build
  - Test
  - Debug
### AWS Toolkit
- Step-through debugging
## 🪄 Policy Template (_important_)
### What is it?
- List of templates to apply permissions for Lambda Functions
### Examples
- `S3ReadPolicy`: Gives read only permissions to
objects in S3
- `SQSPollerPolicy`: Allows to poll an SQS queue
- `DynamoDBCrudPolicy`: CRUD = create read update delete
```YAML
MyFunction:
  Type: 'AWS::Serverless::Function'
  Properties:
    CodeUri: ${codeuri}
    Handler: hello.handler
    Runtime: python2.7
    Policies:
      - SQSPollerPolicy: ⬅️
          QueueName:
            !GetAtt MyQueue.QueueName
```
## 🪄 SAM + CodeDeploy
- SAM natively uses CodeDeploy to update Lambda Functions
### Enables
- Traffic Shifting feature
- Pre and Post traffic hooks features to validate deploymen (before the traffic shift starts and after it ends)
- Easy & automated rollback using CloudWatch Alarms
### Configuration
- `AutoPublishAlias`
  - Detects when new code is being deployed
  - Creates and publishes an updated version of that function with the latest code
  - Points the alias to the updated version of the Lambda function
- `DeploymentPreference`
  - Canary
  - Linear
  - AllAtOnce
- `Alarms`
  - Alarms that can trigger a rollback
- `Hooks`
  - Pre and post traffic shifting Lambda functions to test your deployment
```YAML
Resources:
  MyLambdaFunction:
    Type: AWS::Serverless::Function
    Properties:
      Handler: index.handler
      Runtime: nodejs12.x
      CondeUri: s3://bucket/code.zip

      AutoPublishAlias: live ⬅️

      DeploymentPreference: ⬅️
        Type: Canary10Percent10Minutes
        Alarms: ⬅️
          # A list of alarms that you want to monitor
          - !Ref AliasErrorMetricGreaterThanZeroAlarm
          - !Red LatestVersionErrorMetricGreaterThanZeroAlarm
        Hooks: ⬅️
          # Validation Lambda functions that are run before & after traffic shifting
          PreTraffic: !Ref PreTrafficLambdaFunction
          PostTraffic: !Ref PostTrafficLambdaFunction
```
### Local Capabilities
- **Locally start AWS Lambda**
  - `sam local start-lambda`
  - Starts a local endpoint that emulates AWS Lambda
  - Can run automated tests against this local endpoint
- **Locally Invoke Lambda Function**
  - `sam local invoke`
  - Invoke Lambda function with payload once and quit after invocation completes
  - Helpful for **generating test cases**
  - If the function make API calls to AWS, make sure you are using the correct `--profile` option
- **Locally Start an API Gateway Endpoint**
  - `sam local start-api`
  - Starts a local HTTP server that hosts all your functions
  - Changes to functions are automatically reloaded
- **Generate AWS Events for Lambda Functions**
  - `sam local generate-event`
  - Generate sample payloads for event sources
  - Example Services
    - S3
    - API Gateway
    - SNS
    - Kinesis
    - DynamoDB
    - etc..
### Commands (_important_)
- `sam build`: fetch dependencies and create local deployment artifacts
- `sam package`: package and upload to Amazon S3, generate CF template
- `sam deploy`: deploy to CloudFormation
## 🪄 Serverless Application Repository (SAR)
### What is it?
- Managed repository for serverless applications
- Apps are packaged using SAM
### Notes
- Application settings and behaviour can be customized using **Environment variables**
# 2️⃣6️⃣ Cloud Development Kit (CDK)
## 🪖 What is it?
- Infrastructure as code framework for AWS resources.
- Simplifies cloud resource provisioning and management.
- Enables developers to define infrastructure using familiar programming languages.
## 🪖 Why use it?
- Deploy infrastructure and application runtime code together
- Great for Lambda functions
- Great for Docker containers in ECS / EKS
- It is typesafe! If it doesn't work it won't compile
## 🪖 How it works?
- Code is “compiled” into a CloudFormation template
  1. CDK Application Constructs ⤵️
  2. CDK CLI ⤵️
  3. CloudFormation Template ⤵️
  4. CloudFormation Stack ⤵️
## 🪖 CDK vs SAM
|     | SAM                                               | CDK                                                                                   |
| --- | ------------------------------------------------- | ------------------------------------------------------------------------------------- |
| 1.  | Serverless focused                                | All AWS services                                                                      |
| 2.  | Write your template declaratively in JSON or YAML | Write infra in a programming language (JavaScript/TypeScript, Python, Java, and .NET) |
| 3.  | Great for quickly getting started with Lambda     | It is typesafe! If it doesn't work it won't compile                                   |
| 4.  | Leverages CloudFormation                          | Leverages CloudFormation                                                              |
## 🪖 CDK + SAM
- Can use SAM CLI to locally test your CDK apps
  1. **Must first run `cdk synth`**
  2. Generates CloudFormation Template
  3. Can run `sam local invoke` on the template to call the function
## 🪖 CDK Constructs
### What is it?
- A component that encapsulates everything CDK needs to create the final CloudFormation stack
### AWS Construct Library
- A collection of Constructs included in AWS CDK which contains Constructs for every AWS resource
### Construct Hub
- contains additional Constructs from AWS, 3rd parties, and open-source CDK community
### Layer 1 (L1)
- **CFN Resources** - epresents all resources directly available in CloudFormation
- **Periodically generated from CloudFormation Resource Specification**
- Names start with `Cfn` for example 
  - `CfnBucket`
- Must explicitly configure all resource properties
  ```JS
  const bucket = new s3.CfnBucket(this, "MyBucket", {
    bucketName: "MyBucket"
  }) 
  ```
### Layer 2 (L2)
- Represents AWS resources but with a higher level (intent-based API)
- Boilerplate approach
- Provide methods that make it simpler to work with the resource
  - `bucket.addLifeCycleRule()`
  ```JS
  const s3 = require('aws-cdk-lib/aws-s3');
  const bucket = new s3.Bucket(this, 'MyBucket', {
    versioned: true,
    encryption: s3.BucketEncryption.KMS
  });
  // Returns the HTTPS URL of an S3 Object ⤵️
  const objectUrl = bucket.urlForObject('MyBucket/MyObject')'
  ```
### Layer 3 (L3)
- Can be called **Patterns**
  - Represents multiple related records
- Help complete common tasks in AWS
- Examples
  - `aws-apigateway.LambdaRestApi` represents an API Gateway backed by a Lambda function
  - `aws-ecs-patterns.ApplicationLoadBalancerFargateService` which represents an architecture that includes a Fargate cluster with Application Load Balancer
  ```JS
  const api = new apigateway.LambdaRestApi(this, 'myapi', {
    handler: backend,
    proxy: false
  })
  ```
## 🪖 Commands (_important_)
| Command                      | Description                                        |
| ---------------------------- | -------------------------------------------------- |
| `npm install -g aws-cdk-lib` | Install the CDK CLI and libraries                  |
| `cdk init app`               | Create a new CDK project from a specified template |
| `cdk synth`                  | Synthesizes and prints the CloudFormation template |
| `cdk bootstrap`              | Deploys the CDK Toolkit staging Stack              |
| `cdk deploy`                 | Deploy the Stack(s)                                |
| `cdk diff`                   | View differences of local CDK and deployed Stack   |
| `cdk destroy`                | Destroy the Stack(s)                               |
## 🪖 Bootstrapping
### What is it?
- The process of provisioning resources for CDK before you can deploy CDK apps into an AWS environment
- **AWS Environment** = **account & region**
### Note
- Must run the following command for each new environment
  - `cdk bootstrap aws://<aws_account>/<aws_region>`
  - If not ran a policy error will occur stating there's an invalid principal
  - **Has to be done once per region per account for CDK to work**
## 🪖 Testing
### How it works?
- Use **CDK Assertions Module**
- Verify existence of specific
  - Resources
  - Rules
  - Conditions
  - Parameters...
### Types 
- **Fine-grained Assertions** (common)
  - Test specific aspects of the CloudFormation template
    - Check if a resource has this property with this value
- **Snapshot Tests**
  - Test the synthesized CloudFormation template against a previously stored baseline template
### Import Templates (_important_)
- `Template.fromStack(MyStack)` -  stack built in CDK
- `Template.fromString(mystring)` - stack build outside CDK
```JS
describe("StateMachineStack", ()=>{
  test("synthesizes the way we expect", ()=>{
    //Prepare the stack for assertions
    const template = Template.fromStack(MyStack);

    // Assert it creates Lambda with correct properties
    template.hasResourceProperties("AWS::Lambda::Function", {
      Handler: "handler",
      Runtime: "nodejs14.x"
    }); // ⬅️Fin-grained Assertion
    // Assert it creates the SNS subscription
    template.resourceCountIs("AWS::SNS:Subscription",1); // ⬅️Fin-grained Assertion

    // Assert the synthesized CloudFormation template against a previously stored baseline template
    expect(template.toJSON()).toMatchSnapshot(); //⬅️Snapshot Test
  })
})
```
# 2️⃣7️⃣ Cognito: Cognito User Pools, Cognito Identity Pools, and Cognito Sync
## 🔐 Cognito User Pools vs Identity Pools
| Cognito User Pools<br>(Authentication = Identity Verification)                                        | Cognito Identity Pools<br>(Authorization = Access Control)              |
| ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Database of users for your web and mobile application                                                 | Obtain AWS credentials for your users                                   |
| Allows federating logins through Public Social, OIDC, SAML                                            | Users can login through Public Social, OIDC, SAML & Cognito User Pools  |
| Customizable hosted UI for authentication (including the logo)                                        | Users can be unauthenticated (guests)                                   |
| Triggers with AWS Lambda during the authentication flow                                               | Users are mapped to IAM roles & policies, can leverage policy variables |
| Ability to adapt the sign-in experience to different risk levels (MFA, adaptive authentication, etc.) |                                                                         |
## 🔐 Cognito vs IAM
### Cognito Use Cases Are
- Hundreds of users
- Mobile users
- Authenticate with SAML
## 🔐 Cognito User Pools Integrations
### API Gateway
- Easy to integrate, enabled on API stage level
### Application Load Balancer (ASG)
- Must use an **HTTPS** listener to set authenticate-oidc & authenticate-cognito rules
- `OnUnauthenticatedRequest`
  - Authenticate (default)
  - Deny
  - Allow
## 🔐 Lambda Trigger
### What is it?
- Allow to react to events happening in User Pool

| User Pool Flow | Operation                           | Description                                                   |
| -------------- | ----------------------------------- | ------------------------------------------------------------- |
| Authentication | Pre Authentication Lambda Trigger   | Custom validation to accept or deny the sign-in request       |
|                | Post Authentication Lambda Trigger  | Event logging for custom analytics                            |
|                | Pre Token Generation Lambda Trigger | Augment or suppress token claims                              |
| Sign-Up        | Pre Sign-up Lambda Trigger          | Custom validation to accept or deny the sign-up request       |
|                | Post Confirmation Lambda Trigger    | Custom welcome messages or event logging for custom analytics |
|                | Migrate User Lambda Trigger         | Migrate a user from an existing user directory to user pools  |
| Messages       | Custom Message Lambda Trigger       | Advanced customization and localization of messages           |
| Token Creation | Pre Token Generation Lambda Trigger | Add or remove attributes in ID tokens                         |
## 🔐 Hosted Authentication UI
### What is it?
- A ready-made UI to handle sign-up and sign-in workflows
- Can customize with a custom logo and custom CSS
### Custom Domain (_important_)
- Must create an ACM certificate in **us-east-1**
- Domain must be defined in the “App Integration” section
## 🔐 Adaptive Authentication
### What is it?
- Block sign-ins or require MFA if the login appears suspicious
- Integration with CloudWatch Logs
  - Sign-in attempts
  - Risk score
  - Failed challenges
  - Etc..
## 🔐 JWT - JSON Web Token
- CUP issues JWT tokens (Base64 encoded):
  - **Header**
  - **Payload**
  - **Signature**
- The signature must be verified to ensure the JWT can be trusted
## 🔐 Cognito Identity Pools (Federated Identities)
### What is it?
- Get identities for "users" so they obtain temporary AWS credentials
- Users can then access AWS services directly or through API Gateway
- Cognito Identity Pools allow for **unauthenticated (guest) access**
### IAM Roles
- The IAM policies applied to the credentials are defined in Cognito
- They can be customized based on the `user_id` for fine grained control
- Default IAM roles for authenticated and guest users
- Define rules to choose the role for each user based on the user’s ID
- You can partition your users’ access using **policy variables**
```JSON
{
  "version": "2012-10-17",
  "Statement": [
    {
      "Action": ["s3::ListBucket"],
      "Effect": "Allow",
      "Resource": ["arn:aws:s3:::mybucket"],
      "Condition": {"StringLike":{"s3:prefix":["${cognito-identity.amazonaws.com:sub}/*"]}} ⬅️
    }
  ]
}
```
## 🔐 Quiz Notes
- Cognito Sync is deprecated you can use AWS AppSync
# 2️⃣8️⃣ Other Serverless: Step Functions & AppSync
## 🪜 Step Functions
### What is it?
- Model your workflows as state machines (one per workflow)
  - Order fulfillment
  - Data processing
  - Web applications
  - Any workflow
- Start workflow with
  - SDK call
  - API
  - Gateway
  - Event Bridge (CloudWatch Event)
## 🪜 Step Functions - States
### What is it?
- A task is a state in a workflow that represents a single unit of work that another AWS service performs
### State Types
- **Choice** State - Test for a condition to send to a branch (or default branch)
- **Fail** or **Succeed** State - Stop execution with failure or success
- **Pass** State - Simply pass its input to its output or inject some fixed data, without performing work.
- **Wait** State - Provide a delay for a certain amount of time or until a specified time/date.
- **Map** State - Dynamically iterate steps.’
- **Parallel** State - Begin parallel branches of execution (_important_)
## 🪜 Step Functions - Error Handling
### Error Reasons
- State machine definition issues (for example, no matching rule in a Choice state)
- Task failures (for example, an exception in a Lambda function)
- Transient issues (for example, network partition events)
### Retry & Catch
- Use **Retry** to retry failed state (_Evaluated from top to bottom_)
  - `ErrorEquals`: match a specific kind of error
  - `IntervalSeconds`: initial delay before retrying
  - `BackoffRate`: multiple the delay after each retry
  - `MaxAttempts`: default to 3, set to 0 for never retried
- Use **Catch** to transition to failure path (_When max attempts are reached, the Catch kicks in_)
  - `ErrorEquals`: match a specific kind of error
  - `Next`: State to send to
  - `ResultPath` - A path that determines what input is sent to the state specified in the Next field (_important_)
    ```JSON
    {
      "HelloWorld":{
        "Type": "Task",
        "Resource": "arn:aws:lambda:...",
        "Catch":[
          {
            "ErrorEquals": ["States.ALL"], ⬅️
            "Next": "NextTaskName", ⬅️
            "ResultPath": "$.error" ⬅️
          }
        ]
      }
    }
    ```
### Error Codes
- `States.ALL` : matches any error name
- `States.Timeout`: Task ran longer than TimeoutSeconds or no heartbeat received
- `States.TaskFailed`: execution failure
- `States.Permissions`: insufficient privileges to execute code
- \+ any other error defined inside the state by the developer
## 🪜 Step Functions - Wait for Task Token
### What it does?
- Allows you to pause Step Functions during a Task until a Task Token is returned
- **Pushes** data to run an event and wait for result
### How it works?
-  Append `.waitForTaskToken` to the Resource field to tell Step Functions to wait for the Task Token to be returned
    ```JSON
    "Resource": "arn:aws:states::sqs:sendMessage.waitForTaskToken"
    ```
- Task will pause until it receives that Task Token back with a `SendTaskSuccess` or `SendTaskFailure` API cal
## 🪜 Step Functions - Activity Tasks
- Activity Worker **poll** for a Task using `GetActivityTask` API
- After Activity Worker completes its work, it sends a response of its success/failure using `SendTaskSuccess` or `SendTaskFailure`
### Keep Task Alive **DO**
- Configure how long a task can wait by setting `TimeoutSeconds`
- Periodically send a **heartbeat** from your **Activity Worker** using `SendTaskHeartBeat` within the time you set in `HeartBeatSeconds`
### Keep Task Alive **DON'T**
- Configure a long TimeoutSeconds and actively sending a heartbeat
- This will cause the Activity Task to run up to 1 year
## 🪜 Step Functions - Standard vs Express
| Feature           | Standard                                           | Express                                                      |
| ----------------- | -------------------------------------------------- | ------------------------------------------------------------ |
| Max. Duration     | Up to 1 year                                       | Up to 5 minutes                                              |
| Execution Model   | Exactly-once Execution                             | Async / Sync                                                 |
| Execution Rate    | Over 2000 / second                                 | Over 100,000 / second                                        |
| Execution History | Up to 90 days or using CloudWatch                  | CloudWatch Logs                                              |
| Pricing           | # of State Transitions                             | # of executions, duration, and memory consumption            |
| Use Cases         | Non-idempotent actions (e.g., processing Payments) | IoT data ingestion, streaming data, mobile app backends, ... |
### Express Sync vs Async

| Async                                                            | Sync                                                             |
| ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| Doesn’t wait for Workflow to complete (get results from CW Logs) | Wait for Workflow to complete                                    |
| You don’t need an immediate response (e.g., messaging services)  | You need an immediate response (e.g., orchestrate microservices) |
| Must manage idempotence                                          | Can be invoked from API Gateway or Lambda function               |
## 🪜 AppSync
### What is it?
- AWS AppSync is a fully managed service that makes it easy to develop GraphQL APIs by handling the heavy lifting of securely connecting to data sources like DynamoDB, Lambda, and more
- Retrieve data in **real-time** with **WebSocket** or MQTT on WebSocket
- It all starts with uploading one GraphQL schema
### Security
- **API_KEY**
- **AWS_IAM**: IAM users / roles / cross-account access
- **OPENID_CONNECT**: OpenID Connect provider / JSON Web Token
- **AMAZON_COGNITO_USER_POOLS**
- For **custom domain** & **HTTPS**, use **CloudFront** in front of AppSync
## 🪜 Amplify
### What is it? 
- Set of tools to get started with creating mobile and web applications
### features
- Authentication
  - **Amazon Cognito**
  - Pre-built UI components
  - Support MFA, Social Sign-in
- Datastore
  - Amazon **DynamoDB**
  - Powered by **GraphQL**
  - **Automatic synchronization** to the cloud without complex code
  - Visual data modeling
### Hosting
- Front End
  - Build
  - Deploy to CloudFront
- Back End
  - Build
  - Deploy to Amplify
### End-to-End (E2E) Testing
- Use the test step to run any test commands at build time (**amplify.yml**)
- Integrated with Cypress testing framework
# 2️⃣9️⃣ Advancaed Identity
## 🔏 Security Token Service (STS)
AWS STS (Security Token Service) allows you to get cross-account access through the creation of an IAM Role in your AWS account authorized to assume an IAM Role in another AWS account
- `AssumeRole`
- `GetSessionToken`
- `GetCallerIdentity`
- `DecodeAuthorizationMessage`
### Assume Role
- Define an IAM Role within your account or cross-account
- Define which principals can access this IAM Role
- Use AWS STS (Security Token Service) to retrieve credentials and impersonate the IAM Role you have access to (AssumeRole API)
- Temporary credentials can be valid between 15 minutes to 1 hour
### STS With MFA (_important_)
- Use `GetSessionToken` from STS
- Appropriate IAM policy using IAM Conditions
- `aws:MultiFactorAuthPresent:true`
- Reminder: `GetSessionToken` returns
  - Access ID
  - Secret Key
  - Session Token
  - Expiration date
## 🔏 Advanced IAM
### Best Practices
- Never use Root Credentials, enable MFA for Root Account
- Grant Least Privilege
- Each Group / User / Role should only have the minimum level of permission it needs
- Never grant a policy with “*” access to a service
- Monitor API calls made by a user in CloudTrail (especially Denied ones)
- Never ever ever store IAM key credentials on any machine but a personal computer or on-premise server
- On premise server best practice is to call STS to obtain temporary security credentials
- EC2 machines should have their own roles
- Lambda functions should have their own roles
- ECS Tasks should have their own roles (ECS_ENABLE_TASK_IAM_ROLE=true)
- CodeBuild should have its own service role
- Create a least-privileged role for any service that requires it
- Create a role per application / lambda function (do not reuse roles)
### Cross Account Access
- Define an IAM Role for another account to access
- Define which accounts can access this IAM Role
- Use AWS STS (Security Token Service) to retrieve credentials and impersonate the IAM Role you have access to (AssumeRole API)
- Temporary credentials can be valid between 15 minutes to 1 hour
### Authorization Model Evaluation of Policies
1. If there’s an explicit **DENY** > end decision and **DENY**
2. If there’s an **ALLOW** > end decision with **ALLOW**
3. Else **DENY**
### IAM Policies & S3 Bucket Policies
- When evaluating if an IAM Principal can perform an operation X on a bucket, **the union** of its assigned IAM Policies and S3 Bucket Policies **will be evaluated**.
### Dynamic Policies
- Leverage the special policy variable ${aws:username}
### Inline vs Manage Policies
- AWS Managed Policy
  - Maintained by AWS
  - Good for power users and administrators
  - Updated in case of new services / new APIs
- Customer Managed Policy
  - Best Practice, re-usable, can be applied to many principals
  - Version Controlled + rollback, central change management
- Inline
  - Strict one-to-one relationship between policy and principal
  - Policy is deleted if you delete the IAM principal
### Grant User Permission to Pass a Role to AWS Service
- The use who wants to assign a Role that allows a service to perform an action needs the IAM permission `iam:PassRole`
- It often comes with `iam:GetRole` to **view the role being passed**
  ```JSON
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action":[
          "ec2:*"
        ],
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": "iam:PassRole", ⬅️
        "Resource": "arn:aws:iam:42315231:role/S3Access",
      }
    ]
  }
  ```
- Can a role be passed to any service?
  - **No**: Roles can only be passed to what their **trust** allows
## 🔏 AWS Active Directory Services
### AWS Managed Microsoft AD
- Create your own AD in AWS, manage users locally, **supports MFA**
- Establish “trust” connections with your on-premise AD
### AD Connector
- Directory Gateway (**proxy**) to redirect to on-premise AD, supports MFA
- Users are managed on the on-premise AD
### Simple AD
- AD-compatible managed directory on AWS
- **Cannot** be joined with on-premise AD
# 3️⃣0️⃣ AWS Security & Encryption: KMS, Encryption SDK, SSM Parameter Store, IAM & STS
- KMS keys can be symmetric or asymmetric. Symmetric KMS key represents a 256-bit key used for encryption and decryption. An asymmetric KMS key represents an RSA key pair used for encryption and decryption or signing and verification, but not both. Or it represents an elliptic curve (ECC) key pair used for signing and verification.
- SM Parameters Store can be used to store secrets and has built-in version tracking capability. Each time you edit the value of a parameter, SSM Parameter Store creates a new version of the parameter and retains the previous versions. You can view the details, including the values, of all versions in a parameter's history.
- Use Envelope Encryption if you want to encrypt > 4 KB of data. `GenerateDataKey`
- Because the bucket encrypted using SSE:KMS, you must give permissions to the EC2 instance to access the KMS Keys and to make decrypt operations.
- Encrypt CloudWatch Log Group with KMS key using `associate-kms-key`
- Which IAM policy condition allows you to enforce SSL requests to objects stored in your S3 bucket? `aws:SecureTransport`
# 3️⃣1️⃣ AWS Other Services
- AWS SES
  - Send Emails to people using
    - SMTP
    - AWS SDK
  - Receive emails
    - S3
    - SNS
    - Lambda
  - Integrate with IAM for allowing to send emails
- Amazon Open Search Service
  - Search any field on DynamoDB
    1. CRUD on DynamoDB Table
    2. DynamoDB Stream
    3. Lambda Function
    4. Amazon OpenSearch
    5. API to Search items
    6. API to retrieve item from DynamoDB by id 
  - CloudWatch Logs
    1. CloudWatch Lops
    2. Subscription Filter
    3. Lambda Function (real-time) / Kinesis Data Firehose (near real-time)
    4. Amazon OpenSearch
  - Kinesis Data Streams
    1. Kinesis Data Streams
    2. Lambda Function (real-time) / Kinesis Data Firehose (near real-time)
    3. Lambda Function (data transformation)
    4. Amazon OpenSearch 
  - Modes
    - Managed Cluster
    - Serverless Cluster
  - No native support to SQL
    - Can be enabled via a plugin
- Amazon Athena (Business Intelligence / Analytics)
  - Serverless query service to analyze data stored in Amazon S3
  - Use SQL language
  - Supports
    - CSV
    - JSON
    - ORC
    - Avro
    - Parquet
  - Use with Quicksight for reporting/dashboards
  - Improve Performance
    - Use columnar data for cost-saving
      - Apache Parquet / ORC
      - Use Glue to convert data to Parquet / ORC
    - Compress data
      - bzip2
      - gzip
      - lz4
    - Partition
      - The more partitions the better the performance
      - The more fubfolders the easier to retrieve data
    - Use bigger files > 128MB
    - Federated Query
      - Use Lambda as Data Source Connector
      - Run queries across data stored in relational/non-relational object
      - Store results in Amazon S3
- Amazon Managed Streaming for Apache Kafka (MSK)
  - Alternative to Amazon Kinesis
  - Managed Apache Kafka on AWS
  - Data is stored on EBS volumes as long as you want

    | Kinesis Data Streams      | Amazon MSK                             |
    | ------------------------- | -------------------------------------- |
    | 1MB message limit size    | 1MB default (can configure for higher) |
    | Data Streams with Shards  | Kafka Topics with Partitions           |
    | Shard Splitting & Merging | Can only add partitions to a topic     |
    | TLS In-flight encryption  | PLAINTEXT or TLS In-flight encryption  |
    | KMS at-rest encryption    | KSM at-rest encryption                 |
- Amazon Certificate Manager (ACM)
  - Easily provision and manage SSL/TLS Certificates
  - Integrates with
    - Elastic Load Balancer
    - CloudFront Distribution
    - APIs on API Gateway
- AWS AppConfig
  - Configure, validate, and deploy dynamic configurations
- Deploy dynamic configuration changes to your  applications independently of any code deployments
  - You don’t need to restart the application
- Feature flags, application tuning, allow/block listing…
- Use with apps on EC2 instances, Lambda, ECS, EKS…
- Gradually deploy the configuration changes and rollback if issues occur
- Validate configuration changes before deployment using:
  - JSON Schema (syntactic check) or
  - Lambda Function
- AWS Fault Injection Simulator (FIS)
  - To test your AWS resources against failures, you should use the AWS Fault Injection Simulator (FIS) service. AWS Fault Injection Simulator allows you to proactively test the resilience of your applications and infrastructure by injecting various types of failures and disruptions into your AWS environment. It helps you identify weaknesses, validate your recovery strategies, and improve the overall reliability of your AWS resources.