# Week 2 – Azure Networking: Task 8

## Task: Set Up a Domain, Configure a Web Server, and Route Traffic Using an Internal DNS Server

## Objective

In this task, I implemented a private DNS infrastructure in Azure using a Linux VM. The goal was to host a custom domain (`internal.local`) and route traffic internally to a web server through DNS resolution. This exercise closely simulates how DNS routing works in private enterprise environments and strengthens my understanding of cloud networking and name resolution in virtual networks.

---

## Step-by-Step Implementation

### Step 1: Provisioned a Linux VM (Ubuntu)

I provisioned a Linux-based VM using. This machine was used to install and run both the Apache web server and BIND9 DNS server.

![Screenshot 2025-06-15 141415](https://github.com/user-attachments/assets/6f43c898-8fe4-4599-b57d-241c319984e9)


### Step 2: Installed and Configured Apache Web Server

I connected to the VM via SSH and installed Apache2 as the web server:

```bash
sudo apt update
sudo apt install apache2 -y
systemctl status apache2 
```

I then created a simple `index.html` page to serve as the homepage.

Apache was up and running on port 80, successfully serving web content.

![Screenshot 2025-06-15 141427](https://github.com/user-attachments/assets/37b5e0ab-c8fe-47b7-8aa4-6a0c75041197)


### Step 3: Installed and Configured BIND9 DNS Server

To simulate an internal DNS server, I installed **BIND9**:

```bash
sudo apt install bind9 bind9utils -y
```

![Screenshot 2025-06-15 141440](https://github.com/user-attachments/assets/ac5e6384-42a0-4924-b3bd-da7d37de1ac7)


I created a directory to store DNS zone files:

```bash
sudo mkdir -p /etc/bind/zones
```

### Step 4: Created the Forward DNS Zone

I created a forward zone file for the domain `internal.local`:

```bash
sudo nano /etc/bind/zones/internal.local.db
```

Inside the zone file, I defined the A record pointing the domain `csidevopsweb.internal.local` to the VM’s private IP.

![Screenshot 2025-06-15 141454](https://github.com/user-attachments/assets/0b3ac6b0-7a2e-443c-ab59-c8ffbcd4d8e7)


### Step 5: Linked the Zone in BIND Configuration

I edited the `named.conf.local` file to include the new zone:

```bash
sudo nano /etc/bind/named.conf.local
```

I added a zone declaration that points to the zone file created above.

![Screenshot 2025-06-15 141506](https://github.com/user-attachments/assets/0b9a4515-ed5b-45d6-a4ea-4173069af7a4)


### Step 6: Verified Zone Syntax and Restarted BIND9

To ensure no syntax errors existed, I checked the zone file:

```bash
sudo named-checkzone internal.local /etc/bind/zones/internal.local.db
```

The output was **OK**.

Then, I restarted and enabled BIND9 to persist the service:

```bash
sudo systemctl restart bind9
sudo systemctl enable bind9
```

## Testing Domain Resolution

### Step 1: Local DNS Test with nslookup

I tested the DNS resolution directly on the DNS VM:

```bash
nslookup csidevopsweb.internal.local
```

It successfully returned the correct IP address.

### Step 2: Accessed Web Server Using Domain Name

To verify that DNS routing was working end-to-end, I used `curl` to fetch the web page using the domain name:

```bash
curl http://csidevopsweb.internal.local
```

✅ Response: `Hello This is Vikas From CSI DevOps Configured Custom Domain`

![Screenshot 2025-06-15 141517](https://github.com/user-attachments/assets/16a8a93f-6db0-4b39-95a8-261fd7b45984)


### Step 3: Verified from Another VM in Same VNet

To test cross-VM DNS resolution, I deployed a second VM in the same VNet (Windows).

![Screenshot 2025-06-15 141537](https://github.com/user-attachments/assets/b7a6a3a8-b6d7-43c6-9bc8-c82173577121)

I ran:

```bash
nslookup csidevopsweb.internal.local <DNS_VM_PRIVATE_IP>
```

✅ It resolved the domain successfully and loaded the web page.

![WhatsApp Image 2025-06-15 at 15 16 48_ca3ae133](https://github.com/user-attachments/assets/f5404b59-dab2-4daa-a334-20b4b0ee16f2)

---

## Conclusion

Through this hands-on task, I successfully configured a custom internal DNS using BIND9 on an Azure VM. I was able to create a DNS zone (`internal.local`), add domain records, and verify internal routing of web traffic to another VM using domain-based addressing.

---
