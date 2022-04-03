# iampolicy-using-tags

## Description

Hello folks, today we are going to write an IAM policy that could list all the instances on AWS console, but which only has the privilege to start, reboot and stop a paricular instance.This is done based on the tags that we give to the instances.

### What exactly are we doing here?

Suppose, if you have two instances with the same name tag "webserver" and project tag set to "apache". But the enviornment tag of one instance is set to "prod" and the other is set to "dev". So here, what we exacly going to do is creating an IAM user with a custom policy so that this user will have the privilege to see the list of instances on the AWS console, but will only have privilege to stop, start and reboot the instance with env tage set to "dev".

## Steps 

### Step 1

Created two EC2 instances with the same tag "webserver" and project tag set to "apache". We also add an addition tag "env", whose value is set to "prod" for one instance and "dev" for the other instance.

So, Instance 1:
```
Name = websever
project = apache
env = prod
```

Instance 2:
```
Name = webserver
project = apache
env = dev
```

## Step 2

Create an IAM user "user" with "Password - AWS Management Console access". Choose a custom password or an auto generated password for the user.

## Step 3

Now, the main step of the task is here- Creating the custom IAM policy that enables the IAM user "user" to see the list of all the instances on the AWS console, but will only have privilege to stop, start and reboot the instance with "env" tag set to "dev".

So, the required IAM policy is:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
            "ec2:Describe*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:RebootInstances"
            ],
            "Resource": "arn:aws:ec2:ap-south-1:982583122971:instance/*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/Name": "webserver",
                    "ec2:ResourceTag/project": "zomato",
                    "ec2:ResourceTag/env": "dev"
                }
            }
        }
    ]
}
```

### Step 4

Now we need to login to the AWS console as the IAM user "user". Here we will be able to see the list of instances. We will be able to successfully start, stop and reboot the instance with env tag set to "dev".

But when we try to start the instance with "env" tag set to "prod", it will show the following error:





