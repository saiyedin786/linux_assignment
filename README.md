# linux_assignment
Problem Statement 

Rahul, a Senior DevOps engineer at TechCorp, needs to set up and manage development environments for two new developers, Sarah and Mike. The setup must ensure proper system monitoring, user management, troubleshooting capabilities, and automated backup procedures.

As a Fresher DevOps Engineer, you are assisting Rahul in implementing a secure, monitored, and well-maintained development environment that meets the organization’s operational and security standards. Additionally, Sarah and Mike are tasked with setting up automated backups for their respective web servers.


Task 1: System Monitoring Setup

Install and configure htop or nmon to monitor CPU, memory, and processes, using df and du for disk usage tracking, and identifying resource-intensive processes. Proper logging of system metrics and clear documentation of the setup are essential. 

Task 2: User Management and Access Control

Evaluation includes creating user accounts for Sarah and Mike with secure passwords, setting up isolated directories with appropriate permissions, and enforcing a password policy with expiration and complexity. Detailed documentation of user management steps is required. 


Task 3: Backup Configuration for Web Servers

Configure automated backups for Apache (/etc/httpd/, /var/www/html/) and Nginx (/etc/nginx/, /usr/share/nginx/html/), scheduling cron jobs to run every Tuesday at 12:00 AM, using the correct naming convention for backup files, and verifying backup integrity. Proper documentation and logs are necessary. 

Overall Report and Presentation



#Task 1 solution:

##system monitoring using htop

installing htop:
sudo update -y
sudo install epel-release -y
sudo install htop -y

after installing htop run command htop
and i got following output

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5400926c-9de3-409a-9cc2-a3e1530f908d" />




we can monitor cpu, memory, processes etc by issuing command htop and then pressing f6 and than selecting metric from the list as given in snapshot below

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/14eabed2-ca86-4e95-ae44-ef904502c260" />


##system monitoring using nmon

installation steps:
sudo update -y
sudo install epel-release -y
sudo install nmon -y


after installing htop run command nmon
and i got following output

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7a735196-76e3-4c6a-badd-eaacd9f7b866" />

as nmon is also interactive process so to get status of memory,cpu and processe following needs to be done:

issue command nmon in terminal than
1. for cpu : press 'c'  to view cpu status
2. for memory : press 'm' to view memory status
3. for processes : press 'u' to view top memory and cpu consuming processes

for cpu follwing output observed:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3362adaa-4daf-4503-8316-194481d0e7f0" />

for memory following output observed:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a9922aa1-7b18-46a9-87ca-2f66437caf26" />


for top processes follwing output observed:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/84004421-3a37-4862-8c07-2a3e25d706e3" />


##Disk Usage Monitoring :
follwing command is used for disk usage monitoring
df -h 

output

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5803f9ee-4de5-4b67-a4ca-8ef2aeadb773" />


to make a log file
df -h > /var/log/disk_usage.log

output of a log file:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/536691fe-4a98-40ba-bdf9-034aef734fe2" />




To automate daily at 8 am in morning:
echo "0 8 * * * df -h > /var/log/disk_usage.log" | sudo tee -a /etc/crontab


Identifying Resource-Intensive Processes and logging to log file

ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -n 10 > /var/log/top_processes.log

output of a log file:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a640d3f2-ab60-4471-aaf8-0ec7c823becb" />

to automate on a hourly basis:

echo "0 * * * * ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -n 10 > /var/log/top_processes.log" | sudo tee -a /etc/crontab


making a script for monitoring purpose:
Create a script /usr/local/bin/system_monitor.sh:

#!/bin/bash
LOG_FILE="/var/log/system_monitor_$(date +%F).log"

echo "===== System Report $(date) =====" >> $LOG_FILE
echo "--- CPU/Memory Usage ---" >> $LOG_FILE
top -b -n1 | head -n 10 >> $LOG_FILE

echo "--- Disk Usage ---" >> $LOG_FILE
df -h >> $LOG_FILE

echo "--- Top Processes ---" >> $LOG_FILE
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -n 10 >> $LOG_FILE

echo "===================================" >> $LOG_FILE


changing permission of above script file:
sudo chmod +x /usr/local/bin/system_monitor.sh

running it hourly basis:
echo "0 * * * * root /usr/local/bin/system_monitor.sh" | sudo tee -a /etc/crontab


output of above script file generated in /var/log as system_monitor_current_date.log file

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a51a894e-91e6-444c-bc47-c4961f5a5205" />





Task 2 solution:

Step 1: Create User Accounts
# Create users with home directories
sudo useradd -m -d /home/Sarah -s /bin/bash Sarah
sudo useradd -m -d /home/mike -s /bin/bash mike

# Set passwords for each user (you will be prompted to enter them)
sudo passwd Sarah
sudo passwd mike

snapshot :

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/db88a2f4-a4ee-4e6d-9d41-15658123b91e" />



Step 2: Create Isolated Working Directories

# Create dedicated workspace directories
sudo mkdir -p /home/Sarah/workspace
sudo mkdir -p /home/mike/workspace

# Set ownership and permissions
sudo chown Sarah:Sarah /home/Sarah/workspace
sudo chown mike:mike /home/mike/workspace

# Restrict access so only the owner can access their workspace
sudo chmod 700 /home/Sarah/workspace
sudo chmod 700 /home/mike/workspace

output:
/home/Sarah/workspace → accessible only by Sarah
/home/mike/workspace → accessible only by Mike

screenshot:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c5cb8171-606b-4dfd-937f-0cf9bdccebee" />




Step 3: Configure Password Policy:
sudo vi /etc/login.defs

PASS_MAX_DAYS   30    # Password expires every 30 days
PASS_MIN_DAYS   1     # Minimum 1 day before password can be changed
PASS_WARN_AGE   7     # Warn user 7 days before password expires

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8a29e9c5-1606-4c35-a713-061d2dd7a38a" />


Task 3 solution:

step-1 : created apache_backup.sh in /usr/local/bin directory

step-2 : vi apache_backup.sh written code for taking backup of /etc/httpd and /var/www/html
and modified the permission using command- sudo chmod +x /usr/local/bin/apache_backup.sh 
the screenshot of apache_backup.sh file is given below
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a8634316-47b0-4251-8abf-2069e9687e88" />
















