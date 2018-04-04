# AWS - 10,000 Feet Overview

## <center>The AWS Plataform</center>
![alt](/images/aws_overview.jpg)

## What is a Region  What is an AZ ?
- A region is a geographical area. Each region consists of 2 (or more) Availability zones.
- An Availability Zone (AZ) is simply a Data Center

![alt](/images/aws_region.png)

## <center>What is An Edge Location ?</center>
**Edge locations** are endpoints for AWS which are used for caching content. Typically this consists of CloudFront, Amazon's Content delivery Network (CDN). There are many more Edge Locations than regions. Currently there are over **96** Edge locations.

# **Exam tip**

Understand the difference between a region, an Availability Zone (AZ) and an Edge Location.
- A **region** is a physical location in the world which consists of two or more Availability Zones (AZ's).
- An **AZ** is one or more discrete data centers, each with redundant power, networking and connectivity, housed in separeted facilities.
- **Edge Locations** are endpoints for AWS wich 