# AWS-DEVOPS-INTERN-ASSIGNMENT-1-DAY
## Task 1: Create an AWS EC2 Instance
### Step 1: Launch EC2 Instance
- Go to **AWS Management Console** → **EC2** → **Launch Instance**.
- Choose **Ubuntu Server 22.04 LTS (Free Tier eligible)**.
- Select **t3.micro** (or t3.medium if we want more resources).
- Add inbound rules:
    - **Port 22 (SSH)**
    - **Port 80 (HTTP)**
- Configure:
    - **Network**: Default VPC
    - **Subnet**: Public subnet
    - **Auto‑assign Public IP**: Enabled
- Add **Key Pair** (download .pem file).
- Launch instance.

### Connect via SSH
On our local machine locate .pem file and run the following command.
```bash
chmod 400 your-key.pem
ssh -i "your-key.pem" ubuntu@<EC2_Public_IP>
```

## Task 2: Linux Basics
### Update Packages
```bash
sudo apt update && sudo apt upgrade -y
```
### Install Nginx
```bash
sudo apt install nginx -y
```

Verification:
```bash
nginx -v
```

### Check Nginx Status
```bash
systemctl status nginx
```

### Restart Nginx
```bash
sudo systemctl restart nginx
```

Verification:
```bash
systemctl status nginx
```

### Check Disk Usage
```bash
df -h
```
- Shows disk usage in human‑readable format.
- Useful for monitoring available space.

### Check Memory Usage
```bash
free -m
```
- Displays memory usage in MB.

### Check Running Processes
```bash
ps -ef | head
```
- Lists all processes with details.

Or use:
```bash
top
```
- Interactive view of CPU/memory usage.

## Task 3: Host a Simple Website
### Create Your HTML Page
Create a new file with our details:
```bash
vim index.html
```

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Simple Website</title>
</head>
<body>
    <h1>Name: Atul</h1>
    <p>College: XYZ Institute</p>
    <p>Branch: Computer Science</p>
    <p>Email: atul@example.com</p>
    <p>Date: $(date)</p>
</body>
</html>
```

### Replace Default Nginx Page
Copy our HTML file to Nginx’s default web directory:
```bash
sudo cp index.html /var/www/html/index.html
```

Ensure the file is readable by Nginx:
```bash
ls -l /var/www/html/index.html
```
- Should show root ownership and read permissions.

### Restart Nginx
```bash
sudo systemctl restart nginx
```

### Access Website
Open our browser and visit:
```Code
http://<EC2_Public_IP>
```
- We should see our custom HTML page instead of the Nginx welcome page.

## Bonus
### Attach an Elastic IP
- Go to **EC2 Dashboard** → **Elastic IPs** → **Allocate Elastic IP**.
- Click **Allocate**.
- Select the **Elastic IP** → **Actions** → **Associate Elastic IP Address**.
- Choose our EC2 instance and associate.
- **Verification**: Our EC2 Public IP becomes static (won’t change after reboot).

### Create Another Security Group
- Go to **EC2 Dashboard** → **Security Groups** → **Create Security Group**.
- Add inbound rules (e.g., allow port 8080 for testing).
- Attach this new Security Group to our EC2 instance.
- Verification: Check inbound rules applied by visiting our instance’s **Networking** → **Security Groups**.

### Install Docker & Run Hello‑World
``bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
newgrp docker
```

Run hello‑world:
```bash
sudo docker run hello-world
```

### Create Shell Script to Restart Nginx
Create a script:
```bash
nano restart_nginx.sh
```

Paste:
```bash
#!/bin/bash
sudo systemctl restart nginx
echo "Nginx restarted successfully!"
```

Make it executable:
```bash
chmod +x restart_nginx.sh
```

Run:
```bash
./restart_nginx.sh
```
- Verification: Nginx restarts and message prints.