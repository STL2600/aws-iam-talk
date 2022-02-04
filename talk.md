% AWS IAM
% https://github.com/STL2600/aws-iam
% ![Link to Talk](images/qr-code.png)  

# Overview

 - Identity and Access Management for AWS
 - Things we'll cover
   - Identities
   - Policies
   - Best Practices

# Identities
 
## Users and Groups

 - Similar to users and groups in other systems
 - Can have policies directly attached
 - Can have roles attached
 - Prefer roles to users and groups where possible

## Roles

 - Have a policy or policies representing permissions
 - Have a trusted list of entities which an assume the role

# Policies

## Types
 
 - Identity Policies
 - Resource Policies

## Identity Policies

 - Applied at the user / group / role level, and control what that entity is allowed to do in AWS
 - Probably the most common

## Resource Policies

 - Applied at the resource level, and control what operations can be performed on the resource
 - Useful for anonymous access and blanket restrictions
 
## Policy Evaluation

 !["Policy Evaluation"](images/PolicyEvaluationHorizontal111621.png) 
 
 [Full Evaluation Description](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
 
## Policy Evaluation

 - Any explicit deny will fail the evaluation
 - An allow from either the identity policy or the resource policy will cause the evaluation to succeed

## Format and Structure

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::123456789012:user/username"
            },
            "Action": ["s3:*"],
            "Resource": [
                "arn:aws:s3:::my-s3-bucket/*",
                "arn:aws:s3:::my-s3-bucket"
            ],
            "Condition": {
                "NumericLessThanEquals": {
                    "aws:MultiFactorAuthAge": "3600"
                }
            }
        }
    ]
}
```

## Effect

 - `Allow` or `Deny`

## Principals

 - Used for resource policies
 - The identity of the person making the request
 - Could be a role, user, or an AWS service itself

## Principals

```
"Principal": "*"
```
 - Anyone
 - Very limited use cases

## Principals

```
"Principal": {
    "AWS": ["123456789012"]
}
```
 - Another AWS Account
 - That account must also grant appropriate permissions

## Principals

```
"Principal": {
    "AWS": ["arn:aws:iam::123456789012:username"]
}
```
 - Specific in this or another account

## Principals

```
"Principal": {
    "AWS": ["arn:aws:iam::123456789012:role/role-name"]
}
```
 - Specific role in this or another account

## Principals

```
"Principal": {
    "AWS": ["arn:aws:sts::123456789012:assumed-role/role-name/role-session-name"]
}

"Principal": {
    "AWS": ["arn:aws:sts::123456789012:assumed-role/role-name/*"]
}
```
 - Assumed Role 
 - Applies when using an `assume-role` call from another identity

## Principals

```
"Principal": {
    "Service": ["ecs.amazonaws.com"]
}
```
 - Used to let AWS services operate on your resources

## Actions

 - Resource Specific
 - Format is `<service>:<action>`
 - Supports Wildcards
   - `"S3:*"`
   - `"S3:Get*"`

## Resources

 - Used for identity policies
 - Always in ARN format
 - The resources to allow or deny access to
 - Supports Wildcards `"arn:aws:s3:::s3-bucket-name/s3-directory/*/file.txt"`

## Conditions

 - Allow for fine grained control based on multiple factors
 - Some common conditions are available
 - Resources will have their own conditions

## Conditions - SecureTransport

```
"Condition":{
    "Bool": {
        "aws:SecureTransport": false
    }
}
```
 - Match on unsecure connections
 - Could be used to deny all non-secure connections

## Conditions - SourceIP

```
"Condition": {
    "NotIpAddress": {
        "aws:SourceIp": ["192.0.2.0/24"]
    },
    "Bool": {"aws:ViaAWSService": "false"}
}
```
 - Can be used to restrict request to a list of IP address

## Conditions - ResourceTag

```
"Condition": {
    "StringEquals": {
        "ec2:ResourceTag/CostCenter": "MyDepartment"
    }
}
```
 - Can be used to only allow requests to resources tagged a certain way

# Best Practices

## Never use Root Creds

 - Create users or roles, then do all your activity through those

## Use Roles not Users

 - For human users, use SSO or some type of system to dynamically grant roles when needed
 - For automated systems, have the services assume roles when needed

## Few if any Static Creds

 - Less secrets to keep and rotate
 - This goes along with roles, not users

## Access Analyzer

 - Remove uneeded permissions when possible

## Monitor Events in CloudTrail

 - Track all the times your admin roles were assumed
 - Track changes to permissions

# Questions?

![Link to Talk](images/qr-code.png)  

[https://github.com/STL2600/aws-iam-talk](https://github.com/STL2600/aws-iam-talk)
