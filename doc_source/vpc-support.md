# Use AWS CodeBuild with Amazon Virtual Private Cloud<a name="vpc-support"></a>

Typically, AWS CodeBuild cannot access resources in a VPC\. To enable access, you must provide additional VPC\-specific configuration information in your CodeBuild project configuration\. This includes the VPC ID, the VPC subnet IDs, and the VPC security group IDs\. VPC\-enabled builds can then access resources inside your VPC\. For more information about setting up a VPC in Amazon VPC, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html)\.

**Note**  
 VPC connectivity from CodeBuild is not supported in Windows\. 

**Topics**
+ [Use cases](#use-cases)
+ [Allowing Amazon VPC access in your CodeBuild projects](#enabling-vpc-access-in-projects)
+ [Best practices for VPCs](#best-practices-for-vpcs)
+ [Troubleshooting your VPC setup](#troubleshooting-vpc)
+ [Use VPC endpoints](use-vpc-endpoints-with-codebuild.md)
+ [AWS CloudFormation VPC template](cloudformation-vpc-template.md)
+ [Use AWS CodeBuild with a proxy server](use-proxy-server.md)

## Use cases<a name="use-cases"></a>

VPC connectivity from AWS CodeBuild builds makes it possible to:
+ Run integration tests from your build against data in an Amazon RDS database that's isolated on a private subnet\.
+ Query data in an Amazon ElastiCache cluster directly from tests\.
+ Interact with internal web services hosted on Amazon EC2, Amazon ECS, or services that use internal Elastic Load Balancing\.
+ Retrieve dependencies from self\-hosted, internal artifact repositories, such as PyPI for Python, Maven for Java, and npm for Node\.js\.
+ Access objects in an S3 bucket configured to allow access through an Amazon VPC endpoint only\.
+ Query external web services that require fixed IP addresses through the Elastic IP address of the NAT gateway or NAT instance associated with your subnet\.

Your builds can access any resource that's hosted in your VPC\.

## Allowing Amazon VPC access in your CodeBuild projects<a name="enabling-vpc-access-in-projects"></a>

Include these settings in your VPC configuration:
+ For **VPC ID**, choose the VPC ID that CodeBuild uses\.
+ For **Subnets**, choose a private subnet with NAT translation that includes or has routes to the resources used by CodeBuild\.
+ For **Security Groups**, choose the security groups that CodeBuild uses to allow access to resources in the VPCs\.

To use the console to create a build project, see [Create a build project \(console\)](create-project-console.md)\. When you create or change your CodeBuild project, in **VPC**, choose your VPC ID, subnets, and security groups\. 

To use the AWS CLI to create a build project, see [Create a build project \(AWS CLI\)](create-project-cli.md)\. If you are using the AWS CLI with CodeBuild, the service role used by CodeBuild to interact with services on behalf of the IAM user must have a policy attached\. For information, see [Allow CodeBuild access to AWS services required to create a VPC network interface](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-create-vpc-network-interface)\.

The *vpcConfig* object should include your *vpcId*, *securityGroupIds*, and *subnets*\.
+ *vpcId*: Required\. The VPC ID that CodeBuild uses\. Run this command to get a list of all Amazon VPC IDs in your Region:

  ```
  aws ec2 describe-vpcs
  ```
+ *subnets*: Required\. The subnet IDs that include resources used by CodeBuild\. Run this command obtain these IDs:

  ```
  aws ec2 describe-subnets --filters "Name=vpc-id,Values=<vpc-id>" --region us-east-1
  ```
**Note**  
Replace `us-east-1` with your Region\.
+ *securityGroupIds*: Required\. The security group IDs used by CodeBuild to allow access to resources in the VPCs\. Run this command to obtain these IDs:

  ```
  aws ec2 describe-security-groups --filters "Name=vpc-id,Values=<vpc-id>" --region us-east-1
  ```
**Note**  
Replace `us-east-1` with your Region\.

## Best practices for VPCs<a name="best-practices-for-vpcs"></a>

Use this checklist when you set up a VPC to work with CodeBuild\.
+ Set up your VPC with public and private subnets and a NAT gateway\. For more information, see [VPC with public and private subnets \(NAT\)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html) in the *Amazon VPC User Guide*\.
**Important**  
You need a NAT gateway or NAT instance to use CodeBuild with your VPC so that CodeBuild can reach public endpoints \(for example, to execute CLI commands when running builds\)\. You cannot use the internet gateway instead of a NAT gateway or a NAT instance because CodeBuild does not support assigning Elastic IP addresses to the network interfaces that it creates, and auto\-assigning a public IP address is not supported by Amazon EC2 for any network interfaces created outside of Amazon EC2 instance launches\. 
+ Include multiple Availability Zones with your VPC\.
+ Make sure that your security groups have no inbound \(ingress\) traffic allowed to your builds\. CodeBuild does not have specific requirements for outbound traffic, but you must allow access to any Internet resources required for your build, such as GitHub or Amazon S3\.

  For more information, see [Security groups rules](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#SecurityGroupRules) in the *Amazon VPC User Guide*\. 
+ Set up separate subnets for your builds\.
+ When you set up your CodeBuild projects to access your VPC, choose private subnets only\. 

For more information about setting up a VPC in Amazon VPC, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html)\.

For more information about using AWS CloudFormation to configure a VPC to use the CodeBuild VPC feature, see the [AWS CloudFormation VPC template](cloudformation-vpc-template.md)\.

## Troubleshooting your VPC setup<a name="troubleshooting-vpc"></a>

Use the information that appears in the error message to help you identify, diagnose, and address issues\.

The following are some guidelines to assist you when troubleshooting a common CodeBuild VPC error: `Build does not have internet connectivity. Please check subnet network configuration`\. 

1. [Make sure that your internet gateway is attached to VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway)\.

1. [Make sure that the route table for your public subnet points to the internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html#route-tables-internet-gateway)\.

1. [Make sure that your network ACLs allow traffic to flow](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#SecurityGroupRules)\.

1. [Make sure that your security groups allow traffic to flow](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#SecurityGroupRules)\.

1. [Troubleshoot your NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC-nat-gateway.html#nat-gateway-troubleshooting)\.

1. [Make sure that the route table for private subnets points to the NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html#route-tables-nat)\.

1. Make sure that the service role used by CodeBuild to interact with services on behalf of the IAM user has the permissions in [ this policy](https://docs.aws.amazon.com/codebuild/latest/userguide/auth-and-access-control-iam-identity-based-access-control.html#customer-managed-policies-example-create-vpc-network-interface)\. For more information, see [Create a CodeBuild service role](setting-up.md#setting-up-service-role)\. 

   If CodeBuild is missing permissions, you might receive an error that says, `Unexpected EC2 error: UnauthorizedOperation`\. This error can occur if CodeBuild does not have the Amazon EC2 permissions required to work with a VPC\.