# Task6

## Problem Statement:
Write a script to identify AWS security credentials older than 90 day & renew them.

## Proposed Solution:
Below is the code used, however it is NOT 100% functional. But an overall structure may provide us and idea.

Note: Once this is successful, we can create it as a lambda function and schedule it to autorun using CloudWatch.

```sh
import boto3
from botocore.exceptions import ClientError
import datetime
import json
iam_client = boto3.client('iam')
def list_access_key(user, days_filter, status_filter):
    print("inside list_access_key")
    keydetails=iam_client.list_access_keys(UserName=user)
    print(type(keydetails))
    print(keydetails)
    key_details={}
    user_iam_details=[]

    # Some user may have 2 access keys.
    for keys in keydetails['AccessKeyMetadata']:
        days=time_diff(keys['CreateDate'])
        if (days) >= days_filter and keys['Status']==status_filter:
            key_details['UserName']=keys['UserName']
            key_details['AccessKeyId']=keys['AccessKeyId']
            key_details['days']=days
            key_details['status']=keys['Status']
            user_iam_details.append(key_details)
            key_details={}

    return user_iam_details
def time_diff(keycreatedtime):
    print("reached time_diff")
    now=datetime.datetime.now(datetime.timezone.utc)
    print(now)
    diff=now-keycreatedtime
    print(diff)
    return diff.days
def create_key(username):
    print("print create_key reached")
    access_key_metadata = iam_client.create_access_key(UserName=username)
    access_key = access_key_metadata['AccessKey']['AccessKeyId']
    secret_key = access_key_metadata['AccessKey']['SecretAccessKey']
def disable_key(access_key, username):
    try:
        iam_client.update_access_key(UserName=username, AccessKeyId=access_key, Status="Inactive")
        print(access_key + " has been disabled.")
    except ClientError as e:
        print("The access key with id %s cannot be found" % access_key)
def delete_key(access_key, username):
    try:
        iam_client.delete_access_key(UserName=username, AccessKeyId=access_key)
        print (access_key + " has been deleted.")
    except ClientError as e:
        print("The access key with id %s cannot be found" % access_key)

details = iam_client.list_users(MaxItems=100)
users = details['Users']
print(type(users))
print(users)
for user in users:
    print(user)
    user_iam_details=list_access_key(user=user["UserName"], days_filter=90, status_filter='Inactive')
    print(type(user_iam_details))
    #print("Iam user having keys older than 90 days")
    #print(user_iam_details)
    for iam_u in user_iam_details:
        print("############# Iam user having keys older than 90 days #########")
        print (iam_u['UserName'])
        #Uncomment to use
        #NOTE: THIS WILL DELETE THE OLD KEYS AND APPLICATION WILL BREAK
        #disable_key(access_key=iam_u['AccessKeyId'], username=iam_u['UserName'])
        #delete_key(access_key=iam_u['AccessKeyId'], username=iam_u['UserName'])
        #create_key(username=iam_u['UserName'])
```
