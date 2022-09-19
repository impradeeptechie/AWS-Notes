# AWS-Notes

# AWS Certfied Cloud Practitioner

#### Cloud Computing

-   On-demand delivery of IT resources. Can scale up and down based on needs.
-   Fosters agility (number one reason why customers switch to cloud computing): Speed (global reach), experimentation (operations as code, templated environments with CloudFormation) and culture of innovation (experiment quickly with low cost)
-   Region vs Availability Zone (AZ): Region is a physical location in the world which contains multiple AZs. AZs contain one or more discrete data centers with independent resources and housed in different facilities.
-   Using Auto Scaling and ELB, scale up and down and only pay for what you use.
-   Ability to deploy systems in multiple regions (lower latency)
-   Ability to choose the region where data is stored
-   AWS is responsible for data center security
-   Security policy can be formalized (as code)
-   Ability to recover from failures

#### Core Services

-   Global Infrastructure:
    -   Regions: Have multiple AZs
    -   Availability Zones: Have one or more data centres. They all have different power supplier companies.
    -   Edge Locations: Used by CloudFront.
-   Amazon Virtual Private Cloud (VPC)
    -   Uses same concepts as on-premise networking
    -   VPC can span across multiple AZs
    -   Supports multiple subnets (each of which can be deployed in a different AZ)
    -   Can create public-facing subnets and private-facing subnets within the same VPC
    -   Each account can create multiple VPCs
    -   Using fewer VPCs is recommended to avoid complexity
    -   Can assign Internet Gateways to specific subnets to allow public access
    
-   Security Groups
    -   Act like a built-in firewall
    -   Best practice: Allow what's required only and block everything else
