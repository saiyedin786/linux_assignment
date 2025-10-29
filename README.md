# linux_assignment
Problem Statement 

Rahul, a Senior DevOps engineer at TechCorp, needs to set up and manage development environments for two new developers, Sarah and Mike. The setup must ensure proper system monitoring, user management, troubleshooting capabilities, and automated backup procedures.

As a Fresher DevOps Engineer, you are assisting Rahul in implementing a secure, monitored, and well-maintained development environment that meets the organization’s operational and security standards. Additionally, Sarah and Mike are tasked with setting up automated backups for their respective web servers.


Task 1: System Monitoring Setup

Install and configure htop or nmon to monitor CPU, memory, and processes, using df and du for disk usage tracking, and identifying resource-intensive processes. Proper logging of system metrics and clear documentation of the setup are essential. 

installing htop:
sudo update -y
sudo install epel-release -y
sudo install htop -y

after installing htop run command htop
and i got following output

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5400926c-9de3-409a-9cc2-a3e1530f908d" />

we can monitor cpu, memory, processes etc by issuing command htop and then pressing f6 and than selecting metric from the list

