% AWS IAM
% https://github.com/STL2600/aws-iam

# What is it?

 - Identity and Access Management for AWS
 
# Users and Groups

# Roles

 - Have a policy or policies representing permissions
 - Have a trusted list of entities which an assume the role

# Policies

## Role / User / Group Policies

 - Applied at the user level, and control what that entity is allowed to do in AWS

## Resource Policies

 - Applied at the resource level, and control what operations can be performed on the resource
 
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
                "AWS": "arn:aws:iam::123456789012:user/carlossalazar"
            },
            "Action": [
                "s3:*",
            ]
            "Resource": [
                "arn:aws:s3:::my-s3-bucket/*",
                "arn:aws:s3:::my-s3-bucket"
            ]
        }
    ]
}
```

## Effect

 - `Allow` or `Deny`

## Principals

 - Used for resource policies
 - The identity of the person making the request
 - `"Principal": "*"` for unathenticated users
 - Could be a role, user, or an AWS service itself

## Actions

 - Resource Specific
 - Supports Wildcards `"S3:*"`

## Resources

 - Used for identity policies
 - The resources to allow or deny access to

## Conditions

 - Allow for fine grained control based on multiple factors
 - Some common conditions are available
 - Resources will have their own conditions
  
## Conditions - SecureTransport

## Conditions - SourceIP

## Conditions - ResourceTag

# Best Practices

## Use Roles not Users

 - For human users, use SSO or some type of system to dynamically grant roles when needed
 - For automated systems, have the services assume roles when needed

## Few if any Static Creds

 - Less secrets to keep and rotate

## Access Analyzer

 - Remove uneeded permissions when possible
