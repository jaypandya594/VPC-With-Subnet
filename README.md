# VPC-With-Subnet

# Internship Task: VPC and Subnet Creation

## 📄 Overview

**Objective:** The goal of this task is to design and implement a fundamental cloud network infrastructure. You will create a Virtual Private Cloud (VPC) with one public subnet (for internet-facing resources) and one private subnet (for secure, internal resources), configure internet access correctly, and document the setup.

---

## 📋 Prerequisites

* An active **AWS**  account (Free Tier is sufficient).

---

## 🧭 Detailed Mini Guide (Step-by-Step)

This guide primarily uses AWS terminology, but the concepts are transferable to GCP.

### ⿡ Login to Cloud Console

1.  Log in to your AWS Management Console.
2.  In the search bar, type **"VPC"** and navigate to the VPC Dashboard.
3.  Ensure you are in your desired AWS Region (e.g., `us-east-1`).

### ⿢ Create a Virtual Private Cloud (VPC)

1.  From the VPC Dashboard, click **"Create VPC"**.
2.  Select **"VPC only"**.
3.  **Name tag:** `MY-vpc-01`
4.  **IPv4 CIDR block:** `10.0.0.0/16`
    * *(This defines your main private cloud network, giving you 65,536 private IP addresses.)*
5.  Click **"Create VPC"**.

### ⿣ Create Subnets

Now, you'll create two subnets within your `My-vpc-01`.

1.  Navigate to **"Subnets"** in the left-hand menu.
2.  Click **"Create subnet"**.

#### Public Subnet
* **VPC ID:** Select your `My-vpc-01-vpc`.
* **Subnet name:** `My-vpc-Pubsub-1`
* **Availability Zone:** Choose any one (e.g., `us-east-1a`).
* **IPv4 CIDR block:** `10.0.1.0/24`
* Click **"Create subnet"**.

#### Private Subnet
* Click **"Create subnet"** again.
* **VPC ID:** Select your `My-vpc-01`.
* **Subnet name:** `My-vpc-Prisub-1`
* **Availability Zone:** Choose any one (can be the same as the public subnet, e.g., `us-east-1a`).
* **IPv4 CIDR block:** `10.0.2.0/24`
* Click **"Create subnet"**.

#### Enable Public IP Auto-Assignment
1.  Go back to the **"Subnets"** list.
2.  Select your `My-vpc-Pubsub-1`.
3.  Click **"Actions"** -> **"Edit subnet settings"**.
4.  Check the box for **"Enable auto-assign public IPv4 address"**.
5.  Click **"Save"**.
    * *(The `My-vpc-Prisub-1` should have this **disabled** by default.)*

### ⿤ Create and Attach an Internet Gateway (IGW)

This allows your public subnet to communicate with the internet.

1.  Navigate to **"Internet Gateways"** in the left-hand menu.
2.  Click **"Create internet gateway"**.
3.  **Name tag:** `My-vpc-01-IGW`
4.  Click **"Create internet gateway"**.
5.  Select the new `My-vpc-01-IGW` from the list. Click **"Actions"** -> **"Attach to VPC"**.
6.  Select your `My-vpc-01-IGW` and click **"Attach internet gateway"**.

### ⿥ Route Table Configuration

This step directs traffic from your subnets.

#### Create a Public Route Table
1.  Navigate to **"Route Tables"** in the left-hand menu.
2.  Click **"Create route table"**.
3.  **Name tag:** `My-vpc-01-Pub-RT`
4.  **VPC:** Select your `MY-vpc-01`.
5.  Click **"Create route table"**.
6.  Select your new `My-vpc-01-Pub-RT` from the list.
7.  Go to the **"Routes"** tab below and click **"Edit routes"**.
8.  Click **"Add route"**.
    * **Destination:** `0.0.0.0/0` (This means "all internet traffic")
    * **Target:** Select **"Internet Gateway"** and choose your `My-vpc-01-IGW`.
9.  Click **"Save changes"**.
10. Now, go to the **"Subnet associations"** tab for your `My-vpc-01-Pub-RT`.
11. Click **"Edit subnet associations"**.
12. Check the box next to your `My-vpc-Pubsub-1` and click **"Save associations"**.

#### Configure the Private Route Table
1.  Go back to the **"Route Tables"** list.
2.  You will see a "Main" route table already associated with your `My-vpc-01`.
3.  **Name tag:** `My-vpc-pri-RT` (Rename the main route table for clarity).
4.  Select your `My-vpc-pri-RT`. Go to the **"Subnet associations"** tab.
5.  It should already be associated with your `My-vpc-Pubsub-1` (as it's the main table). If not, edit the associations to add the `private-subnet`.
6.  Go to the **"Routes"** tab. **Do not** add a `0.0.0.0/0` route to the Internet Gateway. It should only have a "local" route. This keeps it isolated from the internet.

---

