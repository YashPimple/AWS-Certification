
IAM is one of like other AWS services is eventually consistent as this data is replicated across multiple servers. 
IAM is a global service (Not scoped per region). Mainly two services can allow access without authentication / authorization in AWS — **STS** and **S3**

Following terms are used in context of IAM:

## Users & Groups:
- Groups are collections of users and have policies attached to them
- Groups cannot be nested
- User can belong to multiple groups
- User doesn't have to belong to a group
- Root User has full access to the account
- IAM User has limited permission to the account
- You should log in as an IAM user with admin access even if you have root access. This is just to be sure that nothing goes wrong by accident.

## Policies
Policies are JSON documents that outline permissions for users, groups or roles
Two types
1. **User based policies**:
IAM policies define which API calls should be allowed for a specific user
2. **Resource based policies**:
Control access to an AWS resource
Grant the specified principal permission to perform actions on the resource and define under what conditions this applies
An IAM principal can access a resource if the user policy ALLOWS it OR the resource policy ALLOWS it AND there’s no explicit DENY.
Policies assigned to a user are called inline policies
Follow least privilege principle for IAM Policies
- Policy Structure:
  ![policy-struct](https://github.com/YashPimple/AWS-Certification/assets/97302447/9479ba9c-6149-4a72-b28c-f72f69c61988)

## Roles 
Collection of policies for AWS services
> If you are going to use an IAM Service Role with Amazon EC2 or another AWS service that uses Amazon EC2, you must store the role in an instance profile. When you create an IAM service role for EC2, the role automatically has EC2 identified as a trusted entity.

## Protect IAM Accounts
1. Password Policy:
- Used to enforce standards for password
   - password rotation
   - password reuse
- Prevents brute force attack
2. Multi Factor Authentication (MFA):
- Both root user and IAM users should use MFA

## IAM Security Tools
1. IAM Credentials Report
- lists all the users and the status of their credentials (MFA, password rotation, etc.)
- account level - used to audit security for all the users
2. IAM Access Advisor
- shows the service permissions granted to a user and when those services were last accessed
user-level
- used to revise policies for a specific user

## IAM Guidelines & Best Practices
- Don't use the root account except for AWS account setup
- One physical user = One AWS user
- Assign users to groups and assign permissions to groups
- Create a strong password policy
- Use and enforce the use of Multi Factor Authentication (MA)
- Create and use Roles for giving permissions to AWS services
- Use Access Keys for Programmatic Access (CLI / SDK)
- Audit permissions of your account using AM Credentials Report & IAM
Access Advisor
- Never share your IAM user and its Access Key
