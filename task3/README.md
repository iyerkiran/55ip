# Task3

## Problem Statement:
Write a script to modify AWS security group to allow access to port 80 only from your public IP. Feel
free to explore any scripting language shell, python, etc.

## Proposed Solution:
Assumptions:
1. The script has to be executed from the device itself.
2. AWS CLI profile is configured.

```sh
#!/bin/bash
# Retrieve current IP address
IP=`curl -s ipinfo.io/ip`

# Else if you are executing the script from a proxy, substitute your public IP below and uncomment the code.
#IP=52.6.229.168

#Create a new ingress rule with the public IP
aws ec2 authorize-security-group-ingress \
--group-name "XXXXXX" \
--protocol tcp --port 80 \
--cidr $IP/32 \
--profile XXXXXX \
--output text
```
