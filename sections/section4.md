# EC2 101 - Part 1

Amazon Elastic Compute Cloud (**Amazon EC2**) is a web service that provides resizable compute capacity in the cloud. Amazon **EC2** reduces the time required to obtain and boot new server instances to minutes, allowing you to quickly scale capacity, both up and down, as your computing requirements change.

- **EC2 Options**
    - **On Demand**: Allow you to pay a fixed rate by the our (or by the second) with no commitment.
        - Users that want the low cost and flexibility of Amazon EC2 without any up-front payment or long-term commitment;
        - Applications with short term, spiky, or unpredictable workloads that cannot be interrupted;
        - Applications being developed or tested on Amazon EC2 for the first time.


    - **Reserved**: Provide you with capacity reservation, and offer a significant discount on the hourly charge for an instance. 1 Year or 3 Year terms.
        -  Applications with steady state or predictable usage;
        -  Applications that require reserved capacity;
        -  Users able to make upfront payments to reduce their total computing costs even further;

    - **Spot**: Enable you to bid whatever price you want for instance capacity, providing for even greater saving if your applications have flexible start and end times.
        - Applications that have flexible start and end times;
        - Applications that are only feasible at very low compute prices;

    - **Dedicated Hosts**: Physical EC2 server dedicated for your use. Dedicated hosts can help you reduce costs by allowing you to use your existing server-bound software licences (Ex: VMware, oracle, etc..). 
        - Useful for regulatory requirements that may not support multi-tenant virtualization;
        - Great for licensing which does not support multi-tenancy or cloud deployments;
        - Can be purchased On-Demand (hourly);
        - Can be purchased as a Reservation for up to 70% off the On-Demand Price

    ![alt](/images/ec2types.jpg)

    ![alt](/images/ec2type2.jpg)

---
# EC2 101 - Part 2

**What is EBS?**

Amazon **EBS** allows you to create storage volumes and attach them to Amazon **ECS instances**. Once attached, you can create a file system on top of these volumes, run a database, or use them in any other way you would use a block device. Amazon EBS are placed in a specific Availability Zone, where they are automatically replicated to protect you from the failure of a single component.

**EBS Volume Types**
- **General purpose SSD (GP2)**
    -  General purpose, balances both price and performance;
    -  Ratio of 3 IOPS per GB with up 10,000 IOPS and the ability to burst up to 3000 IOPS for extended periods of time for volumes at 3334 GiB and above;

- **Provisioned IOPS SSD (IO1)**
    - Designed for I/O intensive applications such as large relational or NoSQL databases;
    - Use if you need more than 10,000 IOPS;
    - Can provision up to 20,000 IOPS per volume.

- **Throughput Optimized HDD (ST1)**
    - Big Data
    - Data warehouses
    - Log Processing
    - **Cannot be a boot volume**

- **Cold HDD (SC1)**
    - Lowst cost storage for infrequently accessed workloads
    - File Server
    - **Cannot be a boot volume**

- **Magnetic (Standard)**
    - Lowest cost per gigabyte of all **EBS Volume** types that is bootable. Magnetic volumes are ideal for workloads where data is accessed infrequently, and applications where the lowest storage cost is important.


--- 

**Exam tips EC2**
- Know differences between (scenario questions):
    - On Demand
    - Spot
    - Reserved
    - Decicated Hosts
- Remember with **spot instances:**
    - If you terminate the instance, you pay for the hour
    - If AWS terminates the spot instance, you get the hour it was terminated in for free.


- **EBS** consists of:
    - SSD, General Purporse - GP2 - (Up to **10,000** IOPS)
    - SSD, Provisioned IOPS - IO1 - (More than **10,000** IOPS)
    - HDD, Throughput Optimized - ST1 - Frequently accessed workloads
    - HDD, Cold - SC1 - Less frequently accessed data
    - HDD, Magnetic - Standard - Cheap, infrequently accessed storage

    - You **cannot** mount 1 EBS volume to multiple EC2 instances, instead use **EFS**


**EC2 Instance Types**


   ![alt](/images/ec2type2.jpg)



--- 

**EC2 Labs Summary**
- Termination Protection is turned off by default, you must turn it on.

- On an EBS-backed instance, the default action is for the root EBS volume to be deleted when the instance is terminated.
- EBS Root Volumes of your **DEFAULT AMI's** cannot be encrypted. You can also use a third party tool (such as bit lockec, etc..) to encrypt the root volume, or this can be done when creating AMI's (lab to follow) in AWS console or using the API.
- Additional volumes can be encrypted.


---

# Security Groups

Security is basically a **virtual firewall** and it's controlling traffic to your instances.

- All inbound traffic is blocked by default;
- All Outbound traffic is allowed;
- All changes made on security ;
- groups have effect **immediatelly**.

- You can have any number of **EC2 instances** within a security group;
- You can have multiple security groups attached to EC2 Instances; 

- Security groups are **stateful**. Just by adding a inbound rule it automatically adds a outbound rule.

- You cannot block IP addresses using Security Groups, instead use **Network Access Control Lists**;

- You can specify allow rules, but not deny rules.
---

# EBS Volumes - LAB

![alt](/images/instancevol.jpg)
- Obviously, volumes (for an instance) should be located at the same region!


- Snapshot indicates machine root volume;
- Volumes can be modified on the fly. (Except magnetic volumes..**Standard**)

![alt](/images/snapshot.jpg)

- To move a EBS volume from a Availability Zone to another, We need create a snapshot of this volume and them create a new volume on the new AZ.
- We can change the volume type during creation..


    ![alt](/images/movevol.jpg)
    ![alt](/images/movevol2.jpg)

- We can use a snapshot to create a new AMI's


## Volumes & Snapshots summary
- Volumes exist on EBS:
    - Virtual Hard Disk

- Snapshots exist on S3.
- Snapshots are point in time copies of volumes;
- Snapshots are incremental - this means only the blocks that have changed since your last snapshot are moved to s3.
- To create a snapshot for Amazon EBS volumes that server as root devices, you should stop the instance before taking the snapshot;
- However you can take a snap while the instance is running;
- You can create AMI's from both Volumes and Snapshots;
- You can change EBS volume sizes on the fly, including changing the size and storage type;
- Volumes will ALWAYS be in the same availability zone as the EC2 instance.
- To move an EC2 volume from one AZ/Region to another, take a snap or an image of it, the copy it to the new AZ/Region

## Volumes vs Snapshots - Security
- Snapshots of encrypted volumes are encrypted automatically.
- Volumes restored from encrypted snapshots are encrypted automatically.
- You can share snapshots, but only if they are unencrypted
    - These snapshots can be shared with other AWS accounts or made public.


--- 

# EFS - LABS
What is EFS ?

Amazon Elastic File System (Amazon EFS) is a file storage service for Amazon Elastic Compute Cloud (Amazon EC2) instances. Amazon EFS is easy to use and provides a simple interface that allows you to create and configure file systems quickly and easily. With Amazon EFS, storage capacity is elastic, growing and shrinking automatically as you add and remove files, so your applications have the storage they need, when they need it.

- Features:
    - Supports the Network File System version 4 (NFSv4) protocol;
    - You only pay for the storage you use (no pre-provisioning required);
    - Can scale up to the petabytes;
    - Can support thousands of concurrent connections;
    - Data is stored across multiple AZ's within a region;
    - Read After Write consistency