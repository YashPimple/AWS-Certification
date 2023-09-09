
IAM is one of like other AWS services is eventually consistent as this data is replicated across multiple servers. 
IAM is a global service (Not scoped per region). Mainly two services can allow access without authentication / authorization in AWS — **STS** and **S3**

Following terms are used in context of IAM:

- User : can be an IAM user or an applications accessing the AWS resources
- Group :Group of users who can be collectively permitted some actions
- Role : This is something which can be assumed by an entity like User or Group (This is similar to Authentication)
- Policy : This defines the permissions you have on the resources that you want to access (This is similar to Authorization)

Policies can be managed in two ways:
1. Managed policies : AWS Managed & Customer Managed
2. Inline Policies

Policies can further be of two types:

i) Identity based policies : These are attached directly to Identities like User/Groups etc. They can be managed or inline policies.

ii) Resource based policies : These are inline policies directly applied on the resource that has to be accessed from same/other accounts. This is mainly used for cross-account resource access
Policy versioning: Customer managed policies can normally have only 5 versions being managed at a single point of time. This is useful when you make a change to a policy and it breaks something, you can quickly set the default setting to a previously used policy
IAM Roles are more preferred instead of resource based policies which are not extendable to other entities
