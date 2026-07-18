---
title : "Create VPC & Subnets"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.3.1. </b> "
---

### Create Amazon VPC & Subnets

In this section, we will configure an **Amazon VPC** virtual network and divide it into **4 Subnets** (2 Public and 2 Private) across 2 different Availability Zones to increase redundancy.

---

#### Steps:

1. Open the [Amazon VPC console](https://us-east-1.console.aws.amazon.com/vpc/home?region=us-east-1#Home:).
2. In the left navigation pane, select **Your VPCs**, then click **Create VPC**:

![Create VPC](/images/5-Workshop/5.3-Network-Security/vpc_create_btn.png)

3. In the **Create VPC** configuration page:
   * Select **VPC only**.
   * **Name tag**: Enter ```ticket-app-vpc```.
   * **IPv4 CIDR block**: Enter ```10.0.0.0/16```.
   * Keep the other default settings and click **Create VPC**.

![VPC Config Details](/images/5-Workshop/5.3-Network-Security/vpc_create_details.png)
![VPC Config Tags and Create](/images/5-Workshop/5.3-Network-Security/vpc_create_tags_btn.png)

4. In the left navigation pane, select **Subnets**, then click **Create subnet**:

![Create Subnet Button](/images/5-Workshop/5.3-Network-Security/subnet_create_btn.png)

   * **VPC ID**: Select the ```ticket-app-vpc``` VPC that was just created.
   * Add the following **4 Subnets** by clicking **Add new subnet**:

     ![Subnet VPC Name](/images/5-Workshop/5.3-Network-Security/subnet_create_vpc_name.png)
     ![Subnet AZ and CIDR](/images/5-Workshop/5.3-Network-Security/subnet_create_az_cidr.png)
     
     * **Subnet 1 (Public Subnet A)**:
       * **Subnet name**: ```ticket-app-subnet-public-a```
       * **Availability Zone**: Select ```us-east-1a```.
       * **IPv4 CIDR block**: ```10.0.1.0/24```.
     
     * **Subnet 2 (Public Subnet B)**:
       * **Subnet name**: ```ticket-app-subnet-public-b```
       * **Availability Zone**: Select ```us-east-1b```.
       * **IPv4 CIDR block**: ```10.0.2.0/24```.
     
     * **Subnet 3 (Private Subnet A)**:
       * **Subnet name**: ```ticket-app-subnet-private-a```
       * **Availability Zone**: Select ```us-east-1a```.
       * **IPv4 CIDR block**: ```10.0.11.0/24```.
     
     * **Subnet 4 (Private Subnet B)**:
       * **Subnet name**: ```ticket-app-subnet-private-b```
       * **Availability Zone**: Select ```us-east-1b```.
       * **IPv4 CIDR block**: ```10.0.12.0/24```.

5. Click **Create subnet** to complete the setup.

![Subnets Created](/images/5-Workshop/5.3-Network-Security/subnets_created_list.png)
