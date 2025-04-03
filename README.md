# Application-Migration-Service-On-Premises-Simulation


🔹 Step 1: Create the Source Server (EC2 Instance)

1️⃣ Launched a new EC2 instance (Source Server) in us-east-1.
2️⃣ Installed Apache Web Server and hosted a simple HTML file.
3️⃣ Verified that the webpage was accessible using the EC2 instance's public IP.

User Data for Apache and HTML Setup:
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Welcome to AWS Application Migration Service</h1>" > /var/www/html/index.html

🔹 Step 2: Create IAM User with Required Permissions

1️⃣ Created an IAM User specifically for AMS migration.
2️⃣ Attached the following IAM Policies to this user:

[AmazonEC2FullAccess, AmazonS3FullAccess, AWSApplicationMigrationAgentInstallationPolicy, AWSApplicationMigrationFullAccess]

3️⃣ Generated the Access Key ID and Secret Access Key for this IAM user.

🔹 Step 3: Register Source Server in AWS MGN

1️⃣ Navigated to AWS Application Migration Service (AMS) in the AWS console.
2️⃣ Clicked “Add server” and entered the Access Key ID & Secret Access Key of the created IAM user.
3️⃣ AWS provided two commands for installing the AWS MGN agent on the source server:
Download the installer command, Run the installer command
4️⃣ Copied and ran these commands on the source server (EC2) to install AWS MGN Agent.

🔹 Step 4: Verify the Source Server in AWS MGN

1️⃣ Once installation completed, AWS MGN automatically created an internal replication EC2 instance.
2️⃣ The instance appeared under AWS Application Migration Service → Source Servers.
3️⃣ Changed the security group of this new instance to allow all TCP connections.
4️⃣ Verified that the Apache web page from the source server was accessible from the new instance.

🔹 Step 5: Modify Launch Template for Target Server
1️⃣ Modified the default launch template to match the desired target instance type:
Instance Type: t3.medium
Auto-Assign Public IP: Enabled
2️⃣ Set this as the default version of the launch template.

🔹 Step 6: Wait for Replication to Reach 100%
1️⃣ Monitored replication progress under AWS MGN → Source Servers.
2️⃣ Once replication reached 100%, clicked "Create Test Instance".
3️⃣ AWS automatically deleted the "AWS Application Migration Service Conversion Server" and replaced it with a Replication Server.

🔹 Step 7: Perform Cutover to Production
1️⃣ Clicked "Mark as Ready for Cutover".
2️⃣ Clicked "Launch Cutover Instance", which automatically created the final target server.
3️⃣ Changed the security group of the new production server to allow web traffic.
4️⃣ Tested the IP of the new production server and verified that the HTML file was successfully replicated.

🔹 Step 8: Delete the Source Server
1️⃣ Once the new production server was fully operational, clicked "Finalize Cutover".
2️⃣ This deleted the old source server from AMS, completing the migration process.

✅ Final Outcome
Successfully migrated a web server (EC2) from a source to a target AWS environment using AMS.
The production server had the same content as the original source server.
Source server was safely decommissioned after migration.
