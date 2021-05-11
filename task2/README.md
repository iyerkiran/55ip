# Task2

## Problem Statement:
Description: There are 3 servers running App version 1(v1) in AWS under autoscaling.
Scaling policy is configured in such way that if average cpu percentage cross 60%, additional 2 new servers will get launched.
Now you have total 5 servers running v1. Team decide to deploy the new version of App version 2 (v2) using ansible. you need to 
make sure your ansible inventory is upto date (with newly added servers). Write a script that will create the ansible inventory 
with newly provisioned servers. Feel free mention any additional tool or different approach to achieve the same

## Proposed Solution:
1. Ensure we have ansible setup and functional on the master node
2. Ensure appropriate networking is setup for master to connect to all nodes
3. Create the ansible config file viz. ansible.cfg in the cwd
4. Execute requirements.txt
5. Install Python3: sudo yum install -y python3
6. For dynamic inventory, we use the ansible aws plugin and create file inventory_aws_ec2.yml
7. For Ansible to communicate with AWS for fetching the inventory, attach the IAM role to ansible master with EC2FullAccess policy.
8. List the dymanic inventory from AWS. (This will list ALL servers)
  * ansible-inventory --graph -i inventory_aws_ec2.yml
  * To list only a group (in our case the ASG server, we can filter by tags present in inventory_aws_ec2.yml)
  * ansible env_dev_asg -m ping (Preferred)
  * OR
  * ansible env_dev_asg -m ping (This will list ALL the instances including the ones in the ASG)
10. Now, as our inventory is set, we are ready to execute our playbooks.
Note: Update the host group in the playbook or ensure the inventory file is filtered with required hosts.