-   Compute Services
    -   Amazon Lightsail: Managed Virtual Private Servers service
        -   Fixed price.
        -   Includes a static IP, DNS management and storage
        -   Fixed configuration
        -   Uses t2 class EC2 instances under the hood
    -   AWS Elastic Compute Cloud (EC2)
        -   Difference betwwen EC2-Classic and EC2-VPC
            -   EC2-Classic: Your instances run in a single, flat network that you share with other customers.
            -   EC2-VPC: Your instances run in a virtual private cloud (VPC) that's logically isolated to your AWS account.
    -   AWS Lambda
        -   No servers to manage
        -   Pay as you go: Only pay for the time your code runs
        -   Continuous scaling
        -   Supports subsecond metering. Charged for every 100 milliseconds of execution time
        -   Some limitations apply: [AWS Lambda Limits](https://docs.aws.amazon.com/lambda/latest/dg/limits.html)
    -   AWS Elastic Beanstalk
        -   Platform as a service
        -   Allows quick deployments of applications
        -   Allows HTTPS on load balancers
        -   Supports various platforms (node.js, python etc)
        -   Provisions the resources required (EC2, ELB etc) automatically
    -   Application Load Balancer
        -   Comes with new features
![alt text](ELBvsALB.png "Logo Title Text 1")

        -   Supports routing to containers
        -   Key terms:
            -   Listeners: A process that checks for connection requests using the configuration (protocol, port)
            -   Target: Destination for traffic
            -   Target Group: Each target group routes requests to one or more registered targets
        -   Target checks can be performed per target group basis
        -   Integrates with ECS and supports dynamic ports utilized by scheduled containers
        -   Need to create at least 2 AZs when creating an Application Load Balancer
        -   Ability to route to different target groups based on port or path
    -   Elastic Load Balancer
        -   Supports sticky sessions
        -   Supports multiple AZs and cross-zone balancing
        -   For HTTP/HTTPS it uses "Least Outstanding" method to route the request. For TCP, it uses "Round robin". The least outstanding routing algorithm is defined as "A 'least outstanding requests routing algorithm' is an algorithm that choses which instance receives the next request by selecting the instance that, at that moment, has the lowest number of outstanding (pending, unfinished) requests."
    -   Auto Scaling
        -   Adding more instances: Scaling out, terminating instanes: Scaling in
        -   Launch configuration answers "What" (AMI, Instance type, Security Groups, Roles). Creating an LC is similar to creating a new EC2 instance.
        -   Auto Scaling Group answers "Where" (VPC and subnet(s), load balancer, minimum and maximum instances, desired capacity)
        -   Auto Scaling Policy answeres "When" (Scheduled/on-demand/scale out or in policy)
-   Amazon EBS
    -   Allows point-in-time snapshots and creation of a new volume from a snapshot
    -   Supports encrypted volumes free of charge
    -   EBS volume must be created in the same AZ as the EC2 instance that will use it
-   Amazon S3
    -   Objects are stored redundantly across multiple facilities withing the same region
    -   The bucket names must be globally unique.
    -   Can configure cross-region replication for backup and disaster recovery
    -   Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and an S3 bucket
-   Amazon Glacier
    -   Vaults have access and lock policies attached to them
    -   Each AWS account can create up to 1000 vaults
    -   Can create an S3 lifecycle policy to move to Glacier then delete after a period of time
        -   Supports up to 40TB max item size (S3 supports 5TB)
        -   It costs more per retrieval
        -   Vault Lock allows you to easily deploy and enforce compliance controls for individual Amazon Glacier vaults with a vault lock policy. You can specify controls such as "write once read many" (WORM) in a vault lock policy and lock the policy from future edits. Once locked, the policy can no longer be changed
-   Amazon RDS
    -   Can create a standby copy in a different AZ within the same VPC
    -   Can create multiple read replicas (in different regions as well)
-   Amazon DynamoDB
    -   Always uses SSD for storage
    -   Supports auto-scaling. Increases/decreases the throughput based on load
    -   Tables are partitioned by primary key
    -   Two query methods: Query and Scan
    -   Query uses the primary key to find items. Scan can use any attribute.
    -   Scan is slower than Query as it needs to look at all items
-   Amazon Redshift
    -   Managed data warehouse
    -   Supports standard SQL
    -   Supports ODBC/JDBC connectors
-   Amazon Aurora
    -   Managed MySQL-clone (compatible with MySQL)
    -   After a crash it doesn't need to redo log files. It performs it on every read operation which reduces the restart time
-   AWS Trusted Advisor
    -   Checks all the resources used and gives advice based on best practices
    -   5 categories:
        -   Cost optimisation
        -   Performance
        -   Security
        -   Fault tolerance
        -   Service limits
    -   Upgrading support plan enables all Trusted Advisor recommendations, free plan doesn't include all
    -   Has an API and can be used to automate optimisations
    -   Can use it with CloudWatch alarms

#### Security

-   The AWS Shared Responsibility Model
    -   AWS handles infrastructure security
    -   AWS provides 3rd party audit reports
    -   AWS's responsibilities include: OS and database patching, firewall configuration and disaster recovery
    -   Customer is responsible for putting logical access controls in place and protect account credentials
    -   Customers are responsible to secure everything they put in the cloud
-   AWS Service Catalog
    -   Allows to centrally manage common IT services that are approved for use on AWS
-   AWS IAM
    -   Controls access to AWS resources
    -   Handles Authentication (who can access resources) and authorization (how they can use resources)
    -   Users can have programmatic access and/or console access.
    -   Best practices
        -   Delete root account keys. Instead use IAM accounts
        -   Use MFA
        -   Use groups
        -   Use roles
        -   Rotate credentials
        -   Remove unnecessary users
-   AWS Security Compliance Programs
    -   Risk Management: Follow the following standards:
        -   COBIT
        -   AICPA
        -   NIST
    -   Constantly scans service endpoints for vulnerabilities
    -   Compliance programs are listed [here](https://aws.amazon.com/compliance/programs/)
-   AWS Security Resources
    -   AWS Trusted Advisor: Helps to follow best practices
    -   AWS Account Teams: First point of contact
    -   AWS Enterprise Support: 15-minute response time, 24x7 availability
    -   AWS Partner Network
    -   AWS Advisories and Bulletins
    -   AWS Auditor Learning Path
    -   AWS Compliance Solutions Guide: <https://aws.amazon.com/compliance/solutions-guide/>
    -   AWS Security Blog: <https://aws.amazon.com/blogs/security/>

#### Architecting

-   Well-architected framework: <https://aws.amazon.com/architecture/well-architected/>
-   Fiver pillars of the framework
    -   Operational excellence
    -   Security
    -   Reliability
    -   Performance efficency
    -   Cost optimization
-   Fault Tolerance
    -   Remain operational even if components fail
    -   Built-in redundancy of an application's components
-   High-Availability
    -   A concept for the whole system
    -   "Always" functioning and accessible
    -   Without human intervention
    -   HA Service Tools
        -   Elastic Load Balancer
        -   Elastic IP Addresses
        -   Amazon Route 53
        -   Auto Scaling
        -   Amazon CloudWatch

#### Pricing and Support

-   Core concepts in billing
    -   Pay as you go: No up front expenses
    -   Pay less when you reserve: Reserved instances cost less
    -   Pay even less per unit by using more: Tiered pricing for services such as S3, EC2 etc. Data transfer in is always free of charge.
    -   Pay even less as AWS grows
-   Amazon RDS Costs
    -   Clock hours of server time
    -   Database characteristics
    -   Database purchase type
    -   Number of DB instances
    -   Provisional storage
        -   No charge for backup storage of up to 100% of database storage for active databases. After terminated, the backups are charged
    -   Additional storage
    -   Requests
    -   Deployment type
    -   Data transfer



# AWS Certfied Developer Associate 
## IAM: Identity and Access Management

When accessing AWS, the root account should **never** be used. Users must be created with the proper permissions. IAM is central to AWS.
- Users: A physical person
- Groups: Functions (admin, devops) Teams (engineering, design) which contain a group of users
- Roles: Internal usage within AWS resources
- Policies (JSON documents): Defines what each of the above can and cannot do. **Note**: IAM has predefined managed policies.


#### For big enterprises:
- IAM Federation: Integrate their own repository of users with IAM using SAML standard

### Policies
IAM policies define permissions for an action regardless of the method that you use to perform the operation.

#### Policy types
- Identity-based policies
  - Attach managed and inline policies to IAM identities (users, groups to which users belong, or roles). Identity-based policies grant permissions to an identity.

- Resource-based policies
  - Attach inline policies to resources. The most common examples of resource-based policies are Amazon S3 bucket policies and IAM role trust policies. Resource-based policies grant permissions to a principal entity that is specified in the policy. Principals can be in the same account as the resource or in other accounts.

- Permissions boundaries
  - Use a managed policy as the permissions boundary for an IAM entity (user or role). That policy defines the maximum permissions that the identity-based policies can grant to an entity, but does not grant permissions. Permissions boundaries do not define the maximum permissions that a resource-based policy can grant to an entity.

- Organizations SCPs
  - Use an AWS Organizations service control policy (SCP) to define the maximum permissions for account members of an organization or organizational unit (OU). SCPs limit permissions that identity-based policies or resource-based policies grant to entities (users or roles) within the account, but do not grant permissions.

- Access control lists (ACLs)
  - Use ACLs to control which principals in other accounts can access the resource to which the ACL is attached. ACLs are similar to resource-based policies, although they are the only policy type that does not use the JSON policy document structure. ACLs are cross-account permissions policies that grant permissions to the specified principal entity. ACLs cannot grant permissions to entities within the same account.

- Session policies
  - Pass advanced session policies when you use the AWS CLI or AWS API to assume a role or a federated user. Session policies limit the permissions that the role or user's identity-based policies grant to the session. Session policies limit permissions for a created session, but do not grant permissions. For more information, see Session Policies.

#### AWS Policy Simulator
- When creating new custom policies you can test it here:
  - https://policysim.aws.amazon.com/home/index.jsp
  - This policy tool can you save you time in case your custom policy statement's permission is denied
- Alternatively, you can use the CLI:
    - Some AWS CLI commands (not all) contain `--dry-run` option to simulate API calls. This can be used to test permissions.
    - If the command is successful, you'll get the message: `Request would have succeeded, but DryRun flag is set`
    - Otherwise, you'll be getting the message: `An error occurred (UnauthorizedOperation) when calling the {policy_name} operation`
  
#### Best practices:
- One IAM User per person **ONLY**
- One IAM Role per Application
- IAM credentials should **NEVER** be shared
- Never write IAM credentials in your code. **EVER**
- Never use the ROOT account except for initial setup
- It's best to give users the minimal amount of permissions to perform their job

------------------------------------------

# EC2: Virtual Machines

- [EC2 User Data](#ec2-user-data)
- [EC2 Meta Data](#ec2-meta-data)
- [EC2 Instance Launch Types](#ec2-instance-launch-types)
- [EC2 Pricing](#ec2-pricing)
- [AMIs](#AMIs)
- [EC2 Instances Overview](#ec2-instances-overview)

By default, your EC2 machine comes with:
* A private IP for the internal AWS Network
* A public IP for the WWW

When you SSH into your EC2 machine:
* We can’t use a private IP, because we are not in the same network
* We can only use the public IP

If your machine is stopped and then restarted, the public IP will change

## EC2 User Data
* It is possible to bootstrap our instances using an EC2 User data script
* Bootstrapping means launching commands when a machine starts
* That script is only run once at the instance first start
* Purpose: Ec2 data is used to automated boot tasks such as:
    * Installing updates
    * Installing software
    * Downloading common files from the internet
* The EC2 User Data Script runs with the root user
  
## EC2 Meta Data
* Information about your EC2 instance
* It allows EC2 isntances to "learn" about themselves without having to use an IAM role for that purpose
* Powerful but one of the least known features to developers
* You can retrieve IAM roles from the metadata but **not** IAM policies
* URL: 169.254.169.254/latest/meta-data

## EC2 Instance Launch Types 
- **On Demand Instances**: short workload, predictable pricing
- **Reserved Instances**: long workloads (>= 1 year)
- **Convertible Reserved Instances**: long workloads with flexible instances
- **Scheduled Reserved Instances**: launch within time window you reserve
- **Spot Instances**: short workloads, for cheap, can lose instances
- **Dedicated Instances**: no other customers will share your hardware
- **Dedicated Hosts**: book an entire physical server, control instance placement

#### On Demand Instance:
* Pay for what you use
* Has the highest cost but no upfront payment
* No long term commitment
* Recommended for short-term and un-interrupted workloads, where you can’t predict how the application will behave
#### Reserved Instances
* Up to 75% compared to On-demand
* Pay upfront for what you use with long term commitment
* Reservation period can be 1 or 3 years
* Reserve a specific instance type
* Recommended for steady state usage applications (think database)
#### Convertible Reserved Instances
* Can change the EC2 instance type
* Up to 54% discount
#### Scheduled Reserved Instances
* Launch within time window you reserve
* When you require a fraction of a day / week / month
#### Spot Instances
* Can get a discount of up to 90% compared to On-demand
* You bid a price and get the instance as long as its under the price
* Price varies based on offer and demand
* Spot instances are reclaimed within a 2 minute notification warning when the spot price goes above your bid
* Used for batch jobs, Big Data analysis, or workloads that are resilient to failures
* Not great for critical jobs or databases
#### Dedicated Instances
* Instances running on hardware that’s dedicated to you
* May share hardware with other instances in same account
* No control over instance placement (can move hardware after stop / start)
#### Dedicated Hosts
* Physical dedicated Ec2 server for your use
* Full control of Ec2 Instance placement
* Visibility into the underlying sockets / physical cores of the hardware
* Allocated for your account for a 3 year period reservation
* More expensive
* Useful for software that have a complicated licensing model (Bring your own License)
* Or for a companies that have strong regulatory or compliance needs

#### Which host is right for me?
- On demand: coming and staying in resort whenever we like, we pay the full price 
- Reserved: like planning ahead and if we plan to stay for a long time, we may get a good discount. 
- Spot instances: the hotel allows people to bid for the empty rooms and the highest bidder keeps the rooms.You can get kicked out at any time 
- Dedicated Hosts: We book an entire building of the resort

## EC2 Pricing
- EC2 instances prices (per hour) varies based on these parameters:
  - Region you’re in
  - Instance Type you’re using
  - On-Demand vs Spot vs Reserved vs Dedicated Host
  - Linux vs Windows vs Private OS (RHEL, SLES, Windows SQL)
  - You are billed by the second, with a minimum of 60 seconds. 
  - You also pay for other factors such as storage, data transfer, fixed IP public addresses, load balancing
  - You do not pay for the instance if the instance is stopped 

- Example
  - t2.small in US-EAST-1 (VIRGINIA), cost $0.023 per Hour 
  - If used for:
    - 6 seconds, it costs $0.023/60 = $0.000383 (minimum of 60 seconds)
    - 60 seconds, it costs $0.023/60 = $0.000383 (minimum of 60 seconds)
    - 30 minutes, it costs $0.023/2 = $0.0115
    - 1 month, it costs $0.023 * 24 * 30 = $16.56 (assuming a month is 30 days)
    - X seconds (X > 60), it costs $0.023 * X / 3600 
  - The best way to know the pricing is to consult the pricing page: https://aws.amazon.com/ec2/pricing/on-demand/

## AMIs
### What's AMI?
- As we saw, AWS comes with base images such as:
  - Ubuntu
  - Fedora
  - RedHat
  - Windows
  - Etc...
- These images can be customized at runtime using EC2 User data
- But what if we could create our own image, ready to go?
- That’s an AMI – an image to use to create our instances
- AMIs can be built for Linux or Windows machines 

### Why you use a custom AMI?
- Using a custom built AMI can provide the following advantages:
  - Pre-installed packages needed
  - Faster boot time (no need for long ec2 user data at boot time
  - Machine comes configured with monitoring / enterprise software
  - Security concerns – control over the machines in the network
  - Control of maintenance and updates of AMIs over time
  - Active Directory Integration out of the box
  - Installing your app ahead of time (for faster deploys when auto-scaling)
  - Using someone else’s AMI that is optimized for running an app, DB, etc...
- **AMI are built for a specific AWS region (!)**

## EC2 Instances Overview
- Instances have 5 distinct characteristics advertised on the website:
  - The RAM(type,amount,generation)
  - The CPU(type,make,frequency,generation,numberofcores)
  - The I/O (disk performance, EBS optimisations)
  - The Network (network bandwidth, network latency
  - The Graphical Processing Unit (GPU) 
- It may be daunting to choose the right instance type (there are over 50 of them) - https://aws.amazon.com/ec2/instance-types/ 
- https://ec2instances.info/ can help with summarizing the types of instances 
- R/C/P/G/H/X/I/F/Z/CR are specialised in RAM, CPU, I/O, Network, GPU 
- M instance types are balanced 
- T2/T3 instance types are “burstable”
Burstable Instances (T2)
- AWS has the concept of burstable instances (T2 machines) 
- Burst means that overall, the instance has OK CPU performance. 
- When the machine needs to process something unexpected (a spike in load for example), it can burst, and CPU can be VERY good. 
- If the machine bursts, it utilizes “burst credits” 
- If all the credits are gone, the CPU becomes BAD 
- If the machine stops bursting, credits are accumulated over time
- Burstable instances can be amazing to handle unexpected traffic and getting the insurance that it will be handled correctly 
- If your instance consistently runs low on credit, you need to move to a different kind of non-burstable instance (all the ones described before). 

### T2 Unlimited 
- Nov 2017: It is possible to have an “unlimited burst credit balance
- You pay extra money if you go over your credit balance, but you don’t lose in performance
- Overall, it is a new offering, so be careful, costs could go high if you’re not monitoring the health of your instances 

------------------------------------------

# Security Groups

#### The fundamental of network security in AWS

* Can be attached to multiple instances
* Locked down to a region / VPC combination
* Does live “outside” the EC2 - if traffic is blocked, the EC2 instance won’t see it
* It’s good to maintain one separate security group for SSH access
* If your application is not accessible (time out), then it’s usually a security group issue
* If your application gives a “connection refused” error, then it’s an application error or its not launched
* All inbound traffic is blocked by default
* All outbound traffic authorized by default

#### Security groups act as a firewall on EC2 Instances
They regulate:
* Access to ports
* Authorized IP ranges - IPv4 and IPv6
* Control of inbound network
* Control of outbound network

------------------------------------------

# ELB: Elastic Load Balancers

Load balancers are servers that forward internet traffic to multiple servers (EC2 Instances) downstream

#### Why use a load balancer?
* Spread load across multiple downstream instances
* Expose a single point of access (DNS) to your application
* Seamlessly handle failures of downstream instances
* Do regular health checks to your instances
* Provide SSL termination (HTTPS) for your websites
* Enforce stickiness with cookies
* High availability across zones
* Separate public traffic from private traffic

#### AN ELB (EC2 Load Balancer) is a managed load balancer
* AWS guarantees that it will be working
* AWS takes care of upgrades, maintenance, high availability
* AWS provides only a few configuration knobs

It costs less to setup your own load balancer but it will be a lot more effort on your end. It is integrated with many AWS offerings / services

#### Types of load balancers on AWS
* Classic Load Balancer (v1 - older generation - 2009)
* Application Load Balancer (v2 - new generation - 2016)
* Network Load Balancer (v2 - new generation - 2017)
* You can setup internal or external ELBs

#### Health Checks
* Health checks are crucial for load balancers
* They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
* The health check is done on a port and a route (/health is common)
* If the response is not 200 (OK), then the instance is unhealthy

#### Application Load Balancer (v2)
* Application load balancers (Layer 7) allow to do:
  * Load balancing to multiple HTTP applications across machines (target groups)
  * Load balancing to multiple applications on the same machine (ex: containers)
  * Load balancing based on route in URL
  * Load balancing based on hostname in URL 
* Basically, they’re awesome for micro services & container-based application (example: Docker & Amazon ECS) 
* Has a port mapping feature to redirect to a dynamic port 
* In comparison, we would need to create one Classic Load Balancer per application before.That was very expensive and inefficient!
* Good to Know
    * Stickiness can be enabled at the target group level
        * Same request goes to the same instance
        * Stickiness is directly generated by the ALB (NOT the application)
    * ALB supports HTTP/HTTPS & Web sockets protocols
    * The application servers don’t see the IP of the client directly
        * The true IP of the client is inserted in the header X-Forwarded-For
        * We can also get Port (X-Forwarded-Port) and protocol (X-Forwarded-Proto)
#### Network Load Balancer (v2)
* Layer 4 allow you to do:
    * Forward TCP traffic to your instances
    * Handle millions of requests per second
    * Support for static IP or elastic IP
    * Less latency ~100ms (vs 400 ms for ALB)
* Network Load Balancers are mostly used for extreme performance and should not be the default load balancer you choose
* Overall, the creation process is the same as the Application Load Balancer

#### Load Balancers Good to Know
* Any Load Balancer (CLB, ALB, NLB) has a static host name. They do not resolve and use underlying IP
* LBs can scale but not instantaneously - contact AWS for a “warm up”
* NLB directly see the client IP
* 4xx errors are client induced errors
* 5xx errors are application induced errors
    * Load balancer Errors 503 means at capacity or no registered target
* If the LB can’t connect to your application, check your security

------------------------------------------

# ASG: Auto Scaling Group

In real-life, the load on your websites and applications can change. You can create and get rid of servers very quickly

The goal of an Auto Scaling Group (ASG) is to:
* Scale out (add EC2 Instances) to match an increased load
* Scale in (remove EC2 Instances) to match a decreased load
* Ensure we have a minimum and a maximum number of machines running
* Automatically register new instances to a load balancer

#### ASGs have the following attributes
* A launch configuration
    * AMI + Instance Type
    * EC2 User Data
    * EBS Volumes
    * Security Groups
    * SSH Key Pair
* Min Size / Max Size / Initial Capacity
* Network + Subnets Information
* Load Balancer Information
* Scaling Policies

#### Auto Scaling Alarms
* It is possible to scale an ASG based on CloudWatch alarms
* An alarm monitors a metric (such as Average CPU)
* Metrics are computed for the overall ASG instances
* Based on the alarm:
    * We can create a scale-out policies (increase the number of instances)
    * We can create a scale-in policies (decrease the number of instances)

#### New Auto Scaling Rules
* It is now possible to define “better” auto scaling rules that are directly managed by EC2
    * Target Average CPU Usage
    * Number of requests on the ELB per instance
    * Average Network In
    * Average Network Out
* These rules are easier to set up and can make more sense

#### Auto Scaling Custom Metric
* We can auto scale based on a custom metric (ex: number of connected users)
* 1. Send custom metrics from an application on EC2 to CloudWatch (PutMetric API)
* 2. Create a CloudWatch alarm to react to low / high values
* 3. Use the CloudWatch Alarm as the scaling policy for ASG

#### ASG Summary
* Scaling policies can be on CPU, Network… and can even be on custom metrics or based on a schedule (if you know your visitors patterns)
* ASGs use Launch configurations and you update an ASG by providing a new launch configuration
* IAM roles attached to an ASG will get assigned to EC2 instances
* ASG are free. You pay for the underlying resources being launched
* Having instances under an ASG means that if they get terminated for whatever reason, the ASG will restart them. Extra safety
* ASG can terminate instances marked as unhealthy by an LB (and hence replace them)

------------------------------------------

# EBS Volume

* An EC2 machine loses its root volume (main drive) when it is manually terminated.
* Unexpected terminations might happen from time to time (AWS would email you)
* Sometimes, you need a way to store your instance data somewhere
* An EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run
* It allows your instances to persist data

#### EBS Volume
* It’s a network drive (Not a physical drive)
    * It uses the network to communicate the instance, which means there might be a bit of latency
    * It can be detached from an EC2 instance and attached to another one quickly
* It’s locked to an Availability Zone (AZ)
    * An EBS Volume in us-east-1a cannot be attached to us-east-1b
    * To move a volume across, you first need to snapshot it
* Have a provisioned capacity (size in GBs and IOPs)
    * You get billed for all the provisioned capacity
    * You can increase the capacity of the drive over time

#### EBS Volume Types
- EBS Volumes come in 4 types 
- GP2 (SSD): General purpose SSD volume that balances price and performance for a wide variety of workloads 
- IO1 (SSD): Highest-performance SSD volume for mission-critical low-latency or high- throughput workloads 
- ST1 (HDD): Low cost HDD volume designed for frequently accessed, throughput- intensive workloads 
- SC1 (HDD): Lowest cost HDD volume designed for less frequently accessed workloads 
- EBS Volumes are characterized in Size | Throughput | IOPS
- When in doubt always consult the AWS documentation

#### EBS Volume Resizing
* Feb 2017: You can resize your EBS Volumes
* After resizing an EBS volume, you need to repartition your drive

EBS Snapshots
* EBS Volumes can be backed up using “snapshots”
* Snapshots only take the actual space of the blocks on the volume
* If you snapshot a 100GB drive that only has 5 gb of data, then your EBS snapshot will only be 5 gb
* Snapshots are used for:
    * Backups: ensuring you can save your data in case of catastrophe
    * Volume migration
        * Resizing a volume down
        * Changing the volume type
        * Encrypt a volume

#### EBS Encryption
* When you create an encrypted EBS volume, you get the following:
    * Data at rest is encrypted inside the volume
    * All the data in flight moving between the instance and the volume is encrypted
    * All snapshots are encrypted
    * All volumes created from the snapshots are encrypted
* Encryption and decryption are handled transparently (you have nothing to do)
* Encryption has a minimal impact on latency
* EBS Encryption leverages keys from KMS (AES-256)
* Copying an unencrypted snapshot allows encryption

#### EBS vs. Instance Store
* Some instance do not come with Root EBS volumes
* Instead, they come with “instance Store”
* Instance store is physically attached to the machine
* Pros:
    * Better I/O performance
* Cons:
    * On termination, the instance store is lost
    * You can’t resize the instance store
    * Backups must be operated by the user
* Overall, EBS-backed instances should fit most applications workloads


#### EBS Summary
* EBS can be attached to only one instance at a time
* EBS are locked at the AZ level
* Migrating an EBS volume across AZ means first backing it up (snapshot), then recreating it in the other AZ
* EBS backups use IO and you shouldn’t run them while your application is handling a lot of traffic
* Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated. (You can disable that)
* In some cases, it's better to externalize your RDS database so that it won't get deleted when you delete your elastic beanstalk enviornment
* Elastic Beanstalk relies on CloudFormation

------------------------------------------

# Route 53

Route 53 is a managed DNS (Domain Name System)

DNS is a collection of rules and records which helps clients understand how to reach a server through URLs.

In AWS, the most common records are (will be on exam):
* A: URL to IPv4
* AAAA: URL to IPv6
* CNAME: URL to URL
* ALIAS: URL to AWS resource

Route 53 can use:
* Public domain names you own
* Private domain names that can be resolved by your instances in your VPCs

Route53 has advanced features such as:
* Load balancing (through DNS - also called client load balancing)
* Health checks (although limited…)
* Routing policy: simple, failover, geolocation, geoproximity, latency, weighted

Prefer Alias over CNAME for AWS resources (for performance reasons)

------------------------------------------

# RDS: Relational Database Service

A managed DB service for DB use SQL a query

It allows you to create databases in the cloud that are
* Postgres
* Oracle
* MySQL
* MariaDB
* Microsoft SQL Server
* Aurora (AWS proprietary database)

Advantages of RDS over deploying a database in EC2
* Managed service
* OS patching level
* Continuous backups and restore to specific timestamps (Point in Time Restore)
* Monitoring dashboards
* Read replicas for improved read performance
* Multi AZ setup for DR (Disaster Recovery)
* Maintenance windows for upgrades
* Scaling capability (vertical and horizontal)
* But you can’t SSH into your instances (amazon manages them for you)

RDS Read replicas for read scalability
* Up to 5 read replicas
* Within AZ, Cross AZ or Cross region
* Replication is Async, so reads are eventually consistent
* Replicas can be promoted to their own DB
* Applications must update the connection string to leverage read replicas

RDS Multi AZ (Disaster Recovery)
* SYNC replication
* One DNS name - automatic app failover to standby
* Increase availability
* Failover in case of loss of AZ, loss of network, instance or storage failure
* No manual intervention in apps
* Not used for scaling (only disaster recovery)

RDS Backups
* Backups are automatically enabled in RDS
* Automated backups:
    * Daily full snapshot of the database
    * Capture transaction logs in real time
    * Ability to restore to any point in time
    * 7 days retention (can be increased to 35 days)
* DB Snapshots:
    * Manually triggered by the user
    * Retention of backup for as long as you want

RDS Encryption
* Encryption at rest capability with AWS KMS - AES-256 encryption
* SSL certificates to encrypt data to RDS in flight
* To enforce SSL:
    * PostgreSQL: rds.force_ssl=1 in the AWS RDS Console (parameter groups)
* TO connect using SSL:
    * Provide the SSL Trust certificate (can be downloaded from AWS)
    * Provide SSL options when connection to the database

RDS Security
* RDS databases are usually deployed within a private subnet, not in a public one
* RDS Security works by leveraging security groups (the same concept as for EC2 instances) - it controls who can communicate with RDS
* IAM policies help control who can manage RDS
* Traditional username and password can be used to login to the database
* IAM users can now be used too (for MySQL / Aurora - New)

RDS vs. Aurora
* Aurora is a proprietary technology from AWS (not open sourced)
* Postgres and MySQL are both supported as Aurora DB (that means you r drivers will work as if Aurora was a Postgres or MySQL database)
* Aurora is “AWS cloud optimized” and claims 5x performance improvements over MySQL on RDS, over 3x the performance of Postgres on RDS
* Aurora storage automatically grows in increments of 10GB, up to 64 TB
* Aurora can have 15 replicas while MySQL has 5, and the replication process is faster (sub 10 ms replica lag)
* Failover in Aurora is instantaneous. It’s HA native.
* Aurora costs more than RDS (20% more) - but is more efficient

------------------------------------------

# ElastiCache

Overview:
The same way RDS is to get managed Relational Databases, ElastiCache is to get managed Redis or Memcached. Caches are in-memory databases with really high performance, low latency. They help reduce loads off of databases for read intensive workloads. They help make your application stateless. 
* Write scaling using shading. 
* Read scaling using Read Replicas
* Multi AZ with Failover Capability
* AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups

#### Solution Architecture - DB Cache
* Applications queries ElastiCache, if not available, get from RDS and store in ElastiCache
* Helps relieve load in RDS
* Cache must have an invalidation strategy to make sure only the most current data is used in there
User Session Store
* User logs into any of the applications
* The application writes the session data into ElastiCache
* The user hits another instance of our application
* The instance retrieves the data and the user is already logged in

#### Redis Overview
* Redis is an in-memory key-value store
* Super low latency (sub ms)
* Cache survive reboots by default (it’s called persistence)
* Great to host
    * User sessions
    * Leaderboards (for gaming)
    * Distributed states
    * Relieve pressure on databases (such as RDS)
    * Pub / Sub capability for messaging 
* Multi AZ with Automatic failover for Disaster Recovery if you don’t want to lose your cache data
* Support for Read Replicas

#### Memcached Overview
* Memcached is an in-memory object store
* Cache doesn’t survive reboots
* Use cases:
* Quick retrieval of objects from memory
* Cache often accessed objects
* Overall, Redis has largely grown in popularity and has better feature sets than memcached
* Most likely, you’d probably only want to use Redis for caching needs

------------------------------------------

# VPC: Virtual Private Cloud

Within a region, you’re able to create VPCs. Each VPC contain subnets (networks). Each subnet must be mapped to an AZ. It’s common to have a public ip and private ip subnet. It’s common to have many subnets per AZ.

#### Public Subnets usually contain:
* Load Balancers
* Static Websites
* Files
* Public Authentication Layers

Private Subnets usually contain:
* Web application servers
* Databases

Public and Private subnets can communicate if they’re in the same VPC

#### AWS VPC Summary
* VPC & Regions aren’t much asked at the developer associate exam
* All new accounts come with a default VPC
* It’s possible to use a VPN to connect to a VPC
* VPC flow logs allow you to monitor the traffic within, in and out of your VPC (useful for security, performance, audit)
* VPC are per Account per Region
* Subnets are per VPC per AZ
* Some AWS resources can be deployed in VPC while others can’t
* You can peer VPC (within or across accounts) to make it look like they’re part of the same network

------------------------------------------

# S3 Buckets

* Amazon S3 allows people to store objects (files) fun “buckets” (directories)
* Buckets must have a globally unique name
    * Naming convention:
        * No uppercase
        * No underscore
        * 3-63 characters long
        * Not an IP
        * Must start with lowercase letter or number
* Objects
    * Objects (files) have a Key. The key is the FULL path:
        * <my_bucket>/my_file.txt
        * <my_bucket>/my_folder/another_folder/my_file.txt
    * There’s no concept of “directories” within buckets (although the UI will trick you to think otherwise)
    * Just keys with very long names that contain slashes (“/“)
    * Object Values are the content of the body:
        * Max Size is 5TB
        * If uploading more than 5GB, must use “multi-part upload”
    * Metadata (list of text key / value pairs - system or user metadata)
    * Tags (Unicode key / value pair - up to 10) - useful for security / lifecycle
    * Version ID (if versioning
    * 

#### AWS S3 - Versioning
* It is enabled at the bucket level
* Same key overwrite will increment the “version”: 1, 2, 3
* It is best practice to version your buckets
    * Protect against unintended deletes (ability to restore a version)
    * Easy roll back to previous versions
* Any file that is not version prior to enabling versioning will have the version “null”

#### S3 Encryption for Objects
* There are 4 methods of encrypt objects in S3
    * SSE-S3: encrypts S3 objects
        * Encryption using keys handled & managed by AWS S3
        * Object is encrypted server side
        * AES-256 encryption type
        * Must set header: “x-amz-server-side-encryption”:”AES256”
    * SSE-KMS: encryption using keys handled & managed by KMS
        * KMS Advantages: user control + audit trail
        * Object is encrypted server side
        * Maintain control of the rotation policy for the encryption keys
        * Must set header: “x-amz-server-side-encryption”:”aws:kms”
    * SSE-C: server-side encryption using data keys fully managed by the customer outside of AWS
        * Amazon S3 does not store the encryption key you provide
        * HTTPS must be used
        * Encryption key must provided in HTTP headers, for every HTTP request made
    * Client Side Encryption
        * Client library such as the amazon S3 Encryption Client
        * Clients must encrypt data themselves before sending to S3
        * Clients must decrypt data themselves when retrieving from S3
        * Customer fully manages the keys and encryption cycle

#### Encryption in transit (SSL)
* AWS S3 exposes:
    * HTTP endpoint: non encrypted
    * HTTPS endpoint: encryption in flight
* You’re free to use the endpoint your ant, but HTTPS is recommended
* HTTPS is mandatory for SSE-C
* Encryption in flight is also called SSL / TLS

#### S3 Security
* User based
    * IAM policies - which API calls should be allowed for a specific user from IAM console
* Resource based
    * Bucket policies - bucket wide rules from the S3 console - allows cross account
    * Object Access Control List (ACL) - finer grain
    * Bucket Access Control List (ACL) - less common
* Networking
    * Support VPC endpoints (for instances in VPC without www internet)
* Logging and Audit:
    * S3 access logs can be stored in other S3 buckets
    * API calls can be logged in AWS CloudTrail
* User Security:
* MFA (multi factor authentication) can be required in versioned buckets to delete objects
* Signed URLs: URLS that are valid only for a limited time (ex: premium video services for logged in users)

#### S3 Bucket Policies
* JSON based policies
    * Resources: buckets and objects
    * Actions: Set of API to Allow or Deny
    * Effect: Allow / Deny
    * Principal: The account or user to apply the policy to
* Use S3 bucket for policy to:
    * Grant public access to the bucket
    * Force objects to be encrypted at upload
    * Grant access to another account (Cross Account)

#### S3 Websites
* S3 can host static website sand have them accessible on the world wide web
* The website URL will be:
    * <bucket-name>.s3-website.<AWS-region>.amzonaws.com
    * OR
    * <bucket-name>.s3-website.<AWS-region>.amazonaws.com
* If you get a 403 (forbidden) error, make sure the bucket policy allows public reads!
* 

#### S3 Cors
* If you request data from another S3 bucket, you need to enable CORS
* Cross Origin Resource Sharing allows you to limit the number of websites that can request your files in S3 (and limit your costs)
* This is a popular exam question

#### AWS S3 - Consistency Model
* Read after write consistency for PUTS of new objects
    * As soon as an object is written, we can retrieve it ex: (PUT 200 -> GET 200)
    * This is true, except if we did a GET before to see if the object existed ex: (GET 404 -> PUT 200 -> GET 404) - eventually consistent
* Eventual Consistency for DELETES and PUTS of existing objects
    * If we read an object after updating, we might get the older version ex: (PUT 200 -> PUT 200 -> GET 200 (might be older version))
    * If we delete an object, we might still be able to retrieve it for a short time ex: (DELETE 200 -> GET 200)

#### AWS S3 - Other
* S3 can send notifications on changes to
    * AWS SQS: queue service
    * AWS SNS: notification service
    * AWS Lambda: serverless service
* S3 has a cross region replication feature (managed)

#### AWS S3 Performance
* Faster upload of large objects (>5GB), use multipart upload
    * Parallelizes PUTs for greater throughput
    * Maximize your network bandwidth
    * Decrease time to retry in case a part fails
* Use CloudFront to ache S3 objects around the world (improves reads)
* S3 Transfer Acceleration (uses edge locations) - just need to change the endpoint you write to, not the code
* If using SSE-KMS encryption, you may be limited to your AWS limits for KMS usage (~100s - 1000s downloads / uploads per second)

------------------------------------------

# CLI: Command Line Interface

Add user credentials locally using this command:
- $ `aws configure`

If you are using multiple AWS accounts, you can add custom profiles with seperate credentials using this command:
- $ `aws configure --profile {my-other-aws-account}`
- if you you'd like to execute commands on a specific profile:
  - example: `aws s3 ls --profile {my-other-aws-account}`
- if you don't specify the aws profile, the commands will be executed to your **default** profile

#### AWS CLI on EC2
* IAM roles can be attached to EC2 instances
* IAM roles can come with a policy authorizing exactly what the EC2 instance should be able to do. This is the best practice.
* EC2 Instances can then use these profiles automatically without any additional configurations

#### CLI STS Decode Errors
- When you run API calls and they fail, you can get a long, encoded error message code 
- This error can be decoded using STS 
- run the command: `aws sts decode-authorization-message --encoded-message {encoded_message_code}`
- your IAM user must have the correct permissions to use this command by adding the `STS` service to your policy

------------------------------------------

# SDK: Software Development Kit

If you want to perform actions on AWS directly from your application's code without using a CLI, you can use an SDK

Official SDKs:
- Java
- .NET
- Node.js
- PHP
- Python
- Ruby
- C++

#### SDK Takeaways
- AWS SDK are required when coding against AWS Services such as DynamoDB
- Fact: AWS CLI uses the Python SDK (boto3)
- The exam expects you to know when you should use an SDK
- If you don’t specify or configure a default region, then us-east-1 will be chosen by default

#### SDK Credentials Security

- It’s recommend to use the default credential provider chain
- The default credential provider chain works seamlessly with:
  - AWS credentials at ~/.aws/credentials (only on our computers or on premise)
  - Instance Profile Credentials using IAM Roles (for EC2 machines, etc...)
- Environment variables (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY)
- Overall, NEVER EVER STORE AWS CREDENTIALS IN YOUR CODE.
- Use IAM Roles if working from within AWS Services to inherit credentials

#### Exponential Backoff

- Any API that fails because of too many calls needs to be retried with Exponential Backoff
- These apply to rate limited API
- Retry mechanism is included in SDK API calls

------------------------------------------

# Elastic Beanstalk

#### *Elastic Beanstalk* is a developer centric view of deploying application on AWS.
- A managed service
  - Instance configuration
  - OS is handled by Beanstalk
  - Deployment strategy is configurableut performed by Beanstalk
  - Application code configurable
- It will leverage all the AWS components that we have gone over thus far:
  - EC2
  - ASG
  - ELB
  - RDS
  - Etc..
- Elastic Beanstalk is free but you pay for the underlying instances
- Three architecture models:
  - Single instance deployment: good for developers
  - LB + ASG: great for production or staging web applications
  - ASG only: great for non-web apps in production
- Elastic Beanstalk has three components:
  - Application
  - Application Version (Each deployment gets assigned a version)
  - Environment name (dev, staging, prod): free naming
- You deploy application versions to environments and can promote application versions to the next environment
- Rollback feature to previous application versions
- Full control over the lifecycle of environments
- Support for many platforms:
  - Go
  - Java
  - Python
  - Node.js
  - Ruby
  - Single Container Docker
  - Multi Container Docker
  - Preconfigure Docker
  - Write your own custom platforms (If the any of the above is not supported)
  
  ------------------------------------------
  
  # CICD: Continuous Integration and Deployment

- Why use CICD?
    - Ideally, you'd want to set up a CICD to help you automate multiple steps to automate builds, push code to a repository and then deploy to your updated code to AWS.
    - This is a faster, efficient way that also helps minimize potential mistakes as opposed to running multiple manual steps
    - Automate deployement to different stages (dev, staging, and production)
    - May add manual approvals when needed
- To be a proper AWS developer, you'd need to learn CICD

- CICD Services in AWS
    - AWS CodeCommit: storing our code (similar to Github)
    - AWS CodePipeline: automatig our pipeline from code to ElasticBeanstalk
    - AWS CodeBuild: To build and test code
    - AWS CodeDeploy: deploying code to EC2 fleets (not Beanstalk)

- Continuous Integration
    - Developers push code to a repository (Github, Bitbucket, CodeCommit, etc)
    - A testing or build server checks the code as soon as it's pushed to the repository (CodeBuild, Jenkins CI, etc)
    - The developer gets feedback about the tests and checks that have passed or failed
    - Benefits of CI:
        - Find bugs and fix it early on
        - Deliver fast as soon as the code is tested
        - Deploy frequently
        - Unblocked developers are happy

- Continuous Delivery
    - Ensure that the software can be released reliably whenever needed
    - Ensures deployment happen often and quick
    - Ability to shift away from "one release every 3 months" to "5 releases a day"
    - Automated deployments

Orchestration == CICD

  ------------------------------------------

# CloudFormation

- Currently, we have been doing a lot of manual work
- All this manual work will be very tough to reproduce:
    - In another region
    - In another AWS account
    - Within the same region if everything was deleted
- Wouldn’t it be great, if all our infrastructure was... code?
- That code would be deployed and create / update / delete our
infrastructure

#### What is CloudFormation?
- CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported).
- For example, within a CloudFormation template, you want to:
    - I want a security group
    - I want two EC2 machines using this security group
    - I want two Elastic IPs for these EC2 machines
    - I want an S3 bucket
    - I want a load balancer (ELB) in front of these machines
    - Then CloudFormation creates those for you, in the right order, with the exact configuration that you specify

**Note**: This is an introduction to CloudFormation
- It can take over 3 hours to properly learn and master CloudFormation
- This section is meant so you get a good idea of how it works
- We’ll be slightly less hands-on than in other sections
- We’ll learn everything we need to answer questions for the exam
- The exam does not require you to actually write CloudFormation
- The exam expects you to understand how to read CloudFormation

#### Benefits of CloudFormation
- Infrastructure as code
    - No resources are manually created, which is excellent for control
    - The code can be version controlled for example using git
    - Changes to the infrastructure are reviewed through code
- Cost
    - Each resources within the stack is stagged with an identifier so you can easily see how much a stack costs you
    - You can estimate the costs of your resources using the CloudFormation template
    - Savings strategy: In Dev, you could automation deletion of templates at 5 PM and recreated at 8 AM, safely
- Productivity
    - Ability to destroy and re-create an infrastructure on the cloud on the fly
    - Automated generation of Diagram for your templates!
    - Declarative programming (no need to figure out ordering and orchestration)
- Separation of concern: create many stacks for many apps, and many layers. Ex:
    - PC stacks
    - Network stacks
    - App stacks
- Don’t re-invent the wheel
    - Leverage existing templates on the web!
    - Leverage the documentation

#### How CloudFormation works
- Templates have to be uploaded in S3 and then referenced in CloudFormation
- To update a template, we can’t edit previous ones. We have to re- upload a new version of the template to AWS
- Stacks are identified by a name
- Deleting a stack deletes every single artifact that was created by
CloudFormation.

#### Deploying CloudFormation templates
- Manual way:
    - Editing templates in the CloudFormation Designer
    - Using the console to input parameters, etc
- Automated way:
    - Editing templates in a YAML file
    - Using the AWS CLI (Command Line Interface) to deploy the templates
    - Recommended way when you fully want to automate your flow

#### CloudFormation Building Blocks
- Templates components (one course section for each):
    1. Resources: your AWS resources declared in the template (MANDATORY)
    2. Parameters: the dynamic inputs for your template
    3. Mappings: the static variables for your template
    4. Outputs: References to what has been created
    5. Conditionals: List of conditions to perform resource creation
    6. Metadata
- Templates helpers:
    1. References
    2. Functions
 
 #### CloudFormation Resources
 - Resources are the core of your CloudFormation template (MANDATORY)
- They represent the different AWS Components that will be created and configured
- Resources are declared and can reference each other
- AWS figures out creation, updates and deletes of resources for us
- There are over 224 types of resources (!)
- Resource types identifiers are of the form:
    - `AWS::aws-product-name::data-type-name`
- Resource documentation:
    - I can’t teach you all of the 224 resources, but I can teach you how to learn how to use them.
    - All the resources can be found here: http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html
    - Example here (for an EC2 instance): http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html
##### Analysis of CloudFormation Templates
- Going back to the example of the introductory section, let’s learn why it was written this way.
- Relevant documentation can be found here:
    - http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html
    - http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html
    - http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-eip.html
    
##### FAQ for resources
- Can I create a dynamic amount of resources?
    - No, you can’t. Everything in the CloudFormation template has to be declared.You can’t perform code generation there
- Is every AWS Service supported?
    - Almost. Only a select few niches are not there yet
    - You can work around that using AWS Lambda Custom Resources

#### CloudFormation Parameters
- Parameters are a way to provide inputs to your AWS CloudFormation template
- They’re important to know about if:
    - You want to reuse your templates across the company
    - Some inputs can not be determined ahead of time
- Parameters are extremely powerful, controlled, and can prevent errors from happening in your templates thanks to types.
- AWS offers us pseudo parameters in any CloudFormation template.
- These can be used at any time and are enabled by default

##### How to reference a parameter
- The `Fn::Ref` function can be leveraged to reference parameters
- Parameters can be used anywhere in a template.
- The shorthand for this in YAML is !Ref
- The function can also reference other elements within the template

#### CloudFormation Mappings
- Mappings are fixed variables within your CloudFormation Template.
- They’re very handy to differentiate between different environments (dev vs prod), regions (AWS regions), AMI types, etc
- All the values are hardcoded within the template
- We use `Fn::FindInMap` to return a named value from a specific key

#### When would you use Mapping vs. Parameters?
- Mappings are great when you know in advance all the values that can be taken and that they can be deduced from variables such as
    - Region
    - Availability Zone
    - AWS Account
    - Environment (dev vs prod)
    - Etc...
- They allow safer control over the template.
- Use parameters when the values are really user specific

#### CF Outputs
- The Outputs section declares optional outputs values that we can import into other stacks (if you export them first)!
- You can also view the outputs in the AWS Console or in using the AWS CLI
- They’re very useful for example if you define a network CloudFormation, and output the variables such as VPC ID and your Subnet IDs
- It’s the best way to perform some collaboration cross stack, as you let expert handle their own part of the stack
- You can’t delete a CloudFormation Stack if its outputs are being referenced by another CloudFormation stack

##### Outputs examples
- Creating a SSH Security Group as part of one template
- Create an output that references that security group

##### Cross Stack Reference
- We then create a second template that leverages that security group
    - Use the `Fn::ImportValue function`
- You can’t delete the underlying stack until all the references are deleted too.

#### CloudFormation Conditions
- Conditions are used to control the creation of resources or outputs based on a condition.
- Conditions can be whatever you want them to be, but common ones are:
    - Environment (dev / test / prod)
    - AWS Region
    - Any parameter value
- Each condition can reference another condition, parameter value or mapping

##### Defining Conditions
- The logical ID is for you to choose. It’s how you name condition
- The intrinsic function (logical) can be any of the following:
    - `Fn::And`
    `Fn::Equals`
    - `Fn::If`
    - `Fn::Not`
    - `Fn::Or`
- Conditions can be applied to resources / outputs / etc

#### CloudFormation Intrinsic Functions
- Refs
    - The `Fn::Ref` function can be leveraged to reference
        - Parameters => returns the value of the parameter
        - Resources => returns the physical ID of the underlying resource (ex: EC2 ID)
    - The shorthand for this in YAML is `!Ref`
- `Fn::GetAtt`
    - Attributes are attached to any resources you create
    - To know the attributes of your resources, the best place to look at is the documentation.
    - For example: the AZ of an EC2 machine
- `Fn::FindInMap`
    - We use `Fn::FindInMap `to return a named value from a specific key
    - `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]`
- `Fn::ImportValue`
    - Import values that are exported in other templates
        - Use the `Fn::ImportValue` function
- `Fn::Join`
    - Join values with a delimiter
- `Fn::Sub`
    - `Fn::Sub`, or `!Sub` as a shorthand, is used to substitute variables from a text. It’s a very handy function that will allow you to fully customize your templates.
    - For example, you can combine `Fn::Sub` with References or AWS Pseudo variables
    - String must contain ${VariableName} and will substitute them
- Condition Functions (`Fn::If`, `Fn::Not`, `Fn::Equals`, etc...)
    - The logical ID is for you to choose. It’s how you name condition
    - The intrinsic function (logical) can be any of the following:
        - `Fn::And`
        - `Fn::Equals`
        - `Fn::If`
        - `Fn::Not`
        - `Fn::Or`

#### CloudFormation Rollbacks
- Stack Creation Fails
    - Default: everything rolls back (gets deleted).We can look at the log
    - Option to disable rollback and troubleshoot what happened
- Stack Update Fails:
    - The stack automatically rolls back to the previous known working state
    - Ability to see in the log what happened and error messages

--------------------------------------

# CloudWatch

CloudWatch is used for monitoring.

### Why is monitoring important?
- To deploy applications
    - Safely
    - Automatically
    - Using Infrastructure as Code
    - Leveraging AWS components
- Because applications are deployed, and users don’t care what services we've used
- Users only care that the application is working!
    - Application latency: will it increase over time?
    - Application outages: customer experience should not be degraded
    - Users contacting the IT department or complaining is not a good outcome
    - Troubleshooting and remediation
- Internal monitoring:
    - Can we prevent issues before they happen?
    - Performance and Cost
    - Trends (scaling patterns)
    - Learning and Improvement

### Monitoring in AWS
- AWS CloudWatch:
    - Metrics: Collect and track key metrics
    - Logs: Collect, monitor, analyze and store log files
    - Events: Send notifications when certain events happen in your AWS • Alarms: React in real-time to metrics / events
- AWS X-Ray:
    - Troubleshooting application performance and errors
    - Distributed tracing of microservices
- AWS CloudTrail:
    - Internal monitoring of API calls being made
    - Audit changes to AWS Resources by your users

### CloudWatch Metrics
- CloudWatch provides metrics for every services in AWS
- **Metric** is a variable to monitor (CPUUtilization, NetworkIn...)
- Metrics belong to **namespaces**
- **Dimension** is an attribute of a metric (instance id, environment, etc...).
- Up to 10 dimensions per metric
- Metrics have **timestamps**
- Can create CloudWatch dashboards of metrics

### CloudWatch EC2 Detailed monitoring
- EC2 instance metrics have metrics “every 5 minutes”
- With detailed monitoring (for a cost), you get data “every 1 minute”
- Use detailed monitoring if you want to more prompt scale your ASG!
- The AWS Free Tier allows us to have 10 detailed monitoring metrics
- **Note:** EC2 Memory usage is by default not pushed (must be pushed
from inside the instance as a custom metric)

### AWS CloudWatch Custom Metrics
- Possibility to define and send your own custom metrics to CloudWatch
- Ability to use dimensions (attributes) to segment metrics
    - Instance.id
    - Environment.name
- Metric resolution:
    - Standard: 1 minute
    - High Resolution: up to 1 second (StorageResolution API parameter - Higher cost)
- Use API call **PutMetricData**
- Use exponential back off in case of throttle errors

### Alarms are used to trigger notifications for any metric
- Alarms can go to Auto Scaling, EC2 Actions, SNS notifications
- Various options (sampling, %, max, min, etc...)
- Alarm States:
    - OK
    - INSUFFICIENT_DATA
    - ALARM
- Period:
    - Length of time in seconds to evaluate the metric
    - High resolution custom metrics: can only choose 10 sec or 30 sec

### AWS CloudWatch Logs
- Applications can send logs to CloudWatch using the SDK
- CloudWatch can collect log from:
    - Elastic Beanstalk: collection of logs from application
    - ECS: collection from containers
    - AWS Lambda: collection from function logs
    - VPC Flow Logs:VPC specific logs
    - API Gateway
    - CloudTrail based on filter
    - CloudWatch log agents: for example on EC2 machines
    - Route53: Log DNS queries
- CloudWatch Logs can go to:
    - Batch exporter to S3 for archival
    - Stream to ElasticSearch cluster for further analytics
- CloudWatch Logs can use filter expressions
- Logs storage architecture:
    - Log groups: arbitrary name, usually representing an application. Log expiration policy should be defineda at this level.
    - Log stream: instances within application / log files / containers
- Can define log expiration policies (never expire, 30 days, etc..)
- Using the AWS CLI we can tail CloudWatch logs
- To send logs to CloudWatch, make sure IAM permissions are correct!
- Security: encryption of logs using KMS at the Group Level

### AWS CloudWatch Events
- Schedule: Cron jobs
- Event Pattern: Event rules to react to a service doing something
    - Ex: CodePipeline state changes
- Triggers to Lambda functions, SQS/SNS/Kinesis Messages
- CloudWatch Event creates a small JSON document to give information
about the change

---------------------------------------

# DynamoDB (No-SQL):

-   Non Relational DB (No-SQL), comprised of collections (tables), of documents (rows), with each document consisting of key/value pairs (fields)
-   Document oriented DB
-   Offers push button scaling, meaning that you can scale your db on the fly without any downtime
-   RDS is not so easy, you usually have to use a bigger instance size or add read replicas
-   Stored on SSD Storage
-   Spread across 3 geographically distinct data centers
-   Eventual Consistent Reads (Default)
    -   Consistency across all copies of data is usually reached within 1 second
    -   Repeating a read after a short time should return updated data
    -   Best Read Performance
-   Strongly Consistent Reads
    -   Returns a result that reflects all writes that received a successful response prior to the read
-   Structure:
    -   Tables
    -   Items (Think rows in a traditional table)
    -   Attributes (Think columns of data in a table)
-   Provisioned throughput capacity
-   Write throughput 0.0065 per hour for every 10 units
-   Read throughput 0.0065 per hour for every 50 units
-   First 25 GB of storage is free
-   Storage costs of 25 cents per additional GB per Month
-   Can be expensive for writes, but really really cheap for reads
-   The combined key/value size must not exceed 400 KB for any given document

**Developer Associate Specific Topics**

-   Supports attribute nesting up to 35 levels
-   Conditional writes are idempotent, you can send the same conditional write request multiple times, but it will have no further effect on the item after the first time Dynamo performs the update
-   Supports atomic counters, using the UpdateItem operation to increment or decrement the value of an existing attribute without interfering with other write requests
-   Atomic counter updates are not idempotent, the counter will increment each time you call UpdateItem
-   If you can have a small margin of error in your data, then use atomic counters
-   If your application needs to read multiple items, you can use the BatchGetItem API endpoint; A single request can retrieve up to 1MB of data with as many as 100 items
-   A single BatchGetItem request can retrieve items from multiple tables
-   All write requests are applied in the order in which they are received
-   Pricing (calculate the amount of writes and reads per second):
    -   Divide total number of writes per day / 25 (hours) / 60 (minutes) / 60 (seconds) = No. writes per second
    -   A write or read capacity unit can handle 1 write/read per second
    -   Individual items or the entire table can be exported to CSV
    -   Example:
        -   Using 28 GB of storage
        -   1,000,000 writes per day = 1,000,000/24 = 41,666.67
        -   41,666.67 / 60 (minutes) = 694.44
        -   694.44 / 60 (seconds) = 11.574 writes per second
        -   This example would require 12 write capacity units (single capacity unit is 1 write per second)
        -   Charge for write is $0.0065 per 10 units
        -   $0.0065 / 10 = $0.00065 per unit
        -   $0.00065 * 12 (required write units) = $0.0078
        -   $0.0078 * 24 (hours per day) = $0.1872 per day for writes
        -   Charge for read is $0.0065 per 50 units
        -   $0.0065 / 50 = $0.00013 per unit
        -   $0.00013 * 12 (required read units) = $0.00156
        -   $0.00156 * 24 (hours per day) = $0.03744 per day for reads
        -   Using 28 GB storage with first 25 GB free = 3 GB storage required
        -   3 GB * $0.25 per GB (after initial 25) = $0.75
-   Indexes:
    -   Primary Key types:
        -   Single attribute (unique ID):
            -   Partition Key (Hash Key composed of one attribute)
            -   Partition Key's value is used as input to an internal hash function which output determines the partition (physical location in which the data is stored)
            -   No 2 items in a table can have the same partition key value
        -   Composite (unique ID and date range):
            -   Partition Key & Sort Key (Hash and Range) composed of two attributes
            -   Partition Key's value is used as input to an internal hash function which output determines the partition (physical location in which the data is stored)
            -   2 Items can have the same partition key, but they MUST have a different sort key
            -   All Items with the same partition key are stored together, in sorted order by the sort key value
    -   Local Secondary Index (LSI):
        -   Has the SAME partition key, but different sort key
        -   Can ONLY be created when creating a table
        -   Can not be removed or modified after creation
        -   Can have up to 5 LSI's per table
    -   Global Secondary Index (GSI):
        -   Has DIFFERENT partition key and different sort key
        -   Can be created at table creation or added LATER
        -   Can have up to 5 GSI's per table
-   Streams:
    -   Used to capture any kind of modification of the DynamoDB tables
    -   If new item is added to the table, the stream captures an image of the entire item, including all of its attributes
    -   If an item is updated, the stream captures the before and after image of any attributes that were modified in the item
    -   If an item is deleted from the table, the stream captures an image of the entire item before it was deleted
    -   Streams are stored for 24 hours and then is lost
    -   Streams can trigger functions with Lambda that will perform actions based on the instantiation of a stream event
-   Query's:
    -   Operation that finds items in a table using only the primary key attribute value
    -   Must provide a partition attribute name and distinct value to search for
    -   Optionally can provide a sort key attribute name and value and use comparison operator to refine the search results
    -   By default a query returns all of the data attributes for items with the specified primary key(s)
    -   The ProjectionExpression parameter can be used to only return some of the attributes from a query as opposed to the default all
    -   Results are always sorted by the sort key
    -   If the data type of the sort key is a number, the results are returned in numeric order
    -   If the data type of the sort key is a string, the results are returned in order of ASCII character code values
    -   Sort order is ascending, the ScanIndexForward parameter can be set to false to sort in descending order
    -   By default queries are eventually consistent but can be changed to strongly consistent
    -   More efficient then a scan operation
    -   For quicker response times, design your tables in a way that can use the query, GET, or BatchGetItem API
-   Scans:
    -   Examines every item in the table
    -   By default, a scan returns all of the data attributes for every item
    -   Can use the ProjectionExpression parameter so that the scan only returns some of the attributes, instead of all
    -   Always scans the entire table, then filters out values to provide the desired result (added step of removing data from initial dataset)
    -   Should be avoided on a large table with a filter that removes many results
    -   As table grows, the scan operation slows
    -   Examines every item for the requested values, and can use up provisioned throughput for a large table in a single operation
-   Provisioned Throughput
    -   400 HTTP status code - ProvisionedThroughputExceededException error will indicate that you exceeded your max allowed provisioned throughput for a table or for one or more GSI's
    -   Unit of read provisioned throughput:
        -   All reads are rounded up to increments of 4 KB
        -   Eventual consistent reads (default) consist of 2 reads per second
        -   Strongly consistent reads consist of 1 read per second
        -   Take the (size of the read rounded to the nearest 4 KB chunk / 4 KB) * No. of items = read throughput
        -   Divide by 2 if eventually consistent
        -   Example:
            -   Application requires to read 10 items of 1 KB per second using eventual consistency, whats the read throughput
            -   Calculate the number of read units per item needed
            -   1 KB rounded to the nearest 4 KB increment = 4 (KB) or a single chunk
            -   4 KB / 4 KB = 1 read unit per item
            -   1 x 10 read items = 10
            -   Using eventual consistency is 10 /2 = 5
            -   5 units of read throughput
        -   Example 2:
            -   Application requires to read 10 items of 6 KB per second using eventual consistency, whats the read throughput
            -   Calculate the number of read units per item needed
            -   6 KB rounded to the nearest 4 KB increment = 8 (KB) or 2 chunks of 4 KB
            -   8 KB / 4 KB = 2 read unit per item
            -   2 x 10 read items = 20
            -   Using eventual consistency is 20 /2 = 10
            -   10 units of read throughput
    -   Unit of write provisioned throughput:
        -   All writes are 1 KB
        -   All writes consist of 1 write per second
        -   Example:
            -   Application requires to write 5 items with each being 10KB in size per second
            -   Each write unit consists of 1 KB of data, need to write 5 items per second with each item using 10 KB of data
            -   5 items * 10 KB = 50 write units
            -   Write throughput is 50 units
        -   Example 2:
            -   Application requires to write 12 items with each being 100KB in size per second
            -   Each write unit consists of 1 KB of data, need to write 12 items per second with each item using 100 KB of data
            -   12 items * 100 KB = 1200 write units
            -   Write throughput is 1200 units
-   Web Identity Providers:
    -   Authenticate users using Web Identity Providers such as Facebook, Google, Amazon or any other ID Connect-compatible identity provider
    -   Accomplished using AssumeRoleWithWebIdentity API
    -   Need to create a role first
    -   Process:
        -   User authentication request sent and received with the identity provider such as Facebook, Google, etc..
        -   Web Identity token returned from provider
        -   Token, App ID of provider, and ARN of IAM Role sent to AssumeRoleWithIdentity API endpoint
        -   AWS issues temporary security credentials back to the user allowing the user to access resources (1 hour default)
        -   Temporary security credentials response consist of 4 things:
            -   AccessKeyID, SecretAccessKey, SessionToken
            -   Expiration (time limit, 1 hour by default)
            -   AssumeRoleID
            -   SubjectFromWebIdentityToken
            
----------------------------------------------

# AWS Lambda
## AWS Lambda language support 
- Node.js (JavaScript) 
- Python 
- Java (Java 8 compatible) 
- C# (.NET Core) 
- Golang 
- C# / Powershell 
- Ruby 
- Custom Runtime API (community supported, example Rust)

## AWS Lambda Pricing: example 
- You can find overall pricing information here: https://aws.amazon.com/lambda/pricing/ 
- Pay per calls : 
    - First 1,000,000 requests are free 
    - $0.20 per 1 million requests thereafter ($0.0000002 per request) 
- Pay per duration: (in increment of 100ms) 
    - 400,000 GB-seconds of compute time per month if FREE 
    - == 400,000 seconds if function is 1GB RAM 
    - == 3,200,000 seconds if function is 128 MB RAM 
    - After that $1.00 for 600,000 GB-seconds

## Lambda – Synchronous Invocations
- Synchronous: CLI, SDK, API Gateway, Application Load Balancer
- Results is returned right away
- Error handling must happen client side (retries, exponential backoff, etc…)

## Lambda - Synchronous Invocations - Services
- User Invoked:
    - Elastic Load Balancing (Application Load Balancer)
    - Amazon API Gateway
    - Amazon CloudFront (Lambda@Edge)
    - Amazon S3 Batch
- Service Invoked:
    - Amazon Cognito
    - AWS Step Functions
- Other Services:
    - Amazon Lex
    - Amazon Alexa
    - Amazon Kinesis Data Firehose

## Lambda – Asynchronous Invocations
- S3, SNS, CloudWatch Events…
- The events are placed in an Event Queue
- Lambda attempts to retry on errors
    - 3 tries total
    - 1 minute wait after 1st , then 2 minutes wait
- Make sure the processing is idempotent (in case of retries)
- If the function is retried, you will see duplicate logs entries in CloudWatch Logs
- Can define a DLQ (dead-letter queue) – SNS
or SQS – for failed processing (need correct IAM permissions)
- Asynchronous invocations allow you to speed
up the processing if you don’t need to wait for
the result (ex: you need 1000 files processed)

## Lambda - Asynchronous Invocations - Services
- Amazon Simple Storage Service (S3)
- Amazon Simple Notification Service (SNS)
- Amazon CloudWatch Events / EventBridge
- AWS CodeCommit (CodeCommit Trigger: new branch, new tag, new push)
- AWS CodePipeline (invoke a Lambda function during the pipeline, Lambda must callback)
----- other -----
- Amazon CloudWatch Logs (log processing)
- Amazon Simple Email Service
- AWS CloudFormation
- AWS Config
- AWS IoT
- AWS IoT Events

## Lambda@Edge
- You have deployed a CDN using CloudFront
- What if you wanted to run a global AWS Lambda alongside?
- Or how to implement request filtering before reaching your application?
- For this, you can use Lambda@Edge:
deploy Lambda functions alongside your CloudFront CDN
    - Build more responsive applications
    - You don’t manage servers, Lambda is deployed globally
    - Customize the CDN content
    - Pay only for what you use

## Lambda Execution Role (IAM Role)
- Grants the Lambda function permissions to AWS services / resources
- Sample managed policies for Lambda:
    - AWSLambdaBasicExecutionRole – Upload logs to CloudWatch.
    - AWSLambdaKinesisExecutionRole – Read from Kinesis
    - AWSLambdaDynamoDBExecutionRole – Read from DynamoDB Streams
    - AWSLambdaSQSQueueExecutionRole – Read from SQS
    - AWSLambdaVPCAccessExecutionRole – Deploy Lambda function in VPC
    - AWSXRayDaemonWriteAccess – Upload trace data to X-Ray.
- When you use an event source mapping to invoke your function, Lambda uses the execution role to read event data.
- Best practice: create one Lambda Execution Role per function

## Lambda Resource Based Policies
- Use resource-based policies to give other accounts and AWS services permission to use your Lambda resources
- Similar to S3 bucket policies for S3 bucket
- An IAM principal can access Lambda:
- if the IAM policy attached to the principal authorizes it (e.g. user access)
- OR if the resource-based policy authorizes (e.g. service access)
- When an AWS service like Amazon S3 calls your Lambda function, the resource-based policy gives it access.

## Lambda Environment Variables
- Environment variable = key / value pair in “String” form
- Adjust the function behavior without updating code
- The environment variables are available to your code
- Lambda Service adds its own system environment variables as well
- Helpful to store secrets (encrypted by KMS)
- Secrets can be encrypted by the Lambda service key, or your own CMK

## Lambda Functions /tmp space
- If your Lambda function needs to download a big file to work…
- If your Lambda function needs disk space to perform operations…
- You can use the /tmp directory
- Max size is 512MB
- The directory content remains when the execution context is frozen,
providing transient cache that can be used for multiple invocations
(helpful to checkpoint your work)
- For permanent persistence of object (non temporary), use S3

## Lambda Function Dependencies
- If your Lambda function depends on external libraries: for example AWS X-Ray SDK, Database Clients, etc…
- You need to install the packages alongside your code and zip it together
- For Node.js, use npm & “node_modules” directory
- For Python, use pip --target options
- For Java, include the relevant .jar files
- Upload the zip straight to Lambda if less than 50MB, else to S3 first
- Native libraries work: they need to be compiled on Amazon Linux
- AWS SDK comes by default with every Lambda function

## AWS Lambda Limits to Know - per region
- Execution:
    - Memory allocation: 128 MB – 3008 MB (64 MB increments)
    - Maximum execution time: 900 seconds (15 minutes)
    - Environment variables (4 KB)
    - Disk capacity in the “function container” (in /tmp): 512 MB
    - Concurrency executions: 1000 (can be increased)
- Deployment:
    - Lambda function deployment size (compressed .zip): 50 MB
    - Size of uncompressed deployment (code + dependencies): 250 MB
    - Can use the /tmp directory to load other files at startup
    - Size of environment variables: 4 KB

## AWS Lambda Best Practices
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

----------------------------------------------

# API Gateway
## API Gateway -- Integrations High Level
- Lambda Function
  - Invoke Lambda function
  - Easy way to expose REST API backed by AWS Lambda
- HTTP 
  - Expose HTTP endpoints in the backend
  - Example: internal HTTP API on premise, Application Load Balancer... 
  - Why? Add rate limiting, caching, user authentications, API keys, etc... 
- AWS Service 
  - Expose any AWS API through the API Gateway? 
  - Example: start an AWS Step Function workflow, post a message to SQS 
  - Why? Add authentication, deploy publicly, rate control...
  
## API Gateway - Endpoint Types 
- Edge-Optimized (default): For global clients - Requests are routed through the CloudFront Edge locations (improves latency) - The API Gateway still lives in only one region 
- Regional: - For clients within the same region - Could manually combine with CloudFront (more control over the caching strategies and the distribution) 
- Private: - Can only be accessed from your VPC using an interface VPC endpoint (ENI) - Use a resource policy to define access

## API Gateway -- Deployment Stages 
- Making changes in the API Gateway does not mean they're effective 
- You need to make a "deployment" for them to be in effect 
- It's a common source of confusion 
- Changes are deployed to "Stages" (as many as you want) 
- Use the naming you like for stages (dev, test, prod) 
- Each stage has its own configuration parameters 
- Stages can be rolled back as a history of deployments is kept

## API Gateway -- Stage Variables 
- Stage variables are like environment variables for API Gateway 
- Use them to change often changing configuration values 
- They can be used in: 
  - Lambda function ARN 
  - HTTP Endpoint 
  - Parameter mapping templates 
- Use cases: 
  - Configure HTTP endpoints your stages talk to (dev, test, prod...) 
  - Pass configuration parameters to AWS Lambda through mapping templates 
- Stage variables are passed to the "context" object in AWS Lambda

## API Gateway - Integration Types 
- Integration Type MOCK 
  - API Gateway returns a response without sending the request to the backend 
- Integration Type HTTP / AWS (Lambda & AWS Services) 
  - you must configure both the integration request and integration response 
  - Setup data mapping using mapping templates for the request & response
- Integration Type AWS_PROXY (Lambda Proxy): 
  - incoming request from the client is the input to Lambda 
  - The function is responsible for the logic of request / response 
  - No mapping template, headers, query string parameters... are passed as arguments
- Integration Type HTTP_PROXY 
  - No mapping template 
  - The HTTP request is passed to the backend 
  - The HTTP response from the backend is forwarded by API Gateway

----------------------------------------------

# Amazon Cognito 
- We want to give our users an identity so that they can interact with our application. 
- Cognito User Pools: 
    - Sign in functionality for app users 
    - Integrate with API Gateway & Application Load Balancer 
- Cognito Identity Pools (Federated Identity): 
    - Provide AWS credentials to users so they can access AWS resources directly 
    - Integrate with Cognito User Pools as an identity provider 
- Cognito Sync: 
    - Synchronize data from device to Cognito. 
    - Is deprecated and replaced by AppSync 
- Cognito vs IAM: "hundreds of users", "mobile users", "authenticate with SAML"

## Cognito User Pools (CUP) -- User Features 
- Create a serverless database of user for your web & mobile apps 
- Simple login: Username (or email) / password combination 
- Password reset 
- Email & Phone Number Verification 
- Multi-factor authentication (MFA) 
- Federated Identities: users from Facebook, Google, SAML... 
- Feature: block users if their credentials are compromised elsewhere 
- Login sends back a JSON Web Token (JWT)

## Cognito User Pools -- Hosted Authentication UI 
- Cognito has a hosted authentication UI that you can add to your app to handle signup and sign-in workflows 
- Using the hosted UI, you have a foundation for integration with social logins, OIDC or SAML 
- Can customize with a custom logo and custom CSS

## Cognito Identity Pools (Federated Identities) 
- Get identities for "users" so they obtain temporary AWS credentials 
- Your identity pool (e.g identity source) can include: 
    - Public Providers (Login with Amazon, Facebook, Google, Apple) 
    - Users in an Amazon Cognito user pool - OpenID Connect Providers & SAML Identity Providers 
    - Developer Authenticated Identities (custom login server) 
    - Cognito Identity Pools allow for unauthenticated (guest) access 
- Users can then access AWS services directly or through API Gateway 
    - The IAM policies applied to the credentials are defined in Cognito 
    - They can be customized based on the user_id for fine grained control

## Cognito Identity Pools -- IAM Roles 
- Default IAM roles for authenticated and guest users 
- Define rules to choose the role for each user based on the user's ID 
- You can partition your users' access using policy variables 
- IAM credentials are obtained by Cognito Identity Pools through STS 
- The roles must have a "trust" policy of Cognito Identity Pools

## Cognito User Pools vs Identity Pools 
- Cognito User Pools: 
    - Database of users for your web and mobile application 
    - Allows to federate logins through Public Social, OIDC, SAML... 
    - Can customize the hosted UI for authentication (including the logo)] 
    - Has triggers with AWS Lambda during the authentication flow 
- Cognito Identity Pools: 
    - Obtain AWS credentials for your users 
    - Users can login through Public Social, OIDC, SAML & Cognito User Pools 
    - Users can be unauthenticated (guests) 
    - Users are mapped to IAM roles & policies, can leverage policy variables 
    - CUP + CIP = manage user / password + access AWS services

    ----------------------------------------------

    # ECS

## Docker
- Docker is a software development platform to deploy apps
- Apps are packaged in containers that can be run on any OS
- Apps run the same, regardless of where they’re run
- Any machine
- No compatibility issues
- Predictable behavior
- Less work
- Easier to maintain and deploy
- Works with any language, any OS, any technology

## Docker Containers Management
- To manage containers, we need a container management platform
- Three choices:
- ECS: Amazon’s own platform
- Fargate: Amazon’s own Serverless platform
- EKS: Amazon’s managed Kubernetes (open source)

## ECS Clusters 
- ECS Clusters are logical grouping of EC2 instances
- EC2 instances run the ECS agent (Docker container)
- The ECS agents registers the instance to the ECS cluster
- The EC2 instances run a special AMI, made specifically for ECS

## ECS Task Definitions 
- Tasks definitions are metadata in JSON form to tell ECS how to run a Docker Container
- It contains crucial information around: 
    - Image Name 
    - Port Binding for Container and Host 
    - Memory and CPU required 
    - Environment variables 
    - Networking information 
    - IAM Role 
    - Logging configuration (ex CloudWatch)

## ECR
- So far we’ve been using Docker images from Docker Hub (public)
- ECR is a private Docker image repository
- Access is controlled through IAM (permission errors => policy)
- AWS CLI v1 login command (may be asked at the exam)
    - $(aws ecr get-login --no-include-email --region eu-west-1)
- AWS CLI v2 login command (newer, may also be asked at the exam - pipe)
    - aws ecr get-login-password --region eu-west-1 | docker login --username AWS -- password-stdin 1234567890.dkr.ecr.eu-west-1.amazonaws.com
- Docker Push & Pull:
    - docker push 1234567890.dkr.ecr.eu-west-1.amazonaws.com/demo:latest
    - docker pull 1234567890.dkr.ecr.eu-west-1.amazonaws.com/demo:latest

## Fargate
- When launching an ECS Cluster, we have to create our EC2 instances
- If we need to scale, we need to add EC2 instances
- So we manage infrastructure…
- With Fargate, it’s all Serverless!
- We don’t provision EC2 instances
- We just create task definitions, and AWS will run our containers for us
- To scale, just increase the task number. Simple! No more EC2 

## ECS Task Placement Process
- Task placement strategies are a best effort
- When Amazon ECS places tasks, it uses the following process to select container instances:
    1. Identify the instances that satisfy the CPU, memory, and port requirements in the task definition.
    2. Identify the instances that satisfy the task placement constraints.
    3. Identify the instances that satisfy the task placement strategies.
    4. Select the instances for task placement.

## ECS Task Placement Strategies
- **Binpack**
    - Place tasks based on the least available amount of CPU or memory
    - This minimizes the number of instances in use (cost savings)
- **Random**
    - Place the task randomly
- **Spread**
    - Place the task evenly based on the specified value
    - Example: instanceId, attribute:ecs.availability-zone

## ECS – Service Auto Scaling
- CPU and RAM is tracked in CloudWatch at the ECS service level
- Target Tracking: target a specific average CloudWatch metric
- Step Scaling: scale based on CloudWatch alarms
- Scheduled Scaling: based on predictable changes
- ECS Service Scaling (task level) ≠ EC2 Auto Scaling (instance level)
- Fargate Auto Scaling is much easier to setup (because serverless)

# AWS SysOps Administrator Associate


Monitoring:
===========

* * * * *

Monitoring is accomplished through the usage of CloudWatch, which is a service to monitor your AWS resources as well as the applications that you run on AWS.

> **CloudWatch Monitoring:**

-   Can monitor EC2 instances, Autoscaling Groups, ELBs, Route53 Health Checks, EBS Volumes, Storage Gateways, CloudFront, DynamoDB, ElastiCache nodes, RDS instances, EMR Job Flows, Redshift. SNS topics, SQS Queues, OpsWorks, CloudWatch Logs, Estimated charges on your AWS bill, and custom metrics - logs generated by your applications and services.
-   EC2 will by default monitor your instances @5 minute intervals
-   EC2 instances can monitor your instances @1 minute intervals if the 'detailed monitoring' option is set on the instance
-   By default CloudWatch will monitor CPU, Network, Disk, and Status Checks
-   RAM utilization is a custom metric and must be added manually to EC2 instances in order to be tracked.
-   2 types of Status Checks:
    -   System Status Checks (Physical Host):
        -   Checks the underlying physical host
        -   Checks for loss of network connectivity
        -   Checks for loss of system power
        -   Checks for software issues on the physical host
        -   Checks for hardware issues on the physical host
        -   Best way to resolve issues is to stop the instance and start it again (will switch physical hosts)
    -   Instance Status Checks
        -   Checks the VM itself
        -   Checks for failed system status checks
        -   Checks for mis-configured networking or startup configs
        -   Checks for exhausted memory
        -   Checks for corrupted file systems
        -   Checks for an incompatible kernel
        -   Best way to troubleshoot is rebooting the instance or modifying the instance OS
-   By default CloudWatch metrics are stored for 2 weeks
-   Can retrieve data that is longer than 2 weeks using the GetMetricStatistics API endpoint, or by using third party tools
-   Can retrieve data from any terminated EC2 or ELB instance for up to 2 weeks after its termination
-   Many default metrics for many default services are 1 min, but it can be 3-5 minutes depending on the service
-   Custom metrics have a minimum 1 minute granularity
-   Alarms can be created to monitor any CloudWatch metric in your account
-   Alarms can include EC2, CPU, ELB, Latency, or even changes on your AWS bill
-   Within the alarm, actions can be set, triggering things like lambda functions, or SNS notifications if the alarm threshold is reached

> [**Configuring custom metrics:**](https://aws.amazon.com/code/8720044071969977)

-   In order to allow custom metrics to be written to CloudWatch, you must assign a CloudWatch full access role to the EC2 instance using the custom metrics.
-   RAM utilization for example must be set up as a custom metric
-   yum install -y perl-Switch perl-DateTime perl-Sys-Syslog perl-LWP-Protocol-https
-   mkdir /CloudWatch && cd /CloudWatch
-   wget http://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.1.zip
-   unzip CloudWatchMonitoringScripts-v1.2.1.zip
-   rm -fr CloudWatchMonitoringScripts-v1.2.1.zip
-   cd aws-scripts-mon
-   ./mon-put-instance-data.pl --mem-util --verify --verbose (dry run no data will be sent to CloudWatch)
-   ./mon-put-instance-data.pl --mem-util --mem-used --mem-avail (set this up on 1/5 minute cron job)
-   Set Cron job to run regulary (*/5 * ** * ec2-user /CloudWatch/mon-put-instance-data.pl --mem-util --mem-used)

> **Monitoring EBS:**

-   4 Types of EBS Storage, General Purpose (SSD) - gp2, Provisioned IOPS (SSD) - io1, Throughput Optimized (HDD) - st1, and Cold (HDD) - sc1
-   Throughput Optimized HDDs (ST1) and Cold HDDs (SC1), both CAN NOT BE USED AS BOOT VOLUMES!
-   Throughput Optimized HDDs (ST1) and Cold HDDs (SC1), both are not available in the drop list if the volume is the root volume. Adding an additional volume will allow these option types to become present in the drop list.
-   GP2 volumes have a base of 3 IOPS per GiB of volume size
-   Maximum volume size is 16 TB
-   Maximum IOPS size of 10K IOPS total (after which you need to move to provisioned IOPS storage tier)
-   Can burst performance on the volume up to 3K IOPS
-   Bursting uses I/O credits
-   Each volume receives an initial I/O credit balance of 5.4 million I/O credits
-   This is enough to sustain the max burst performance of 3K IOPS for 30 minutes (3K being the MAX iOPS available, including your standard 3 IOPS per GB. You can not burst an additional 3K to your standard, only burst up to a max of 3K)
-   If you need more than 3K IOPS then you need to increase the volume size accordingly via the 3 IOPS per GB rule
-   When not going over provisioned IO level (bursting) you earn credits back
-   Don't need to know the calculation to replenish the credit balance
-   New volumes no longer require pre-warming, they receive their maximum performance the moment that they are available and do not require initialization / pre-warming.
-   When restoring a volume from snapshots, the first time you access the storage block, you can see a 5 to 50 % loss of IOPS due the volume either needing to be wiped clean or instantiated from a snapshot
-   Performance is restored after the data is accessed once
-   To avoid the performance hit, volumes can be pre-warmed
-   For a new volume, you should write to all blocks before using the volume
-   For a volume that has been restored from a snapshot, you should read all blocks that have data before using the volume
-   Instructions for pre-warming volumes can be found [here](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-initialize.html)
-   EBS CloudWatch Metrics:
    -   VolumeReadBytes
    -   VolumeWriteBytes
        -   Provides info on the I/O operations in a specified period of time
        -   The SUM statistic reports the total number of bytes transferred during the period
        -   The AVG statistic reports the average size of each I/O operation during the period
        -   The SampleCount statistic reports the total number of I/O operations during the period
        -   The Minimum and Maximum statistics are not relevant for this metric
        -   Data is only reported to CloudWatch when the volume is active
        -   If the volume is idle, no data is reported to CloudWatch
    -   VolumeReadOps
    -   VOlumeWriteOps
        -   The total number of I/O operations in a specified period of time
        -   To calculate the AVG IOPS for the period, divide the total operations in the period by the number of seconds in that period
    -   VolumeTotalReadTime
    -   VolumeTotalWriteTime
        -   The total number of seconds spent by all operations that completed in a specified period of time
        -   If multiple requests are submitted a the same time, the total could be greater than the length of the period
    -   VolumeIdleTime
        -   The total number of seconds in a specified period of time when no read or write operations were submitted
    -   VolumeQueueLength
        -   Then number of read and write operation requests waiting to be completed in a specified period of time
        -   If the count is high, it would be a good indicator to up the volume size to get more IOPS available via the 3 IOPS per GiB rule
    -   VolumeThroughputPercentage
        -   Used with Provisioned IOPS (SSD) volumes only
        -   The percentage of IOPS delivered of the total IOPS provisioned for an EBS volume
        -   Provisioned IOPS SSD volumes deliver within 10% of the provisioned IPS performance 99.9% of the time over a given year
        -   During a write, if there are no other pending I/O requests in a minute, the metric value will be 100%
        -   A volume's I/O performance may become degraded temporarily due to an action that was taken (such as creating a snapshot of a volume during peak usage, or running the volume on a non-EBS-optimized instance, or accessing data on the volume for the first time, if the volume wasn't pre-warmed)
    -   VolumeConsumedReadWriteOps
        -   Used with Provisioned IOPS (SSD) volumes only
        -   The total amount of read and write operations (normalized to 256K capacity units) consumed in a specified period of time
        -   I/O operations that are smaller than 256K each count as 1 consumed IOPS
        -   I/O operations that are larger than 256K are counted in 256K capacity units
-   VolumeQueueLength can come up frequently, know what it is
-   Volume Status Checks:
    -   OK:
        -   I/O Enabled status:
            -   Enabled (I/O Enabled or I/O Auto-Enabled)
        -   I/O Performance Status:
            -   Only available for Provisioned IOPS (IO1) volumes
            -   Normal (Volume performance is as expected)
    -   Warning:
        -   I/O Enabled status:
            -   Enabled (I/O Enabled or I/O Auto-Enabled)
        -   I/O Performance Status:
            -   Only available for Provisioned IOPS (IO1) volumes
            -   Degraded (Volume performance is below expectations)
    -   Impaired:
        -   I/O Enabled status:
            -   Enabled (I/O Enabled or I/O Auto-Enabled)
            -   Disabled (volume is off-line and pending recovery, or is waiting for the user to enable I/O)
        -   I/O Performance Status:
            -   Only available for Provisioned IOPS (IO1) volumes
            -   Stalled (Volume performance is severely impacted)
    -   Insufficient Data:
        -   I/O Enabled status:
            -   Enabled (I/O Enabled or I/O Auto-Enabled)
            -   Insufficient Data
        -   I/O Performance Status:
            -   Only available for Provisioned IOPS (IO1) volumes
            -   Insufficient Data
-   Degraded, Severely Degraded = Warning
-   Stalled or Not Available = Impaired
-   If your EBS volume is attached to a current generation EC2 instance type, you can increase its size, change its volume type, or adjust its IOPS performance without detaching it
-   These changes can be applied to detached volumes as well
-   From the console (Volumes Console, Not EC2 Console), or from the API, Volumes can be modifed.
-   When modifying a volume, you can monitor the progress of the modification. If the size of the volume was modified, be sure to extend the volumes file system to take advantage of the increased capacity.

> **Monitoring RDS:**

-   2 types of monitoring:
    -   Monitor by metrics (CloudWatch monitoring):
        -   Per-Database Metrics
        -   By Database Class
        -   By Database Engine
        -   Across All Databases
    -   Monitor by events (RDS monitoring):
        -   Located in Events tab
        -   Events of everything that has happened with your instance
        -   Can set event subscriptions which work like SNS topics
        -   Events like fail-overs can be a notifying event using subscriptions
        -   Available RDS Metrics:
            -   BinLogDiskUsage
            -   The amount of disk space occupied by binary logs on the master. Applies to MySQL read replicas
            -   Units: Bytes
            -   Burst Balance
            -   The percent of General Purpose SSD (gp2) burst-bucket I/O credits available
            -   Units: Percent
            -   CPUUtilization
            -   The percentage of CPU utilization.
            -   Units: Percent
            -   CPUCreditUsage (T2 Instances)
            -   The number of CPU credits consumed by the instance
            -   One CPU credit equals one vCPU running at 100% utilization for one minute or an equivalent combination of vCPUs, utilization, and time
            -   Example: one vCPU running at 50% utilization for two minutes or two vCPUs running at 25% utilization for two minutes
            -   Units: Count
            -   CPUCreditBalance (T2 Instances)
            -   The number of CPU credits available for the instance to burst beyond its base CPU utilization
            -   Credits are stored in the credit balance after they are earned and removed from the credit balance after they expire
            -   Credits expire 24 hours after they are earned
            -   CPU credit metrics are available only at a 5 minute frequency
            -   Units: Count
            -   DatabaseConnections
            -   The number of database connections in use
            -   Units: Count
            -   DiskQueueDepth
            -   The number of outstanding IOs (read/write requests) waiting to access the disk
            -   Units: Count
            -   FreeableMemory
            -   The amount of available random access memory
            -   Units: Bytes
            -   FreeStorageSpace
            -   The amount of available storage space
            -   Units: Bytes
            -   MaximumUsedTransactionIDs
            -   The maximum transaction ID that has been used. Applies to PostgreSQL
            -   Units: Count
            -   ReplicaLag (Seconds)
            -   The amount of time a Read Replica DB instance lags behind the source DB instance. Applies to MySQL, MariaDB, and PostgreSQL Read Replicas
            -   Units: Seconds
            -   ReplicationSlotDiskUsage
            -   The disk space used by replication slot files. Applies to PostgreSQL
            -   Units: Megabytes
            -   OldestReplicationSlotLag
            -   The lagging size of the replica lagging the most in terms of WAL data received. Applies to PostgreSQL
            -   Units: Megabytes
            -   TransactionLogsDiskUsage
            -   The disk space used by transaction logs. Applies to PostgreSQL
            -   Units: Megabytes
            -   TransactionLogsGeneration
            -   The size of transaction logs generated per second. Applies to PostgreSQL
            -   Units: Megabytes/second
            -   SwapUsage
            -   The amount of swap space used on the DB instance
            -   Units: Bytes
            -   ReadIOPS
            -   The average number of disk I/O operations per second
            -   Units: Count/Second
            -   WriteIOPS
            -   The average number of disk I/O operations per second
            -   Units: Count/Second
            -   ReadLatency
            -   The average amount of time taken per disk I/O operation
            -   Units: Seconds
            -   WriteLatency
            -   The average amount of time taken per disk I/O operation
            -   Units: Seconds
            -   ReadThroughput
            -   The average number of bytes read from disk per second
            -   Units: Bytes/Second
            -   WriteThroughput
            -   The average number of bytes written to disk per second
            -   Units: Bytes/Second
            -   NetworkReceiveThroughput
            -   The incoming (Receive) network traffic on the DB instance, including both customer database traffic and Amazon RDS traffic used for monitoring and replication
            -   Units: Bytes/second
            -   NetworkTransmitThroughput
            -   The outgoing (Transmit) network traffic on the DB instance, including both customer database traffic and Amazon RDS traffic used for monitoring and replication
            -   Units: Bytes/second
        -   Have general idea of what each of the RDS metrics do
        -   DatabaseConnections, DiskQueueDepth, FreeStorageSpace, ReplicaLag (Seconds), ReadIOPS, WriteIOPS, ReadLatency, WriteLatency are all important ones to know

> **Monitoring ELB:**

-   Monitored every 60 seconds provided there is traffic
-   Only reports when requests are flowing through the LB
-   If there are no requests or data for a given metric, the metric will not be reported to CloudWatch
-   If there are requests flowing through the LB, ELB will measure and send metrics for that LB in 60 second intervals
-   Available Metrics:
    -   HealthyHostCount:
        -   The count of the number of healthy instances in each AZ
        -   Hosts are declared healthy if they meet the threshold for the number or consecutive health checks that are successful
        -   Hosts that have failed more health checks then the value of the unhealthy threshold are considered unhealthy
        -   If cross-zone is enabled, the count of the number of healthy instances is calculated for all AZs
        -   Preferred Statistic: Average
    -   UnHealthyHostCount:
        -   The count of the number of unhealthy instances in each AZ
        -   Hosts that have failed more health cheeks than the value of the unhealthy threshold are considered unhealthy
        -   If cross-zone is enabled, the count of the number of unhealthy instances is calculated for all AZs
        -   Instances may become unhealthy due to connectivity issues, health checks returning non-200 responses (in the case of HTTP or HTTPS health checks), or timeouts when performing the health check
        -   Preferred Statistic: Average
    -   RequestCount:
        -   The count of the number of completed requests that were received and routed to the back end instances
        -   Preferred Statistic: Sum
    -   Latency:
        -   Measures the time elapsed in seconds after the request leaves the load balancer until the response is received
        -   Preferred Statistic: Average
    -   HTTPCode_ELB_4XX
        -   The count of the number of HTTP 4XX client error codes generated by the load balancer when the listener is configured to use HTTP or HTTPS protocols. Client errors are generated when a request is malformed or is incomplete
        -   Preferred Statistic: Sum
    -   HTTPCode_ELB_5XX
        -   The count of the number or HTTP 5XX server error codes generated by the load balancer when the listener is configured to use HTTP or HTTPS protocols
        -   This metric does not include any responses generated by back end instances
        -   The metric is reported if there are no back-end instances that are healthy or registered to the load balancer, or if the request rate exceeds the capacity of the instances or the load balancers
        -   Preferred Statistic: Sum
    -   HTTPCode_Backend_2XX:
    -   HTTPCode_Backend_3XX:
    -   HTTPCode_Backend_4XX:
    -   HTTPCode_Backend_5XX:
        -   The count of the number of HTTP response codes generated by back-end instances
        -   Metric does not include any response codes generated by the load balancer
        -   The 2XX class status codes represent successful actions
        -   The 3XX class status codes indicate that the user agent requires action
        -   The 4XX class status code represents client errors
        -   The 5XX class status code represents back-end server errors
        -   Preferred Statistic: Sum
    -   BackendConnectionErrors:
        -   The count of the number of connections that were not successfully established between the LB and the registered instances
        -   The LB will retry when there are connection errors, so the count can exceed the request rate
        -   Preferred Statistic: Sum
    -   SurgeQueueLength:
        -   A count of the total number of requests that are pending submission to a registered instance
        -   Preferred Statistic: Max
    -   SpilloverCount:
        -   A count of the total number of requests that were rejected due to the queue being full
        -   Preferred Statistic: Sum
-   Have an idea of what each metric does
-   Important metrics to note are SurgeQueueLength & SpilloverCount

> **Monitoring Elasticache:**

-   Consists of 2 different engines:
    -   Memcached
    -   Redis
-   When it comes to monitoring cache engines, there are 4 monitoring points:
    -   CPU Utilization
        -   Memcached:
        -   Multi-threaded
        -   Handles loads of up to 90% CPU utilization
        -   If > 90% CPU utilization add more nodes to the cluster
        -   Redis:
        -   Single-threaded
        -   Take 90% and / number of cores to determine scale point
        -   Will not have to calculate Redis CPU utilization in exam
    -   Swap Usage
    -   Memcached:
        -   Should be around 0 most of the time and should not exceed 50MB
        -   If 50MB is exceeded, you should increase the memecached_connections_overhead parameter
        -   memecached_connections_overhead defines the amount of memory to be reserved for Memcached connections and other misc. overhead
    -   Redis
        -   No SwapUsage metric, instead use reserved-memory
    -   The amount of the Swap file that is used.
    -   Swap file is the amount of disk storage space reserved on disk if your computer runs out of RAM
    -   Typically the size of the swap file = the amount of RAM available
    -   Evictions
    -   Memcached:
        -   No recommended setting
        -   Choose a threshold based off your application
        -   Scale up (increase the memory of existing nodes) or Scale out (add more nodes) to avoid evictions
    -   Redis:
        -   No recommended setting
        -   Choose a threshold based off your application
        -   Only scale out (add read replicas) to avoid evictions
    -   Like clowns stuffed in a car, There is a finite number of empty seats that slowly fill up. Eventually the car is full and if more seats are needed, then an Eviction will occur
    -   Evictions occur when a new item is added and an old item must be removed due to lack of free space on the system
    -   Concurrent Connections
    -   No recommended setting
    -   Choose a threshold based off your application
    -   If there is a large and sustained spike in the number of concurrent connections, this can either mean a large traffic spike or your application is not releasing connections efficiently

Organizations & Consolidated Billing:
=====================================

* * * * *

AWS Organizations is an account management service that enables you to consolidate multiple AWS accounts into an organization that you create and centrally manage

> **Consolidated Billing:**

-   Have a single payer account
-   Have multiple linked accounts that all roll up to the payer account for billing purposes
-   Payer account is independent and cannot access resources of any linked account
-   Linked accounts are also independent and cannot access resources in any of the other linked accounts, or the payer account
-   Currently there is a limit of 20 linked accounts for consolidated billing, unless a limit increase is requested
-   Advantages include a single bill per AWS account, easy way to track charges and allocate costs, and volume pricing discount availability
-   Always enable MFA on root account, always use strong and complex passwords on the root account, Payer account should be used for billing purposes only, do not deploy resources in the payer account
-   When monitoring is enabled on the payer account, the billing data for all linked accounts is also included
-   You can still create billing alerts per individual accounts as well
-   CloudTrail is per AWS account and is enabled per region
-   CloudTrail can be aggregated in to a single bucket in the payer account
-   Consolidated billing allows you to get volume discounts on all of your accounts
-   Unused reserved instances for EC2 are applied across the group

> **Cost Optimization:**

-   3 different instance types:
-   Spot
    -   Allow you to name your own price for EC2 capacity
    -   You bid on spare EC2 instances and these will run automatically whenever your bid exceeds the current spot price
    -   Spot price varies in real time based on supply and demand
    -   If the spot price goes above your bid price after the instances are provisioned, the instances will be automatically terminated
-   Reserved Instances
    -   Provide you with up to 75% discount as compared to on-demand pricing
    -   You are assured that your RI will always be available for the OS and AZ in which you purchased it
    -   For applications that have steady state needs, RIs provide significant savings compared to using on-demand instances
    -   RI's and On-demand instances perform identically.
-   On Demand
    -   Pay for compute capacity by the hour with no long term commitments or upfront payments
    -   Increase or decrease your compute capacity depending on the demands of your application and only pay the specified hourly rate for the instances that you use
    -   EC2 always strives to have enough capacity to meet customer needs, but during high periods of high demand, it is possible that you may not be able to launch specific instance types in specific AZs

Elasticity and Scalability:
===========================

* * * * *

Elasticity is focused around being able to scale your infrastructure up, and down automatically based on traffic, where Scalability is focused on scaling your infrastrucure out more permanentlty. - Elasticity - Allows you to stretch out and retract your infrastructure based on demand - Pay for only what you need - Used during a short time period, such as hours or days - EC2: - Increase instance sizes as required using RIs - DynamoDB - Increase additional IOPS for additional spikes in traffic, then decrease IOPS after the spike - RDS - Not elastic, can't scale RDS based on demand

-   Scalability
-   Used to talk about building out the infrastructure to meet your demands long term
-   Used over a longer time period such as weeks, days months and years
-   EC2:
    -   Increase the number of EC2 instances based on Autoscaling
-   DynamoDB
    -   Unlimited amount of storage
-   RDS

    -   Increase instance size from small to medium
-   Scale UP vs Scale Out

-   Scale Up
    -   Increase the number of CPUs, RMA, or the amount of storage
    -   EC2: increase the instance type from say a T1.micro to T2.small or T2.medium, etc..
    -   If questions appear to be network related, then its probably a scale up answer
-   Scale Out
    -   Add more resources such as web servers
    -   EC2: add additional EC2 instances and Autoscaling
    -   If questions appear to be in relation to not having enough resources, then its probably a scale out answer.

RDS Multi Availability Zones & Failover:
========================================

* * * * *

> Multi AZ Deployment:

-   Multi AZ deployments for MySQL, PostgreSQL, and Oracle engines utilize synchronous physical replication to keep data on the standby up to date with the primary
-   Multi AZ deployments for the MSSQL Server engine uses synchronous logical replication to achieve the same result, employing SQL Server native mirroring technology
-   Both approaches safeguard your data in the event of a DB instance failure or loss of an AZ
-   Failovers are handled via DNS moves from a primary to secondary instances on the backend
-   During a failover your connection URL string does not change
-   High Availability:
-   Backups are taken from secondary which avoids I/) suspension to the primary
-   Restores are taken from the secondary which avoids I/o suspensions to the primary
-   You can force a failover from one AZ to another by rebooting your instance. This can be done from the AWS management console or by using the RebootDBInstance API Call
-   RDS Multi AZ failover is NOT A SCALING SOLUTION
-   Read Replicas are used to scale only

> Read Replicas:

-   MySQL, PostgreSQL, MariaDB
-   Amazon uses these engines native asynchronous replication to update the read replica
-   Aurora
-   Not yet covered by the exam
-   Not using Asynchronous replication
-   Uses an SSD backed virtualized storage layer purpose built for DB workloads
-   Aurora replicas share the same underlying storage as the source instance
-   Using same storage lowers cost and avoids the need to copy data to the replica nodes over the network

-   Make it easy to take advantage of supported engines built in replication functionality

-   Used to elastically scale out beyond the capacity constraints of a single DB instance for read heavy workloads
-   You can create a read replica within a few clicks in the AWS management console
-   Can also create a read replica with the CreateDBInstanceReadReplica API call
-   Once the read replica is created, database updates on the source DB instance will be replicated using a supported engines native asynchronous replication
-   Can create multiple read replicas for a given source DB instance and distribute your applications read traffic among them
-   Can have up to 5 read replicas for any 1 primary db instance

-   When to use Read Replicas:

-   Scaling beyond the compute or I/O capacity of a single DB instance for read heavy database workloads
-   The access read traffic can be directed to one or more read replicas
-   Serving read traffic while the source DB instance is unavailable
-   If your source DB instance cannot take I/O requests, you can direct read traffic to your read replicas
-   Commonly used for business reporting or data warehousing scenarios
-   Reports or data warehousing queries are typically ran against read replicas instead of the primary production DB instance

-   Creating a read replica

-   AWS takes a snapshot of your database
-   If Multi AZ is not enabled, the snapshot will be of the primary database and can cause brief I/O suspension for around 1 minute
-   If Multi AZ is enabled, then snapshots will be taken of the secondary database and there will be no performance impact on your primary db

-   Connecting to a read replica

-   Read replicas have a new DNS a record created that should be used to directly access the read replica when the read replica is created
-   You can promote a read replica to its own standalone db.
-   Doing this will break the replication link between the primary and secondary db

-   Read Replica Tips:

-   Can have up to 5 read replicas for MySQL, PostgreSQL, and MariaDB
-   Can have read replicas in different regions for all engines
-   Replication is asynchronous only not synchronous
-   Read replicas can be built off Multi AZ DBs
-   Read replicas themselves cannot be Multi AZ currently
-   Can have read replicas of read replicas, but beware of latency
-   DB snapshots and automated backups cannot be taken of read replicas
-   Key metric is ReplicaLag
-   Know differences between read replicas and Multi AZ RDS instances

> RDS Multi AZ and Read Replicas:

-   When you delete an RDS database, the default action will be set to take a final snapshot, this can be over-ridden with a drop down on the delete screen in the AWS management console
-   DB Instance Identifiers which are created during the creation of an RDS instance must be unique withing the same AWS account for each RDS instance created
-   Instance Identifier is the name of the database instance, not the database name, which is specified on a separate view when creating in the AWS management console
-   Can now Enable IAM DB authentication, which will allow you to control authentication into DB instances via IAM users and groups
-   Cannot create a read replica initially because no snapshot is present, must take a snapshot in order to create a read replica
-   Multi AZ can be turned on from a standalone. This can be done via a snapshot and restore, or you can modify an existing RDS instance, and change the Multi AZ Deployment drop option from No to Yes
-   Modifying the database will take the database off line while it applies its modifications
-   Modifying or changing an existing db will not change the DNS record
-   Creating a new instance from an existing snapshot will provision a new DNS record
-   In order to create a read replica of a read replica, Database backups must be turned on (or a snapshot must exist)

-   RDS Tips

-   Know the difference between read replicas and Multi AZ (scale out vs DR)
-   If you can't create a read replica you most likely have disabled db backups, change it and turn it on
-   you can create read replica of read replica's in multiple regions
-   you can modify the DB itself or create a new database from a snapshot
-   Endpoints DO NOT CHNAGE if you modify a db, they will change if you create a new d from a snap or if you create a read replica
-   You can manually fail over a multi AZ DB from one AZ to another by rebooting it

Connectivity and Troubleshooting:
=================================

* * * * *

> **Connectivity:**

-   Bastion hosts serve as a more secure way to connect to your VPC and AWS Infrastructure components
-   Bastion hosts act as a gateway between you and your EC2 instances
-   Bastion hosts help reduce attack vectors on your infrastructure and means that you only have to harden 1-2 EC2 instances as opposed to the entire fleet
-   1 subnet = 1 AZ, a single subnet cannot span more than 1 Availability Zone
-   Bastion hosts are used by allowing SSH/RDP connections directly to the hosts, and only those hosts are allowed to SSH/RDP into the rest of your EC2 instances

> **High Availability Troubleshooting:**

-   Things to look for if your instances are not launching into an Autoscaling group:
-   Associated Key Pair does not exist
-   Security Group does not exist
-   Autoscaling config is not working correctly
-   Autoscaling group not found
-   Instance type specified is not supported in the AZ
-   Invalid EBS device mapping
-   Autoscaling service is not enabled on your account
-   Attempting to attach EBS block device to an instance store AMI

Elastic Load Balancers:
=======================

* * * * *

> **Root Access:**

-   The following services still allow root access to the hosts provisioned by the corresponding services:
-   Elastic Beanstalk
-   Elastic MapReduce
-   OpsWorks
-   EC2
-   ECS

> **ELB Configurations:**

-   You can use ELBs or Elastic Load Balancers to load balance across different AZ's within the same region, but not to different regions or different VPC's themselves
-   An ELB is different than a NAT
-   Can have 2 types of ELBs:
-   External ELB's with External DNS names
-   Internal ELB's with Internal DNS names
-   Health checks can be configured to check backend services via protocols such as HTTP/HTTPS
-   Health check intervals are calculated by multiplying the Health Check Interval x Healthy or Unhealthy Threshold value.
-   In the example that the HC Interval is 30 sec, and the Threshold is set to 2, after 2 30 second cycles or 1 minute the host will be marked unhealthy
-   Supports Sticky Sessions:
-   Not enabled by default
-   By default the ELB routes each request independently to the application with the smallest amount of load
-   The sticky session feature (session affinity) enables the ELB to lock a user down to a specific web server (EC2 instance)
-   All requests at that point from the user during the session are always sent to the same server
-   To manage sessions, determine how long your ELB should consistently route the user's request to the same application server
-   2 Types of session stickiness:
    -   Duration based:
    -   Most commonly used
    -   The ELB creates a session cookie
    -   When the ELB receives a request, it checks to see if this cookie is present in the request.
    -   If the cookie is present, then the request is sent to the server specified in the cookie.
    -   If the cookie is not present, the ELB chooses a backend server based on the existing load balancing algorithm and adds a new cookie to the response
    -   The stickiness policy config defines the cookie expiration, which establishes the duration of validity for each cookie
    -   The cookie is automatically updated after the duration expires.
    -   If the backend sever fails or becomes unhealthy, the ELB stops routing requests to it and instead chooses a new instance based on the selected algorithm
    -   In the event of failure, the request is routed to the new instance as if there is no cookie and the session is no longer sticky
    -   Application controlled:
    -   The ELB uses a special cookie to associate the session with the original server that handled the request, but follows the lifetime of the application generated cookie corresponding to the cookie name specified in the policy configuration
    -   The ELB only inserts a new stickiness cookie if the application response includes a new application cookie
    -   The ELB stickiness cookie does not update with each request
    -   If the ELB stickiness cookie is explicitly removed or expires, the session stops being sticky until a new application cookie is issued
    -   If an application instance fails or becomes unhealthy, the ELB stops routing requests to that instance, and instead chooses a new healthy instance based on the existing load balancing algorithm
    -   The ELB will treat the session as now stuck to the new healthy instance and continue routing requests to that instance even if the failed instance comes back online
    -   It is up to the new application instance whether and how to respond to a session which it has not previously seen
-   ELB Metrics:
    -   HealthyHostCount - The number of healthy instances in each AZ. Hosts are declared healthy if they meet the threshold for the number of consecutive health checks that are successful. Hosts that have failed more health checks then the value of the unhealthy threshold are considered unhealthy. If cross-zone is enabled, the count of the number of healthy instances is calculated for all AZ's. The preferred statistic is the average
    -   UnHealthyHostCount - The count of the the number of unhealthy instances in each AZ. Hosts that have failed more health checks then the value of the unhealthy threshold are considered unhealthy. If cross-zone is enabled the count of the number of unhealthy instances is calculated for all AZ's. Instances may become unhealthy due to connectivity issues, health checks returning non 200 responses (in the case of HTTP or HTTPS health checks), or timeouts when performing the health check. The preferred statistic is the average
    -   RequestCount - The count of the number of completed requests that were received and routed to the back end instances. The preferred statistic is the sum
    -   Latency - Measures the time elapsed in seconds after the request leaves the ELB until the response is received. The Preferred statistic is the average
    -   HTTPCode_ELB_4XX - The count of the number of 4XX client error codes generated by the load balancer when the listener is configured to use HTTP or HTTPS. Client errors are generated when a request is malformed or incomplete. The preferred statistic is the sum
    -   HTTPCode_ELB_5XX - The count of the number of HTTP 5XX server error codes generated by the load balancer when the listener is configured to use HTTP or HTTPS. This metric does not include any responses generated by back end instances. The metric is reported if there are no back end instances that are healthy or registered to the load balancer, or if the request rate exceeds the capacity of the instances or the load balancers. The preferred statistic is sum
    -   HTTPCode_Backed_2/3/4/5XX - The count of the number of HTTP response codes generated by back end instances. This metric does not include any response codes generated by the load balancer. The 2XX class status codes represent successful actions. The 3XX status codes indicate that the user agent requires action. The 4XX class status codes represent client errors, and the 5XX class status codes represents back end instance errors. The preferred statistic is sum
    -   BackendConnectionErrors - The count of the number of connections that were not successfully established between the load balancer and the registered instances. Because the load balancer will retry when there are connection errors, this count can exceed the request rate. The preferred statistic is sum
    -   SurgeQueueLength - The count of the total number of requests that are pending submission to a registered instance. The preferred statistic is max
    -   SpilloverCount - A count of the total number of requests that were rejected due to the queue being full. The preferred statistic is sum
    -   Pay attention to SurgeQueueLength and SpilloverCount
-   Pre Warming ELB's:
    -   AWS can pre-configure the ELB to have the appropriate level of capacity based on expected traffic
    -   Used in scenarios such as when flash traffic is expected, or in the case where a load test cannot be configured to gradually increase traffic
    -   This can be done by contacting AWS prior to the expected event. You will need to know the following:
    -   The start and end date of the expected flash traffic
    -   The expected request rate per second
    -   The total size of the typical request/response that you will be sending/receiving

Backups and Disaster Recovery:
==============================

* * * * *

> **Disaster Recovery:**

-   DR is about preparing for and recovering from a disaster
-   Any event that has a negative impact on a company's business continuity or finances could be termed a disaster
-   Disasters include software, hardware, network failures, power outages, physical damage to buildings like fire or flooding, human error, etc..
-   Traditional approaches involve an N+1 approach and have different levels of off-site duplication of data and/or infrastructure
-   Advantages of using AWS for DR
-   Only minimum hardware is required for data replication
-   Allows you flexibility depending on what your disaster is and how to recover from it
-   Open cost model (pay as you use) rather than heavy investment upfront
-   Scaling is quick and easy
-   Automate infrastructure for DR deployments
-   AWS storage Gateways:
-   Gateway cached volumes store primary data and cache most recently used data locally
-   Gateway stored volumes store entire datasets on site and asynchronously replicate data back to S3
-   Gateway virtual tape libraries store your virtual tapes in either S3 or Glacier
-   RTO vs RPO
-   RTO or Recovery Time Objective is the length of time from which you can recover from a disaster
-   RTO is measured from when the disaster first occurred to when you have fully recovered from it
-   RPO or Recovery Point Objective is the amount of data your organization is prepared to lose in the event of a disaster
-   RPO examples are only allowing 1 day of email loss, or 5 hours of transaction records lost, 24 hours of backups, etc..
-   Typically the lower RTO and RPO thresholds that are set, the more costly the solution will be

> **DR Strategies:**

-   Pilot Light:
-   The term used to describe a DR scenario in which a minimal version of the environment is always running in the cloud
-   Similar to a backup and restore scenario. AWS can maintain a pilot light by configuring and running the most critical core elements of your system in AWS. When the time comes for recovery, the environment can quickly provision a full scale production environment around the critical core via auto-scaling and other measures.
-   Typically includes Databases, which could be replicated to RDS or EC2 instances along with any other critical core components
-   Rest of your infrastructure can be set up using pre-configured AMI's and Cloudformation
-   For networking, use pre-allocated EIP's and associate them with your instances when invoking DR, or use pre-allocated ENI's with pre-allocated MAC addresses for applications with special licensing requirements
-   Use ELBs to distribute traffic to multiple instances, and update DNS records to point at your EC2 instances or point to the ELB's using CNAME records
-   Warm Standby:
-   Used to describe a DR scenario in which a scaled down version of a fully functional environment is always running in the cloud
-   Extends the pilot light elements and decreases the recovery time because some services are always running
-   By identifying business critical systems, you can fully duplicate those systems on AWS and have them always on
-   Critical components can be running on minimum sized instances. The scenario is not scaled to handle production load, but is fully functional
-   Can be used for non production work, such as testing, QA and internal use
-   In a disaster, the system can be scaled horizontally or vertically quickly to handle production load
-   In AWS you can simply add more instances to the environment or by resizing the small capacity servers to run on larger instance types
-   Horizontal scaling is preferred over vertical scaling
-   To set up Warm Standby:
    -   Set up EC2 instances to replicate or mirror data
    -   Create and maintain AMIs
    -   Run your application using a minimal footprint of EC2 instances or AWS infrastructure
    -   Patch and update software and configuration files in line with your environment
    -   Increase the size of the EC2 fleet in service with the load balancer (horizontal scaling)
    -   Start applications on larger EC2 instance types as needed (vertical scaling)
    -   Either manually change the DNS records or use Route53's automated health checks so that all traffic is routed to the AWS environment
    -   Consider using auto scaling to right size the fleet or accommodate the increased load
    -   Add resilience or scale up your database
-   Multi-Site:
-   Runs in AWS as well as your existing on-site infrastructure in an active active configuration
-   The data replication method that you employ will be determined by the recovery point that you choose
-   You can use Route53 to root traffic to both sites either symmetrically or asymmetrically
-   In an on-site disaster recovery situation, you can adjust the DNS weighting and send all traffic to AWS servers
-   The capacity of the AWS service can be rapidly increased to handle the full production load
-   You can use EC2 Auto scaling to automate the process
-   May need some application logic to detect the failure of the primary database service and cut over to the parallel database service running in AWS
-   To set up Multi-site Standby:
-   Set up AWS environment to duplicate your production environment
-   Set up DNS weighting or a similar traffic routing technology, to distribute incoming requests to both sites
-   Configure automatic failover to re-route traffic away from the affected site
-   Have application logic for failover to use the local AWS database servers for all queries
-   Move from DR Site back to primary site:
    -   Establish reverse mirroring / replication from the DR site back to the primary site
    -   Wait for primary site to catch up to DR site
    -   Freeze data changes to the DR site
    -   Re-Point users back to the primary site
    -   UnFreeze the changes

> **Backups:**

-   Traditionally data is backed up to tape and sent off site regularly
-   Using the tape method can take a long time to restore your system in the event of a disaster
-   S3 is an ideal destination for backup data that might need to be restored quickly
-   Transferring data to and from S3 is typically done through the network and therefor accessible from any location
-   Can use AWS import/export to transfer very large data sets by shipping storage devices directly to AWS
-   For longer term data storage where retrieval times of several hours are adequate, Glacier can be leveraged
-   Glacier has the same durability model as S3, and can be used in conjunction with S3 to produce a tiered backup solution
-   Select an appropriate tool or method to backup your data to AWS
-   Ensure you have an appropriate retention policy for your data
-   Ensure that appropriate security measures are in place for the data including encryption and access policies
-   Regularly test the recovery and restoration of the data and applicable systems
-   Moving from DR back to Primary Site:
-   Freeze data changes to the DR site
-   Take backup
-   Restore the backup to the primary site
-   Re-point users to the primary site
-   Unfreeze changes

> **Services with automated backups:**

-   RDS
-   Need InnoDB (translated Engine)
-   Performance hit if Multi-AZ is not enabled
-   If you delete an instance, then ALL automated backups are deleted
-   Manual DB snapshots will NOT be deleted
-   All backups stored on S3
-   When you do a restore, you can change the engine type (SQL standard to SQL Enterprise for example), provided you have enough space
-   Elasticache (Redis Only)
-   Available for Redis Cache Cluster only
-   The entire cluster is snapshotted
-   Snapshots WILL degrade performance
-   Set the snapshot window during the least busy part of the day
-   All snapshots stored on S3
-   Redshift
-   By default Redshift enables automated backups of your data warehouse cluster with a 1 day retention
-   Redshift only backs up data that has changed, so most snapshots only use up a small amount of backup storage
-   All snapshots stored on S3
-   EC2 does NOT have automated backups (Can take snapshots, but they are not automated)
-   No automated backups
-   Backups degrade your performance, schedule these times during off peak hours
-   Can create automated backups using either the CLI interface or Python
-   Snapshots are Incremental:
    -   Snapshots only store incremental changes since the last snapshot
    -   Only charged for incremental storage
    -   Each snapshot still contains the base snapshot data
-   All snapshots are stored on S3

EC2 & EBS:
==========

* * * * *

> **EC2:**

-   When EC2 was first launched all AMI's were backed by Instance store or Ephemeral storage
-   Ephemeral storage is non-persist or temporary storage
-   When an instance is shut down, even if turned back up, the the contents of the instance store, or ephemeral storage will be gone, and unaccessible
-   Stopping and restarting an instance moves the instance to another host, hence the lost data
-   EC2 eventually got the ability to attach EBS or Elastic Block Storage which allows for data persistence
-   There is NO way to flag data preservation on ephemeral storage, if the instance restarts, or the host experiences issues, you can incur data loss
-   2 types of Volumes
-   Root Volume:
    -   This is where your operating system is installed
    -   Can either be EBS or Ephemeral
    -   Max size is 10GB
    -   EBS root device volume can be up to 1 or 2TB depending on OS
    -   Delete on Terminate is the default value
-   Additional Volumes:
    -   This can be your D:, E:, F: / dev/sdb, /dev/sdc, /dev/sdd etc..
    -   Delete on Terminate is NOT the default value, additional volumes WILL persist after the instance is terminated and must be manually deleted

> **EBS:**

-   Allows users to have data persistence
-   EBS volumes can be detached from an instance and attached to other instances without data loss
-   EBS volumes can only be attached to a single instance at a time
-   EBS root volumes are terminated/deleted by default when the EC2 instance is terminated
-   Termination/Deletion default behavior can be stopped by un-selecting the "Delete on Termination" option when creating the instance or by setting the deleteontermination flag to false using the command line at boot time
-   Non root EBS volumes attached to the instance are preserved if you delete the instance
-   Boot time is quicker using EBS, typically less than 1 minute, where Instance store volumes are generally less than 5 minutes
-   Must manually delete additional EBS volumes when an instance is terminated. Failure to do so will hold a storage charge for unattached non deleted volumes

> **Snapshots:**

-   Exist on S3, you do not have access to the snapshots directly, but on the backend they are stored on S3
-   Snapshots are point in time copies of volumes
-   Snapshots are incremental, only the blocks that have changed since your last snapshot are moved to S3
-   The first snapshot takes some time to create as its a full snapshot of the volume
-   To create a snapshot for EBS volumes that serve as root devices you should stop the instance before taking the snapshot
-   You can take a snapshot while the instance is running
-   You can create AMI's from both volumes and snapshots
-   You can change EBS volume sizes on the fly, including changing the size and storage type
-   Volumes will ALWAYS be in the same AZ as the EC2 instance
-   To move an EC2 volume from one AZ/Region to another, take a snapshot or an image of it and then copy it to the new AZ/Region
-   Snapshots of encrypted volumes are encrypted automatically
-   Volumes restored from encrypted snapshots are encrypted automatically
-   You can share snapshots, but only if they are unencrypted
-   Shared snapshots can be shared with other AWS accounts or made public

Opsworks:
=========

* * * * *

-   Cloud based applications usually require a group of related resources that must be created and managed collectively
-   The collection of instances is called a stack
-   Opsworks provides a simple and straight forward way to create and manage stacks and their associated resources
-   Opsworks is an application management service that helps automate operational tasks like code deployment, software configurations, package installations, database setups, and server scaling using Chef
-   Provides the flexibility to define your application architecture and resource configuration and handles the provisioning and management of your AWS resources for you
-   Includes automation to scale your application based on time or load, monitoring to help troubleshot and take automated actions based on the state of your resources
-   Handles permissions and policy management to make management of multi-user environments easier
-   Chef turns infrastructure into code
-   Can automate how you build, deploy, and manage your infrastructure
-   Allows infrastructure to become as versionalble, testable, and repeatable as application code
-   Chef server stores your recipes as well as other configuration data
-   The chef client is installed on each server, instance, container or networking device that you manage referred to as nodes
-   The client periodically polls the Chef server for the latest policy and state of the network, if anything is out of date, the client brings it up to date
-   Opsworks provides a GUI to deploy and configure your infrastructure quickly
-   Consists of 2 elements, Stacks and Layers
-   A stack is a container or group of resources such as ELBs, EC2 instances, RDS instances, etc
-   A layer exists within a stack and consists of things like a web application layer
-   Think of a stack as a virtual data center
-   Each function is a different layer, you can wrap up the full configuration of a component within a layer such as PHP, Apache, etc..
-   Need 1 or more layer per stack
-   An instance must be assigned to at least 1 layer
-   Which Chef layers run, are determined by the layer the instance belongs to
-   There are pre-configured layers that will auto provision things such as Applications, Databases, Load balancing, or Caching
-   if you select an existing ELB to be used in a layer, Opsworks will remove any currently registered instances and then manages the ELB for you. If you use the ELB console to modify the configuration, the changes will NOT be permanent

Security:
=========

* * * * *

> **Shared Security Model:**

-   AWS Responsibilities:
-   Securing the underlying infrastructure that supports the cloud
-   Protecting the global infrastructure that runs all of the services offered on AWS
-   All hardware, software, networking, and facilities that run AWS services
-   Security configuration of its products and services that are considered managed services
    -   DynamoDB
    -   RDS
    -   Redshift
    -   EMR
    -   Workspaces
    -   Workmail
    -   etc...
-   Patching of managed service nodes
-   Antivirus for managed service nodes
-   Storage device decommissioning, with prevention of customer data exposure
-   AWS uses techniques detailed in DoD 5220.22-M or NIST 800-88 to destroy data as part of its decommissioning process
-   All decommissioned magnetic storage devices are degaussed and physically destroyed in accordance with industry standard practices
-   AWS corporate network is completely segregated from the AWS production network by means of complex network security devices
-   AWS provides protection against DDOS, Man in the Middle attacks, Ip Spoofing, Port Scanning and Packet Sniffing by other tenants
-   Different instances run on the same physical hardware and are isolated from each other via the Xen hypervisor
-   AWS has firewalls that reside in the hypervisor layer, between the physical network interface and the instances virtual interfaces
-   All network packets must pass through the firewall layer, ensuring that no instance has access to any other instance other than what is intended. Instance traffic to other instances is treated the same as public internet traffic
-   Customer instances have no access to raw disk devices, but are presented instead with virtual disks
-   AWS proprietary disk virtualization automatically resets each block of storage used by customers so that one customers data is never unintentionally exposed to another
-   Memory allocated to guests is scrubbed or set to 0 by the hypervisor when it is unallocated from a guest
-   Unallocated memory is NEVER returned to the pool of free memory until the memory scrubbing process is complete
-   AWS Service compliance
    -   SOC 1/SSAE 16/ISAE 3402 (formerly SAS 70 Type II)
    -   SOC2
    -   SOC3
    -   FISMA
    -   DIACAP
    -   FedRAMP
    -   PCI DSS Level 1
    -   ISO 27001
    -   ISO 9001
    -   ITAR
    -   FIPS 140-2
    -   HIPPA
    -   Cloud Security Alliance (CSA)
    -   Motion Picture Association of America (MPAA)
-   AWS provides their annual certifications and compliance reports
-   User Responsibilities:
-   Anything that is put on the cloud or connects to the cloud
-   IAAS (Infrastructure as a service) components require the user to perform all security configuration and management tasks
    -   EC2
    -   VPC
    -   S3
-   Account management and user access
-   MFA implementation
-   Communication to services using SSL/TLS
-   Logging of API/User activity via CloudTrail
-   Protecting data transmission via HTTPS using SSL
-   Obtaining permission from AWS to perform penetration testing and or port scanning against your AWS Nodes
-   All vulnerability scans, port scans, and penetration testing requests MUST be submitted in advance and approved by AWS
-   When requesting and granted permission for port scanning, scans must be limited to your own instances
-   Unauthorized port scans are a violation of the AWS Acceptable Use Policy
-   Checking Trusted Advisor (TA) recommendations for potential cost savings, system performance improvements, and potential security gaps
-   TA can provide alerts on security misconfiguration, such as open ports, public access to S3 buckets, user logging activities, lack of MFA, and more
-   Guest operating system on non-managed services such as EC2 are under the full control of the user
-   AWS does NOT have any login or access rights to your instances guest operating system
-   Configuration of EC2 firewall. The inbound firewall configured on each EC2 instances is set by default in deny-all mode
-   Users are fully responsible for explicitly opening the ports needed to allow inbound traffic to their instances
-   User is responsible for using AWS's provided encryption options to encrypt EBS volumes and their snapshots with AES-256 bit encryption
    -   Encryption occurs on the servers that host the EC2 instances and EBS storage
    -   EBS Encryption is only available on EC2's bigger instance types such as the M, C, R, and G instance families
-   SSL Termination on ELBs
-   Anything put on AWS assets including the raw data compliance

> **IAM Policies:**

-   Each IAM Policy must contain the Resource property
-   Policies consist of 3 main components, Action, Resource, and Effect
-   Effect - Whether the policy allows or denies access
-   Action -- The list of actions that are allowed or denied by the policy
-   Resource -- The list of resources on which the actions can occur
-   Condition (Optional) -- The circumstances under which the policy grants permission
-   Roles are more secure than programmatic access, and should always be used as the first resort where possible
-   All IAM users should have MFA (Multi-Factor Authentication) enabled

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:ListBucket",
    "Resource": "arn:aws:s3:::example_bucket"
  }
}

```

This sample policy would allow a ListBucket Request to be performed on the example_bucket S3 bucket for example.

> **STS (Security Token Service):**

-   Grants users limited and temporary access to AWS resources
-   Users can come from 3 different sources:
-   Federation (Active Directory):
    -   Uses Security Assertion Markup Language (SAML)
    -   Grants temporary access based off hte users AD credentials
    -   Does not need to be an IAM user
    -   Single sign on allows users to log into the AWS console without assigning IAM credentials
-   Federation with Mobile Apps:
    -   Use Facebook, Amazon, Google, or other OpenID providers to log in
-   Cross Account Access:
    -   Lets users from one AWS account access to resources in another AWS account
-   Federation - Combining or joining a list of users in one domain with a list of users in another domain (Active Directory -> IAM for example)
-   Identity Broker - A service that allows you to take an identity from Domain A and join it (federate it) to Domain B
-   Identity Store - Services like Active Directory, Facebook, Google, Amazon, etc..
-   Identities - A user of a service like Amazon, Facebook, Google, etc..
-   Steps of Authentication:
-   User enters username/password
-   Application calls an Identity Broker. The broker is passed the username/password
-   The Identity Broker uses the organizations centralized authentication to validate the identity of the user (Think Active Directory)
-   The Identity Broker then calls the new GetFederationToken function using IAM credentials. The call must include an IAM policy and duration (1-36 hours), along with a policy that specifies the permissions to be granted to the temporary security credentials
-   STS confirms that the policy of the user making the call gives permission to create new tokens and then returns 4 values
    -   Access Key
    -   Secret Access Key
    -   Token
    -   Duration of token
-   Identity Broker returns the temporary security credentials to the requesting application
-   The requesting application uses the temporary security credentials and token to make requests to Amazon
-   Amazon uses IAM to verify that the credentials allow the requested operation on the given service using the given key
-   IAM provides the service with a allowed action to perform the requested operation
-   Steps in Simplicity:
-   Develop an Identity Broker to communicate with LDAP and AWS STS
-   Identity Broker should always authenticate with LDAP first, then the STS service
-   Application gets temporary access to AWS resources

Route53:
========

* * * * *

> **DNS:**

-   DNS or Domain Name System is used to convert human friendly domain names into IP addresses
-   2 types of IP addresses:
-   IPv4
    -   32 bit address
    -   4 billion different addresses (4,294,967,296)
-   IPv6
    -   Created to solve depletion issue of IPv4 address space
    -   128 bit address
    -   340 undecillion addresses (340,282,366,920,938,463,463,374,607,431,768,211,456)
-   Top Level Domains: Signified by the last word in a domain name
-   .com
-   .edu
-   .gov
-   .net
-   .io
-   etc
-   Controlled by the Internet Assigned Numbers Authority (IANA)
-   Stored in a root zone database which is a database of all available TLDs (Top Level Domains)
-   Database can be found at <http://www.iana.org/domains/root/db>
-   Domain Names:
-   All names in a given domain name have to be unique
-   DNS registrars are authority's that can assign domain names directly under one or more TLD's
-   Domains are registered with InterNIC, as service of ICANN, which enforces uniqueness of domain names across the internet
-   Each domain name becomes registered in a central database known as the WhoIS database
-   Popular domain registrars include godaddy.com, namecheap.com, Route53 etc..
-   SOA (Start of Authority) Records store information about:
-   Name of the server that supplied the data for the zone
-   Administrator of the zone
-   Current version of the data file
-   Number of seconds a secondary name server should wait before checking for updates
-   Number of seconds a secondary name server should wait before retrying a failed domain transfer
-   Maximum number of seconds that a secondary name server can use data before it must either be refreshed or expired
-   Default number of seconds for the TTL (Time to Live) on resource records
-   DNS Record Types:
-   NS or Name Server Records are used by TLD's to direct traffic to the content DNS server which contains the authoritative DNS records
-   A or Address records are used by a computer to translate the name of the domain to an IP address
-   CNAMES or Canonical Names can be used to resolve one domain name to another
    -   CNAME's can't be used for naked domain names (zone apex). As such awsdocs.com must be either an A record or an Alias record
-   Alias records are used to map resource record sets in your hosted zone to ELBs, CloudFront Distributions, or S3 Buckets that are configured as websites
    -   Alias records work like CNAME records in that you can map one DNS name to another target DNS name
    -   Alias records can save time because Route53 automatically recognizes changes in the record set that the alias resource record set refers to
    -   You are NOT charged for requests to Alias records, you ARE charged for requests to CNAMES, so using Alias records is cheaper
-   TTL or Time to Live is the length that a DNS record is cached on either the resolving server or the users local PC. The lower the TTL, the faster changes to DNS records take to propagate throughout the internet
-   ELBs do not have a pre-defined IPv4 address, DNS names are used for ELB resolution
-   Understand the difference between an Alias Record and a CNAME
-   Always use an Alias record over a CNAME where possible, as it's cheaper and faster

> **Route53 Routing Policies:**

-   Simple
-   Default routing policy when you create a new record set
-   Most commonly used when you have a single resource that performs a given function for your domain
-   Example would be a single web server that serves content for a single domain name
-   Weighted
-   Use to route traffic to multiple resources in proportions that you specify
-   Split traffic based on different weights assigned within the record set
-   Example would be sending 10% of user traffic to US-East-1 and the other 90% to US-East-2
-   Latency
-   Use when you have resources in multiple locations and you want to route traffic to the resource that provides the least latency
-   Route traffic based on lowest network latency for your end users, such as sending requests to the region that will give the user the fasted response time
-   Create a resource record set for EC2 or ELB resources in each region that hosts your content. When Route53 receives a request for your content, it selects the latency resource record for the region that gives the user the lowest latency
-   Failover
-   Use when you want to configure active-passive failover
-   Example would be when you want your primary site to be in US-East-1, and a DR site in US-West-1
-   Route53 will monitor the health of our primary site using a health check
-   Health checks are not automatic and must be configured by the user
-   GeoLocation
-   Use when you want to route traffic based on the location of your users
-   Example would be ensuring that EU customers get routed to servers residing in the EU, and ensuring US customers get routed to servers residing in the US

VPCs and Direct Connect:
========================

* * * * *

> **VPC's:**

Lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. You have complete control over your virtual networking, IP ranges, creation of subnets and configuration of route tables and network gateways.

-   Virtual data center in the cloud
-   Allowed up to 5 VPCs in each AWS region by default. This limit can be increased with a support ticket request
-   All subnets in default VPC have an Internet gateway attached
-   Multiple IGW's can be created, but only a single IGW can be attached to a VPC.. No exceptions
-   Again, You can only have 1 Internet gateway per VPC
-   Each EC2 instance has both a public and private IP address
-   If you delete the default VPC, the only way to get it back is to submit a support ticket
-   This answer is correct for the current iteration of tests, however AWS has now crated a mechanism in the console that allows you to recreate a default VPC
-   By default when you create a VPC, a default main routing table automatically gets created as well.
-   Subnets are always mapped to a single AZ
-   Subnets can not be mapped to multiple AZ's
-   /16 is the largest CIDR block available when provisioning an IP space for a VPC
-   /28 is the smallest CIDR block available when provisioning an IP space for a VPC
-   Amazon uses 3 of the available IP addresses in a newly created subnet
    -   x.x.x.0 - Always subnet network address and is never usable
    -   x.x.x.1 - Reserved by AWS for the VPC router
    -   x.x.x.2 - Reserved by AWS for subnet DNS
    -   x.x.x.3 - Reserved by AWS for future use
    -   x.x.x.255 - Always subnet broadcast address and is never usable.
-   169.254.169.253 - Amazon DNS
-   By default all traffic between subnets is allowed
-   By default not all subnets have access to the Internet. Either an Internet Gateway or NAT gateway is required for private subnets
-   A security group can stretch across different AZ's
-   Security Groups are stateful (Don't need to open inbound and outbound, if inbound is allowed, outbound is auto allowed)
-   Network Access Control Lists (NACLs) are stateless (Must define both inbound and outbound rules)
-   You can also create Hardware Virtual Private Network (VPN) connection between your corporate data center and your VPC and leverage the AWS cloud as an extension of your corporate data center
-   VPC Flow Logs:
-   VPC Flow Logs is a feature that enables the user to capture information about the IP traffic going to and from network interfaces in your VPC
-   Flow log data is stored using Cloudwatch Logs
-   When Flow log data is collected it can be viewed and its data can be retrieved within Cloudwatch
-   Flow logs can be created at 3 different levels, VPC, Subnet and Network Interface levels
-   Flow logs via Cloudwatch can be configured to stream to services such as Elasticache, or Lambda
-   You cannot enable flow logs for VPC's that are peered with your VPC unless the peer VPC is in your account
-   You cannot tag a flow log
-   After you have created a flow log, you cannot change its configuration, for example you cannot associate a different role with the flow log
-   Not all traffic is monitored:
    -   Traffic generated by instances when they contact Route53 is not monitored or logged
    -   If you use your own DNS server, then all traffic to that DNS server is logged
    -   Traffic generated by a Windows instance for Windows license activation is not monitored or logged
    -   Traffic to and from the metadata service (169.254.169.254) is not monitored or logged
    -   DHCP traffic is not monitored or logged
    -   Traffic to the reserved IP address for the default VPC router is not monitored or logged
-   Network Address Translation (NAT) Instances:
    -   When creating a NAT instance, disable Source/Destination checks on the instance or you could encounter issues
    -   NAT instances must be in a public subnet
    -   There must be a route out of the private subnet to the NAT instance in order for it to work
    -   The amount of traffic that NAT instances support depend on the size of the NAT instance. If bottlenecked, increase the instance size
    -   If you are experiencing any sort of bottleneck issues with a NAT instance, then increase the instance size
    -   HA can be achieved by using Auto-scaling groups, or multiple subnets in different AZ's with a scripted fail-over procedure
    -   NAT instances are always behind a security group
-   Network Address Translation (NAT) Gateway:
    -   NAT Gateways scale automatically up to 10Gbps
    -   There is no need to patch NAT gateways as the AMI is handled by AWS
    -   NAT gateways are automatically assigned a public IP address
    -   When a new NAT gateway has been created, remember to update your route table
    -   No need to assign a security group, NAT gateways are not associated with security groups
    -   Preferred in the Enterprise
    -   No need to disable Source/Destination checks
    -   More secure than a NAT instance
-   Network Access Control Lists (NACLS):
    -   NACL's are stateless, meaning both inbound and outbound rules must be configured for traditional request/response model
    -   Numbered list of rules that are evaluated in order starting at the lowest numbered rule first to determine what traffic is allowed in or out depending on what subnet is associated with the rule
    -   The highest rule number is 32766
    -   Start with rules starting at 100 so you can insert rules if needed
    -   NACL's have separate inbound and outbound rules, and each rule can either allow or deny traffic
    -   The Default NACL will allow ALL traffic in and out by default
    -   Custom NACL's by default will deny all inbound and outbound traffic until allow rules are added
    -   You must assign a NACL to each subnet, if a subnet is not associated with a NACL, it will allow no traffic in or out
    -   NACL rules are stateless, established in does not create outbound rule automatically
    -   You can only assign a single subnet to a single NACL
    -   When you associate a NACL with a subnet, any previous associations are removed
    -   You can associate a single NACL with multiple subnets
    -   Each subnet in your VPC must be associated with a NACL. If you don't explicitly associate a subnet with an ACL, the subnet automatically gets associated with the default ACL
    -   You can block IP addresses using NACLs not Security Groups
-   VPC Peering:
    -   Connection between two VPCs that enables you to route traffic between them using private IP addresses via a direct network route
    -   Instances in either VPC can communicate with each other as if they are within the same network
    -   You can create VPC peering connections between your own VPCs or with a VPC in another account within a SINGLE REGION
    -   AWS uses existing infrastructure of a VPC to create a VPC peering connection. It is not a gateway nor a VPN, and does not rely on separate hardware
    -   There is NO single point of failure for communication nor any bandwidth bottleneck
    -   There is no transitive peering between VPC peers (Can't go through 1 VPC to get to another)
    -   Hub and spoke configuration model (1 to 1)
    -   Be mindful of IPs in each VPC, if multiple VPCs have the same IP blocks, they will not be able to communicate
    -   You can peer VPC's with other AWS accounts as well as with other VPCs in the same account
-   VPC Endpoints:
-   Allows internal resources such as EC2 instances to reach various AWS services without having to traverse the public internet to get to the service
-   When you use an endpoint, the source IP address from your instances in your affected subnets for access the AWS service in the same region will use private IP address's instead of public IP address's
-   When configuring VPC endpoints, existing connections from your affected subnets to the AWS service that use public IP address's may be dropped

> **Direct Connect (DX):**

-   DX or Direct Connect makes it easy to establish a dedicated network connection from your premises to AWS
-   Using DX, you can establish private connectivity between AWs and your data center, office or collocation environment
-   Requires a dedicated line such as MPLS, or other circuit ran from tel-co.
-   From this line, you would have a cross connect from your on-premises device direct to AWS data centers
-   Using DX, can reduce network costs, increase bandwidth throughput and provide a more consistent network experience then internet based connections
-   Lets you establish a dedicated network connection between your network and one of the AWS DX locations
-   Uses industry standard 802.1Q VLANs
-   Dedicated connections can be partitioned into multiple virtual interfaces
-   Same connection can be used to access public resources such as objects stored in S3 using public IP's and private resources such as EC2 instances running in a VPC using private IP's, all while maintaining network separation between the public and private environments
-   Virtual interfaces can be reconfigured at any time to meet changing needs
-   Offers more bandwidth and a more consistent network experience over using VPN based solutions
-   VPC VPN connections utilize IPSec to establish encrypted network connectivity between your intranet and your AWS VPC over the internet
-   VPN connections can be configured in minutes and are a good solution if you have an immediate need
-   DX does NOT involve the internet, instead, it uses dedicated private network connections between your intranet and AWS VPC

# AWS Solutions Architect Associate

Regions / Availability Zones (AZs)
==================================

-   Regions : AWS Geographical regions like US East, US West, EU Central etc

-   Availability Zones : Distinct data centres that host the physical compute and other resources for AWS (AWS ensures a minimum of 2 AZs per region). They are often separated with each other but a geographical calamity could still impact/disrupt services on both

-   Edge Locations : Each Region further consists of many edge locations which basically serve cached data for frequent access from nearby users. Edge location can also be used to write data (For eg. in S3 Transfer Acceleration)

-   As of today there are approximately 15 regions, 45 AZs and ~100+ Edge locations fronted by Cloudfront. Sometimes edge locations could also be operated/managed by AWS partner network

* * * * *

Route53
=======

Fun fact : In Route53, 'Route' comes from Route 66 --- Oldest inter state highway in the United States, and port 53 used by DNS in Computer Networking

It is used for resolving DNS names to IP addresses, Registering Domain Names

There are different types of records used in DNS system:

-   SOA records

-   A records

-   CNAME records

-   MX records

-   PTR records

-   Alias records

-   NS records

A request from browser first goes to Top level domain (.com, .au, .gov etc), from there request is forwarded to Name Servers (NS), which fetch the details of A records and answer the request with respective IP address which can then be used by the browser to initiate a TCP connection.

-   When given a choice between Alias record and a CNAME record, Alias record usually offers more benefits.

-   When one domain like [m.acm.org](http://m.acm.org/) has to be routed to [mobile.acm.org](http://mobile.acm.org/), always use a CNAME record to delegate resolution to another domain

Different types of resolution policies supported by Route53 are:

-   Simple Routing Policy : In this you can also provide multiple IP addresses but cannot associate health check of all IP addresses

-   Weighted Routing Policy : Using this policy one can route traffic based on weight assigned to different IP addresses (For eg. 70% traffic to IP X, and other 30% to IP Y), which is then proportionally routed/distributed. (Sounds similar to Canary Deployments ?)

-   Latency Based Routing Policy: This is based on response latency from the location of user. Using this information, request can be routed to different servers.

-   Failover Based Routing Policy : One active, One passive --- If Active health check starts failing, traffic routes to Passive Setup

-   Geographical Routing Policy : In this we can define that user in Europe should be routed to Europe server only (This is different from Latency based routing policy and is more hard-coded per se)

-   Geographical Proximity Routing Policy : In this policy, we also have to use Route53 Traffic Flow rules to define a complex routing policy and also use bias to override the decisions taken by Route53. Using this policy one can utilise multiple configurations and control the domain resolution to IPs at a very granular level. Practically this is very less used

-   Multivalue Answer Routing Policy: Same as Simple routing policy with multiple IP addresses, but only difference is that health checks can be associated

* * * * *

IAM (Identity and Access management)
====================================

-   Users

-   Groups

-   Roles (Who you are ?)

-   Policies (What you can do?)

By default, new users have:

-   only access_key_id and secret_access_key, but no console access

-   no permissions, until they are made member of a group or assigned some roles/policies

IAM, like other AWS services is eventually consistent as this data is replicated across multiple servers. IAM is a global service (Not scoped per region). Mainly two services can allow access without authentication / authorization in AWS --- STS and S3

Following terms are used in context of IAM:

-   User : can be an IAM user or an applications accessing the AWS resources

-   Group :Group of users who can be collectively permitted some actions

-   Role : This is something which can be assumed by an entity like User or Group (This is similar to Authentication)

-   Policy : This defines the permissions you have on the resources that you want to access (This is similar to Authorization)

Request Context : This is the request object which AWS receives when somebody tries to access something or take an action on some AWS resource. This object includes source IP, resources that you are trying to access, what actions are you taking on those resources, what time of day this request originated etc.

Policies can be managed in two ways:

-   Managed policies : AWS Managed & Customer Managed

-   Inline Policies

Policies can further be of two types:

i) Identity based policies : These are attached directly to Identities like User/Groups etc. They can be managed or inline policies.

ii) Resource based policies : These are inline policies directly applied on the resource that has to be accessed from same/other accounts. This is mainly used for cross-account resource access

Policy versioning: Customer managed policies can normally have only 5 versions being managed at a single point of time. This is useful when you make a change to a policy and it breaks something, you can quickly set the default setting to a previously used policy

IAM Roles are more preferred instead of resource based policies which are not extendable to other entities

* * * * *
> **EC2:**

-   When EC2 was first launched all AMI's were backed by Instance store or Ephemeral storage
-   Ephemeral storage is non-persist or temporary storage
-   When an instance is shut down, even if turned back up, the the contents of the instance store, or ephemeral storage will be gone, and unaccessible
-   Stopping and restarting an instance moves the instance to another host, hence the lost data
-   EC2 eventually got the ability to attach EBS or Elastic Block Storage which allows for data persistence
-   There is NO way to flag data preservation on ephemeral storage, if the instance restarts, or the host experiences issues, you can incur data loss
-   2 types of Volumes
-   Root Volume:
    -   This is where your operating system is installed
    -   Can either be EBS or Ephemeral
    -   Max size is 10GB
    -   EBS root device volume can be up to 1 or 2TB depending on OS
    -   Delete on Terminate is the default value
-   Additional Volumes:
    -   This can be your D:, E:, F: / dev/sdb, /dev/sdc, /dev/sdd etc..
    -   Delete on Terminate is NOT the default value, additional volumes WILL persist after the instance is terminated and must be manually deleted
 - Termination:
    - We have an instance where shutdown behavior = terminate and enable, terminate protection is ticked
    - We shutdown the instance from the OS, what will happen ?
    - The instance will still be terminated!   

> **EBS:**

-   Allows users to have data persistence
-   EBS volumes can be detached from an instance and attached to other instances without data loss
-   EBS volumes can only be attached to a single instance at a time
-   EBS root volumes are terminated/deleted by default when the EC2 instance is terminated
-   Termination/Deletion default behavior can be stopped by un-selecting the "Delete on Termination" option when creating the instance or by setting the deleteontermination flag to false using the command line at boot time
-   Non root EBS volumes attached to the instance are preserved if you delete the instance
-   Boot time is quicker using EBS, typically less than 1 minute, where Instance store volumes are generally less than 5 minutes
-   Must manually delete additional EBS volumes when an instance is terminated. Failure to do so will hold a storage charge for unattached non deleted volumes

> **Placement Groups**

- Sometimes you want control over the EC2 Instance placement strategy
- That strategy can be defined using placement groups
- When you create a placement group, you specify one of the following
strategies for the group:
    - Cluster---clusters instances into a low-latency group in a single Availability Zone
    - Spread---spreads instances across underlying hardware (max 7 instances per
    group per AZ) -- critical applications
    - Partition---spreads instances across many different partitions (which rely on
    different sets of racks) within an AZ. Scales to 100s of EC2 instances per group
    (Hadoop, Cassandra, Kafka)

> **EC2 Instance Launch Types**
- On Demand Instances: short workload, predictable pricing
- Reserved: (MINIMUM 1 year)
    - Reserved Instances: long workloads
    - Convertible Reserved Instances: long workloads with flexible instances
    - Scheduled Reserved Instances: example -- every Thursday between 3 and 6 pm
- Spot Instances: short workloads, for cheap, can lose instances (less reliable)
- Dedicated Instances: no other customers will share your hardware
- Dedicated Hosts: book an entire physical server, control instance placement
* * * * *

Databases
=========

Databases are mainly of two types:

-   Relational --- Conventional relational databases to store data --- RDS

-   Non-relational (DynamoDB --- Like MongoDB) --- Collections contain tables which are basically JSON objects

Relational Database Engines supported by AWS are: (POMMMA)

-   PostgreSQL

-   Oracle

-   MariaDB

-   MySQL

-   MS SQL

-   Aurora

Processing types supported by RDS:

-   OLTP (Online Transaction Processing)

-   OLAP (Online Analytic Processing) --- This works on large amount of data and derives analytics out of it --- Redshift is the Amazon offering for OLAP requirements

RDS can support multi-az setup and read replicas

-   Multi-AZ setup is for disaster recovery and Read replicas are for performance --- redirecting read only queries to read replicas instead of Database masters

Read Replicas
-------------

-   Each read replica has its own DNS end point

-   Read replicas can be promoted to be their own databases --- this breaks the replication though

-   You can have a read replica in another region

-   Read replicas ONLY work if backups are turned ON

Two types of backups are possible:

-   Automated backups --- done during planned maintenance windows

-   Snapshots --- Done manually to save state of RDS

DynamoDB
--------

-   NoSQL solution from Amazon

-   Cluster is spread across 3 different segregated data centres / AZs

-   Eventual read consistency with maximum delay of 1s

-   Incoming data transfer IS NOT charged if in a single region. If you cross regions, you will be charged at both ends of the transfer

-   DynamoDB supports concepts of streams where any modification to existing record in the table is written out on a data stream which can be processed by compute capabilities like AWS Lambda. Lambda can then take decisions based on that event stream or send a SNS notification instead

-   DynamoDB streams can also be configured to send out two copies of state (previous / current) with the primary key attribute to reflect on the actual change that has happened on the table data

-   DynamoDB supports DAX, to cache responses and improve time from milliseconds to microseconds

Redshift
--------

-   1/10th of the cost of other data warehousing solutions

-   Helps with OLAP requirement --- to derive analytics out of data

-   Automated backups are by default done every day

-   Maximum retention period like RDS is 35 days

-   Leader node hours are not charged, only compute node hours are charged

-   Redhisft can currently run only in OneAZ -> For same reason, Redshift offers asynchronous backup replication in S3 to another region for Disaster Recovery (DR)

-   It is used for business intelligence use-cases

-   Cross region replication can be set up

-   Redshift additionally supports VPC Routing feature, where all COPY and UNLOAD requests between your cluster and data repositories are routed through VPC, thus gathering benefits of Security Groups, NACL, VPC Endpoints etc.

-   If enhanced VPC routing is not enabled, REDSHIFT cluster routes all traffic through internet

-   Redshift Spectrum allows to execute queries on files which are directly stored on S3

AWS Aurora
----------

-   Compatible version of MySQL/PostgreSQL that AWS built from scratch

-   By default stores 2 copies of data in each Availability Zone, with a minimum of 3 availability zones (6 copies of data are hence stored at the minimum)

-   Compute resources can scale upto 32 vCPU cores and 244GB of RAM

-   Starts with 10GB of storage but scales up to 64TB automatically based on requirement, while other databases can grow max till 16TB

-   Aurora can automatically handle loss of 2 copies of data without affecting write capability and 3 copies of data without affecting read availability

-   Storage for Aurora is self healing -> Data blocks are continuously checked for errors and fixed

-   Aurora automated backups or snapshots does not affect performance of running clusters

-   Aurora Snapshots can be shared with other AWS accounts

-   Aurora read replicas can be of two types:- MySQL Read Replicas (Maximum 5) and Aurora Read Replicas (maximum 15)

-   Automated failover is supported for Aurora read replicas but not for MySQL read replicas

-   When you create an Aurora Read Replica from a MySQL RDS instance, AWS basically creates a new Aurora DB Cluster(with read/write capability) which is asynchronously synced with the main DB instance.

-   Two types of endpoints are supported:

i) Reader Endpoint : Load balances traffic across all read replicas

ii) Cluster Endpoint : Routes write queries to active master

ElastiCache
-----------

-   In memory cache store for speeding up an application so that data fetch queries can be reduced

-   For REDIS AUT, user needs to enable in-transit encryption

-   Two types of engines are available:

i) Memcached

Multithreaded, NOT multi-az and useful for simple cache offloading

ii) Redis

Single threaded, MultiAZ, backups are possible, business use-cases available like MIN, MAX, AVG etc.

Cloudfront
==========

-   Cloudfront is a service that is used to store cached content at edge locations so that global users get it from their nearest location
-   Cloudfront can monitor a S3 bucket, EBS load balancer, EC2 machine etc. and then it can just get new data and store it locally so that users can then download it directly from the Cloudfront distribution URL
-   We can also invalidate cached objects in Cloudfront, but these invalidations are normally charged by AWS
-   Objects are stored/cached by the edge locations until the TTL expires; after which a new version is then fetched again from the central server
-   Edge locations can also be used to write data and not just READ data. Write scenario is used mostly in case of S3 Transfer acceleration where an user writes data to a local edge location and then AWS takes care of actually transferring the data to real S3 bucket
-   Origins are the source from which cloudfront gets the data (S3, ELB, EC2 instance etc.)

* * * * *

Elastic Load Balancers
======================

ELBs by default come up in background in all AZs and they also dynamically scale up and down based on the traffic

Full DNS lookup will often tell us about all the ELBs that are currently used by AWS to handle incoming requests

Load balancers are basically of three types:

Application Load balancers
--------------------------

Can route traffic based on layer 7 interaction. Basically work on HTTP/HTTPS layer and can be used for intelligent routing based on application needs (headers, query parameters, source IP etc.)

Network Load Balancers
----------------------

Are used for scenarios where pretty heavy workload (millions of requests) have to be routed/managed

Classic Load Balancers
----------------------

They are deprecated now but were used for basic HTTP/TCP routing

-   AWS never gives us IP addresses of Load Balancer resources, instead we only get a DNS routable name and the IP address can always change. However, for network load balancer we can attach elastic IP addresses to them and the NLB will be available over those addresses.
-   Application load balancers route traffic to a target group instead of routing them to an instance
-   Sticky Sessions have to be disabled, if all requests are going to only few/one instances. They are only useful if the instances store some information locally which is relevant for subsequent requests
-   Cross zone load balancing allows load balancer to route requests evenly to instances in multiple availability zones
-   Path patterns can be used to redirect traffic to different EC2 instances based on a specific URL Path pattern which exists
-   Single Application Load Balancer can be loaded with certificates from different domains. Multiple certificates can be uploaded to ACM (Amazon Certificate Manager) and then clients (reaching out to ALB) can use something like SNI (Server Name Indication) to specify which host to reach

Launch configurations and ASGs
------------------------------

If ASG is terminated, all instances associated as part of it will also be terminated

Launch Configurations are more about the configurations of the individual EC2 machines i.e. instance types, security group configurations, root volume configurations, tags etc. whereas Autoscaling Groups use LCs (Launch configurations) to spin up new instances and work on scaling up/down EC2 instances based on pre-defined policies

Egress Only Internet Gateways
-----------------------------

Egress only gateways allow IPv6 based internet traffic to access the internet and at the same time denying access from internet to the instances within the VPC

* * * * *

Amazon FSx
==========

Amazon FSx is a file system offering from AWS. It is offered in two variants:

-   FSx for Windows
-   FSx for Lustre (High performance compute)

FSx is basically a high performance file system that can be used for compute intensive workloads offering high data throughput. Users can additionally configure the throughput irrespective of the data storage size of the file system (unlike EFS)

FSx is frequently used as file storage for Windows systems as it offers SMB protocol support. Additionally, it also offers integrations with other storage services like S3, where data can be temporarily copied from S3 to AWS FSx for high throughput needs from a filesystem perspective; and later the result can be copied back to S3 after the computations are completed.

Payment model is pay-as-you-go

* * * * *

AWS WAF (Web App Firewall)
==========================

AWS WAF is a managed service designed to protect public facing web applications from unintended/unsafe traffic

WAF provides readymade integrations with:

-   Cloudfront
-   Application Load Balancer
-   API Gateway

With these integrations, whenever any of these services receive a request; they forward it to WAF for validation. If WAF allows, only then these requests are further routed by CF, ALB or API GW to the back-end machine which needs to process the request

WAF offers many managed rules (based on industry best practices like OWASP top 10 vulnerabilities, SQL injection etc.)

As a customer, we can define our custom conditions or use these managed rules to provide security for our application

Custom rules for throttling (IP '123.x.x.x' can only trigger 4000 requests per second etc.) can also be defined at WAF layer and then custom error messages/pages could also be configured in services like Cloudfront which could then be returned to the end-user. All this happens without affecting the real back-end systems.

S3
==

-   S3 Standard : Availability --- 4 9s (99.99%), Durability --- 11 9s (99.999999999 %)
-   Global namespace (Bucket names should be unique)
-   Minimum size of objects is 0 bytes, maximum is 5TB
-   Files can be uploaded in buckets which are nothing but folders hostingd the files
-   On successful upload, S3 returns a HTTP 200 status code
-   By default all buckets are private

S3 Consistency Model
--------------------

Read after write consistency for new PUT objects (Newly uploaded objects are guaranteed to be read immediately without any stale state or problems)

Eventual consistency for overwrite PUTs and DELETEs (Modifications / deletions will eventually reflect latest state --- there could be a delay of some seconds)

S3 Object Properties
--------------------

-   *Key* : Name of the object
-   *Value* : Sequence of bytes/contents of the object
-   *Versioning* : Version of the object
-   *Metadata* : Objects can further have metadata (data about data) to classify the contents

S3 Storage Tiers
----------------

S3 offers various storage tiers that help control cost, availability and durability of the data

-   S3 Standard : This is the most costly alternative with 99.99% availability (probability that the object will be available when needed) and 99.999999999% durability (probability that data will not be lost)
-   S3 IA (Infrequently accessed) : This still replicates data to different zones in a region but less costlier than standard. Retrieval fees are charged
-   S3 IA --- 1 Zone : Multiple zones replication is not done and costs even lesser than S3 IA
-   S3 Intelligent Tiering : Storage tiering is managed by AWS instead which uses Machine learning to decide on which tier to use for the bucket based on historical patterns of usage
-   S3 Glacier : Used when cost has to be less but time of retrieval can be configurable ranging from minutes to hours
-   S3 Glacier deep archive : This is cheapest option where retrieval time of more than 12 hours is acceptable. This is used for data which might be rarely needed and time of retrieval being around ~12 hours is still acceptable

S3 Security Policies
--------------------

-   Access to S3 object/bucket can be controlled with ACL control lists or Bucket Policies
-   Bucket policies work at bucket level BUT Access Control Lists can go all the way down to individual objects
-   Access logging can be configured for S3 buckets which logs all access requests for S3, made by different users
-   Encryption in transit is achieved by HTTPS (SSL / TLS)

Encryption at rest is achieved in two ways

Service Side encryption (Can be further managed by AWS in three ways)

*i) Keys managed by S3 service for encryption (**SSE-S3**)*

*ii) Keys provisioned by user in KMS (**SSE-KMS**)*

*iii) User/Customer provided encryption keys can also be used (**SSE-C**)*

Client Side encryption --- Client himself manages the encryption/decryption and uploads the encrypted data only

S3 Versioning
-------------

-   S3 versioning can be used to maintain multiple copies of objects
-   Once enabled, it cannot be disabled on the same bucket

S3 Transfer Acceleration
------------------------

This is used to speed up large data uploads to S3. With this, user can upload the data to nearest edge location and S3 will then ensure that the data is replicated to the actual bucket for final storage. For Edge Location -> S3, AWS will then use the backbone network which is quite fast than the usual internet speed

S3 Lifecycle Rules
------------------

-   LRs can be used to automatically expire objects based on prefix/tag filters (For eg. All objects having tags "*abc"* should expire after 30 days)
-   Objects can automatically transition across different storage tiers' based on lifecycle rules. For eg. after 30 days migrate objects to IA-1Zone and after 60 days move it to Glacier and finally expire them after 120 days

S3 Cross Region Replication
---------------------------

-   This is used when you want to replicate the contents of a bucket automatically to another bucket (The destination bucket could also enforce different storage tiers on the objects)
-   Versioning has to be mandatorily enabled on both source and destination buckets
-   Delete markers OR object deletions are not replicated on the destination bucket. This is done intentionally by Amazon to inadvertently replicating deletion of objects

Other S3 Related Services Offered by AWS
----------------------------------------

-   S3 Athena --- S3 Select based query analysis on S3 objects without transferring the data first to a data lake
-   S3 Object Lock
-   S3 Inventory --- Reporting for auditing purposes of all S3 objects. Reports can be stored in json, yaml or parquet format
-   S3 Batch Operations : Run lambda function or other operations on millions of objects in one go

* * * * *

Storage Gateway
===============

Virtual / Physical appliance that sits in your data centre and replicates data to S3

*File Gateway* : Plain files, replicated to S3

*Volume Gateway* : There are two types of Volume Gateways

-   Stored Volumes : Entire data is on-site but backed up on S3 asynchronously
-   Cached Volumes : Entire data is on S3, but cached data (frequently accessed) is on-site

*Gateway Virtual Tape library*

* * * * *

SQS (Simple Queue Service)
==========================

-   Messages retained for a period of 14 days
-   Messages can arrive out of order however highly unlikely
-   Long polling can be used to avoid cost of frequent polling. Long polling only returns when a message is available and you avoid polling empty queue time and again
-   Visibility of the message can be altered while it is being processed upto a maximum of 12 hours. During this time, a message if processed completely by a worker, can be removed from the queue

SQS Queue Types
---------------

*Standard SQS Queue : *This is the standard processing model for SQS service

*FIFO SQS Queue : *In this messages are delivered only once and also arrive in order. Maximum throughput of 300 transactions is supported

* * * * *

SWF (Simple Workflow Service)
=============================

This makes more sense when a manual intervention or task oriented workflow is needed in contrast to a message oriented workflow with SQS

-   In contrast SQS is more of a message oriented workflow
-   In SWF, maximum retention period is for 1 year where the messages can be stored
-   This workflow guarantees that a task is processed only once

It works with the following components

i) *Workflow Starters* : Something like web application which triggers a workflow

ii) *Deciders* : Which decide that a particular workflow task has to be executed

iii) *Activity Executors* : They execute the real business logic defined in the workflow

* * * * *

SNS (Simple Notification Service)
=================================

-   This is push based service in contrast to SQS which is pull based
-   One can manage various different topics of notification and different subscribers who can then register with the topic with different subscriptions (SMS, Email, HTTPS etc.)
-   In order to ensure that updates are not lost, SNS messages are replicated across all AZs
-   It is immediate notification service with no delays

* * * * *

Elastic Transcoder
==================

-   Media transcoding service used to convert media files across different formats and resolutions
-   Supports various templates with best practices for converting media for iphones, android devices, browsers etc.
-   High resolution transcoding service is pay as you go model --- cost depends on resolution and time taken to convert

* * * * *

API Gateway
===========

API Gateway is an entry-point for various types of resources acting as a front door entry mechanism with support for:

-   HTTPS/TLS
-   Invocation for resources like EC2, Lambda etc.
-   Caching response at it's own layer, thereby reducing requests to things like Lambda on every API invocation

API Gateway uses the following things to realise an API that can be exposed to the end-user

-   A container which defines the API to be exposed
-   Request types that are supported for the API container (GET, POST, OPTIONS etc.)
-   URL Paths that have to be supported as part of the API (/main, /help, /register etc.)
-   Destinations like Lambda, EC2 instances etc. which receive the request
-   CORS (Cross origin resource sharing is needed to be enabled if requests can originate from various different sources) --- FYI, CORS is always enforced by the client (like browsers)

API Gateway supports throttling API requests on global or API level and also supports caching by defining a fixed data size for storage to be provisioned. With caching enabled you can then avoid passing on redundant calls to the backend systems

* * * * *

Kinesis (Streaming Data Ingestion)
==================================

*Streams --- Analytics --- Firehose*

-   When there is a need to consume lots of streaming data on the fly, Kinesis platform can be used

Kinesis offers three different types of services:

-   *Kinesis Streams* : They work on shards (Shards are containers which define the logical boundaries of data storage). Streams persist the data for minimum 24 hours and maximum 7 days, so that something like Lambda or EC2 can work on this data and understand it
-   *Kinesis Firehose* : This is without persistence --- As soon as data comes in, it has to be read/understood/processed by something like Lambda / EC2 and later the result can be stored in DynamoDB, RDS, S3 or Elastic Cluster etc.
-   *Kinesis Analytics* : This is used for real time analytics over the data that is pushed to the Kinesis Platform

* * * * *

AWS Cognito and Web Identity Federation
=======================================

AWS Cognito builds upon two concepts:

-   *User pools* --- They handle user registration, authentication, password reset etc.
-   *Identity Pools* --- They are more aligned with authorization as they return the AWS credentials which can be temporarily used to assume a role, using which the user can then access various AWS resources

Web Identity Federations
------------------------

When you build a mobile app for example, you cannot distribute AWS credentials along with the application code. When the application needs to access any AWS resource, it can instead generate a temporary AWS token which maps to a particular role and using that temporary token it accesses the specified resource. This avoids bundling any secure credential directly with the source code. For fetching an auth token, the app first authenticates the user against Google, Facebook, Amazon etc. or any other provider which support OIDC (Open ID Connect) connect capability.

Mobile App User -> Logs in to Amazon, Facebook, Microsoft etc. -> Authenticates -> Mobile app gets a secure token and exchanges it with AWS for a temporary access token mapped to a role

-   While Web Identity Federations don't use something like Active Directory, but instead have integrations with Amazon, Facebook, Google etc. On the flip side, when integrations with on-premise Active Directory are needed, users can instead use SAML based integration with an identity provider like OneLogin which integrates with the on-premise AD solution and generates SAML based authorization which is also understood by different solutions like AWS.
-   So, Web Identity providers are to be used when user logs in against well known services, whereas SAML based assertion is to be used, when user logs in against an IdP and the IdP further validates against AD. One benefit with IdP based SAML is that user can now access all applications using the same IdP based authentication which he had to do only once.

* * * * *

Amazon EMR (Managed Hadoop Framework)
=====================================

-   EMR helps users run Big Data workloads like Hadoop, Apache Spark etc on AWS
-   In Amazon EMR, AWS provisions EC2 instances for you to work on the data. This enables users to access the EC2 instances and gain visibility in Operating System Configuration

* * * * *

Amazon Inspector
================

It helps to monitor and investigate state of security of the systems by scanning networks or configurations

* * * * *

Cloudwatch Agent
================

Custom monitoring scripts written in Perl, Ruby etc. and are available to be installed on the EC2 instances. Same Cloudwatch agent can be used to ship logs as well as additional monitoring data like Memory Utilization etc to Cloudwatch. Metrics like MemoryUtilization, CPU Core usage, Disk space utilization, disk space utilization etc. are not available out of the box with default Cloudwatch capabilities

* * * * *

Amazon MQ
=========

It is a messaging broker with support for large number of protocols and standards, and is usually better when migrating existing messaging broker workloads to the cloud. When building new applications that depend on messaging capabilities, we can always use Amazon SQS which is highly scalable.

Amazon SQS on the other hand is similar but does not support a large number of APIs and protocols.

![](https://miro.medium.com/max/60/0*Piks8Tu6xUYpF4DU?q=20)

# OCIFoundationsAssociate
=======================

**OCI Architecture**

-   OCI Regions --- 21 Available + 15 Planned; Commercial, Govt, Microsoft Azure Interconnect
-   Region --- Localized Geographical area comprised of 1 or more AD
-   Availability Domains --- One or more fault-tolerent, isolated DC located within a region, but connected to each other by low latency, high bandwidth network; Do not share physical infra
-   Fault Domains --- Grouping of hardware and infrastructure with in an AD to provide anti-affinity(logical data center); 3 FD per AD; Do not share SPOHF; change procedures are isolated at FD
-   One AD Regions --- within one year second AD or region will be made available
-   Choosing Region --- Location, Data Residency & Compliance, Service Availability
-   Avoid SPOF --- Design architecture to deploy instances that perform same tasks in different FD or different AD for multiple AD regions
-   Data Guard --- Data replication across AD
-   HA Design --- FD, AD, Region Pair
-   Compartments --- collection of related resources; helps to isolate and control access to resources; Tenancy/Root compartment; Compartment Network, Compartment Storage etc;
-   Each resource belong to a single compartment; Resource can interact with other resource in diff compartment; Resources and compartments can be added/deleted anytime;
-   Resources can be moved from one to another; Resources from multiple regions can be in the same compartment; Compartments can be nested (6 levels deep); can give group of users access to compartments by writing policies; Analyze cost and assign budget for resources in compartments

**OCI Compute Services**

-   Bare Metal --- Code, App Container, Language Runtime, OS, Virtualization; No virtualization
-   Dedicated Virtual Hosts --- Code, App Container, Language Runtime, OS;
-   Virtual Machines --- Code, App Container, Language Runtime, OS; Guset on a host server with hypervisor based virtualization;
-   Container Engine --- Code, App Container (Docker);
-   Functions --- Code; Consumption based pricing
-   BM --- Direct Hardware access; Single Tenant server; Use Case: Performance intensive workloads (DB), workloads not virtualized; workload that require specific Hypervisor, workload requires BYO licensing (SQL, Exchange etc)
-   VM --- Multi-tenant VMs; Use cases: to control all aspects of env, to deploy legacy app running on windows/linux, to move apps from on-premise to OCI
-   Dedicated Virtual Host --- Single-tenant VMs
-   Instance Basics --- various instance sizes(CPU, RAM, Bandwidth); Support both Intel and AMD processors; Provide GPU and HPC instance options(RDMA); instance placed on virtual network with powerful connectivity options; Depends on other OCI services such as Block volume (Boot(OS)/Data) and VCN(Virtual Nic)
-   Vertical Scaling --- Scale up/Down; Downtime required;
-   Autoscaling --- Enable large scale deployment of VM from a single gold image with automatic configuration; Scale out/Scale in; If one VM fails, others will keep working; based on metrics; Running Instance -> Config (Gold Image --- OS image, metadata, shape, vNICs, Storage, subnets) -> Instance Pool (put in diff ADs, Manage all together) -> Scaling Rule
-   How to Deploy containers?\
    1\. Manually SSH into machines and run Docker\
    2\. Scripting or Config mgmt tools\
    3\. Orchestration Systems
-   Oracle Kubernetes Engine --- K8S; Containers in Pods, Pods in Node (Instances); OKE and OCIR
-   Functions --- small but powerful blocks of code that generally do one simple thing; stores as Docker image; invoked in response to a CLI command or signed HTTP request Push container to Registry -> Configure Function Trigger -> Code runs only when triggered -> Pay for code execution time only; based on FN project

**OCI Storage Services**

-   Block Volume, Local NVMe, File Storage, Object Storage, Archive Storage
-   Storage Requirements --- Persistent vs Non-persistent, What type of data?(Database, videos, audio, photos, text), Performance (max capacity, IOPS, throughput), Durability (Copies of data), Connectivity (Local vs network, How does apps access the data), Protocol (Block vs File vs HTTPs)
-   Block Storage --- Hard drive in a server(on a remote chassis); stored on device in fixed sized blocks (512 bytes); Access by OS as mounted drive volume; Storage for compute services; 2 types (Boot Volume/OS Disk, Block Volume/Data Disks) Use cases --- Databases, Exchange, VMWare, Server Boot. Block volume stores replica of data in 3 separate FDs; No need to configure s/w based protection(RAID-10 etc); Periodic backups\
    (automated schedule backups);
-   Block volume backup --- Complete point-in-time snapshot copy of block volumes; Encrypted and stored in Object Storage and can be restored as new volumes to any AD within same region; Can copy block volume backups from one-region to another(X-Region Backup); Backups can be scheduled
-   Block Volume Tiers --- (50 GB --- 32 TB, up to 32 volumes/instance (32x32=1PB); Data encrypted at rest and in-transit(oracle managed/customer managed key)\
    -   Basic(2 IOPS/GB, 240 KB/s/GB Throughput); throughput intensive workloads with large sequential I/O such as big data & streaming, log processing and data warehouses.\
    -   Balanced (60 IOPS/GB, 480 KB/s/GB); most workloads that perform random I/O such as boot disks\
    -   High Performance (75 IOPS/GB, 600 KB/s/GB); workload require best possible performance including large DB's
-   Local NVMe --- temp storage, locally attached to compute instance; app require high performance local storage; Use case --- NoSQL DB, In-memory DB, Scale-out txn DB, Data warehousing. Storage non-persistent but survives reboot. OCI uses NVMe(Non-Volatile Memory Express) interface for very high performance. OCI provides no RAID, snapshots, backup capabilities
-   File Storage --- Hierarchical collection of docs organized into named directories which are themselves structured files. Distributed file systems make distributed look exactly like local file systems. Distributed file standards --- NFS and SMB (provide access over networks). FSS --- supports NFS v.3; Data protection: Snapshots(10000 per file system); Security (Data at rest, in-transit encryption). Use cases: Oracle Apps, HPC, Big Data and Analytics, General purpose file systems. FS --- replicates data in 3 FDs; can take snapshot and restore snapshot
-   Object Storage --- All data, managed as objects; Each object stored in a bucket, relies on standard HTTP verbs; flat structure; OSS --- An internet-scale, high performance storage platform; ideal for unstructured data;\
    regional service; storage classes (hot/cold); Use cases: content repo for data, images, logs & video etc; Archive/Backup, Storing log data for analysis; Storing large datasets; Big Data/Hadoop storage\
    OS replicates in 3 FDs; stores replica of data in more than AD
-   OS Tiers --- Standard Storage Tier(Hot) Fast, immediate, and frequent access; Data retrieval in instances; Always serves the most recent copy of the data when retrieved; Standard buckets can't be downgraded to archive storage. Archive Storage Tier (Cold) Seldom or rarely accessed data but must be retained and preserved for long periods of time; 10x cheaper than standard tier ($0.0026 vs $0.0255 Gb/month);\
    90 days min retention period; objects needs to be restored before download; TTFB after restore request is made: 4 hours; Archive bucket can't be upgraded to Standard

**OCI Network Services**

-   Virtual Cloud Network --- software defined private network that you setup in OCI; Enable OCI resources to communicate
-   VCN address space --- Address space 10.0.0.0/16; Every resource will get its own unique private IP address; subnet --- divide VCN into one or more sub networks;
-   Gateways --- IGW; Public Subnet(DMZ);
-   NAT Gateway (Blocks inbound connection)
-   DRG --- virtual router that provides a path for private traffic between your VCN and destinations other than the internet; DRG to establish a connection with on-premises network via IPsec VPN, FastConnect(private, dedicated connectivity)
-   Service Gateway --- Communication to public OCI services --- access without using internet
-   Peering --- process of connecting multiple VCN; Local VCN peering (same region); Remote VCN peering (Different Region) No transitive peering\
    VCN Security --- Firewall rules (Subnet layer); Network Security Group (VNIC layer)
-   Load Balancer --- sits between client and backends; performs tasks such as: Service Discovery, Health Check, Algorithm. LB Benefits --- Fault tolerance and HA; Scale; Naming abstraction. LB Types --- Public LB, LB pair for HA

**OCI IAM**

-   IAM --- Identities, Permissions
-   Principals --- IAM entity that is allowed to interact with OCI resources; IAM users and Instance Principals
-   IAM Users and Groups --- 1st IAM user is default admin; Users -> Groups -> at least one policy
-   Instance Principals --- let instances make API calls against other OCI services
-   Network Admin, Storage Admin etc --- Policies
-   Authentication --- deals with user identity; Username/Password, API Signing key, Auth Tokens
-   Authorization --- actions performed by principals; Policies; Allow group <> to <> resource-type in tenancy/compartment where conditions <>\
    Policies --- Allow to in where\
    Verb --- inspect(list resources), read(inspect_user-specified metadata), use (Read+Update), manage(all permissions)\
    Resource type --- all-resources, database-family, instance-family, object-family etc
-   Common Policies --- Network admin(manage virtual-network-family), Instance Launchers(manage instance-family, use volume-family, use virtual-network-family)

**OCI Database Services**

-   OCI DB Options --- VM (Fast Provisioning), Bare metal(Fast performance), RAC (Managed HA), Exadata DB systems(Managed Exadata Infra), Autonomous --- Shared/Dedicated(Self-driving, Self-Securing, Self-Repairing)
-   DB Systems --- Managed DB systems, Complete Lifecycle automation (Provisioning, Patching, Backup & Restore), HA and DR (RAC & Data Guard), Scalability (Dynamic CPU and Storage Scaling), Security (Infra(IAM, VCN, Audit), Database(TDE, Encrypted RMAN backup/Block volume encryption)), BYOL
-   DB Systems Operations --- Launch, start, stop or reboot DB systems(Billing continues in stop state for BM DB systems), Scale (CPU cores (BM DB), Storage(VM DB)), Patching (2 step process, For Exadata and RAC patches are rolling)
-   DB Systems Backup --- Manual/Automatic Backups, Auto backups written to Oracle owned Object storage buckets, Runs between midnight --- 6 AM in DB system time zone, Preset retention periods: 7, 15, 30, 45 and 60 Days; Recover DB from backup stored in Object storage(Last known good state, Timestamp specified, Using SCN)
-   DB Systems HA and DR --- Oracle Data Guard --- survive disasters and data corruptions (maintain sync between primary and standby DB); Active Data Guard (adv features for data protection and availability, included in Extreme Performance edition and Exadata service); 2 modes --- switchover(planned migration, no data loss), Failover (unplanned, min data loss)
-   MAA --- Primary and standby DB can be either a single-instance oracle db or RAC db
-   Autonomous Databases --- Fully managed DB with 2 workload types; TP, DWH; Deployment options --- Dedicated/Shared; Automates backing up DB, patching w/o downtime, Upgrade DB, Tune DB

**OCI Security**

-   Shared Security Model --- OCI upto virtualization; Customer (Patching app and OS, OS config, IAM, Network security, Endpoint protection, Data Classification and Compliance)
-   Security Services --- OCI IAM, MFA, Federation, Storage and DB services, Data Safe, Key Management, OS Management Service, Bare Metal, Dedicated VM hosts, VCN, NSG, SL, WAF
-   IAM --- RBAC; Authentication -> OCI IAM -> Authorization -> Compartments -> Resources; MFA; SSO using IDP
-   Data Protection --- Block volume (Data enc at-rest/in-transit, BYOK) File Storage (Data enc at-rest/in-transit, BYOK) Object Storage (Data enc at-rest, BYOK, Private Buckets, Pre authenticated requests) Database(TDE, Data safe, Data Vault) Key Management (BYOK, use HSM)
-   OS & Workload isolation --- OS Mgmt service configured by default for Oracle Linux; Network protection --- Tiered subnet strategy for VCN, Gateways, Security Lists, NSG, OCI WAF (XSS, SQL Injection), Protection against layer 7

**OCI Pricing and Billing**

-   Pricing Models --- Pay as you go; Monthly Flex (Universal Credits) $1000 monthly charge/12 months -> 33% --- 60% savings vs PAYG; BYOL (apply on-premise Oracle license); All OCI region have same pricing;
-   Block volume (Storage cost $0.0255 per GB/month, Performance Cost (VPU/GB) --- NA for Basic, 10 VPU at $0.0017 for balanced, 20 VPU at $0.0034 for higher performance); Data Transfer costs --- Ingress/Egress free b/w data transfers, Egress charge for different regions; To and from internet (Egress charged), DRG/FastConnect both Ingress/Egress free
-   Pricing Example --- Outbound Data Transfer 10 TB free
-   Billing --- Cost Tracking Tags, Cost Analysis, Budgets, Alert every 15 mins, Usage reports (automatically generated CSV file, 24 hrs data, retained for 1 year)
-   Free Tier --- $300 free credit for 30 days; upto 8 instances, 5TB storage
-   Always Free --- 2 Oracle Autonomous DB, 2 OCI Compute VMS, Block, Object and Archive Storage, LB and Data egress, Monitoring and Notifications

