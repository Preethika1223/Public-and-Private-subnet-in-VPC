# Public-and-Private-subnet-in-VPC
# EC2 Instance Setup in Public and Private Subnets

This guide provides instructions for creating an EC2 instance in both a public and private subnet within a custom VPC. 
---

## Prerequisites
- An active AWS account.
- Access to the AWS Management Console.
- A key pair for SSH access (create one if not already available).

---

## Steps to Create EC2 Instances in Public and Private Subnets

### 1. Create a VPC
1. Go to the **VPC Dashboard** in the AWS Management Console.
2. Click **Create VPC**.
3. Configure the VPC:
   - **Name tag**: `uniquename_VPC` 
   - **IPv4 CIDR block**: `10.0.0.0/16` (or any valid CIDR range).
4. Click **Create**.

### 2. Create Subnets
1. In the VPC Dashboard, go to **Subnets** and click **Create subnet**.
2. Create a **Public Subnet**:
   - **VPC**: Select the VPC you just created.
   - **Subnet name**: `uniquename_PublicSubnet`.
   - **Availability Zone**: Select an AZ (e.g., `us-east-1a`).
   - **IPv4 CIDR block**: `10.0.1.0/24`.
3. Create a **Private Subnet**:
   - **VPC**: Select the same VPC.
   - **Subnet name**: `uniquename_PrivateSubnet`.
   - **Availability Zone**: Select the same or a different AZ (e.g., `us-east-1b`).
   - **IPv4 CIDR block**: `10.0.2.0/24`.
4. Click **Create** for both subnets.

### 3. Configure Internet Access for Public Subnet
1. Create an **Internet Gateway**:
   - Go to **Internet Gateways** in the VPC Dashboard.
   - Click **Create internet gateway**.
   - Name it `uniquename_IGW`.
   - Attach it to your VPC.
2. Create a **Route Table** for the public subnet:
   - Go to **Route Tables** in the VPC Dashboard.
   - Create a new route table named `uniquename_PublicRT`.
   - Add a route with destination `0.0.0.0/0` and target as the Internet Gateway.
   - Associate the public subnet (`uniquename_PublicSubnet`) with this route table.

### 4. Create Security Groups
1. Go to **Security Groups** in the EC2 Dashboard.
2. Create a new security group:
   - **Name**: `uniquename_SG`.
   - **Description**: Allow SSH and HTTP access.
   - **Inbound Rules**:
     - Allow SSH (port 22) from your IP.
     - Allow HTTP (port 80) from anywhere.
   - **Outbound Rules**: Allow all traffic.

### 5. Launch EC2 Instances
1. Go to the **EC2 Dashboard** and click **Launch Instance**.
2. Configure the instance:
   - **Name**: `uniquename_PublicInstance` (for public subnet) or `uniquename_PrivateInstance` (for private subnet).
   - **AMI**: Select **Amazon Linux 2023 AMI**.
   - **Instance type**: `t2.micro` or `t3.micro`.
   - **Key pair**: Select your key pair (e.g., uniquename_keypair).
3. **Network Settings**:
   - For the public instance:
     - Select the VPC you created.
     - Select the public subnet (`uniquename_PublicSubnet`).
   - For the private instance:
     - Select the same VPC.
     - Select the private subnet (`uniquename_PrivateSubnet`).
4. **Configure Security Group**:
   - Select the security group you created (`uniquename_SG`).
5. **Advanced Details**:
   - For **Purchase option**, select **Request Spot Instances**.
   - Set your maximum price (e.g., the on-demand price).
6. Click **Launch Instance**.

### 6. Verify Instances
1. Go to the **EC2 Dashboard** and check the instances.
2. Ensure the public instance has a public IP and the private instance does not.
---

## Screenshot Instructions
1. After launching the instances, go to the **EC2 Dashboard**.

---

## Notes
- Ensure the naming convention is strictly followed (e.g., `uniquename_VPC`, `uniquename_PublicSubnet`, etc.).
- Use only `t2.micro` or `t3.micro` instance types to avoid unnecessary costs.
- Spot instances may be terminated if the spot price exceeds your maximum bid.

---

## Troubleshooting
- If instances fail to launch:
  - Verify the VPC, subnets, and security group configurations.
  - Ensure the key pair is correct.
  - Check the Spot instance request status.

---
