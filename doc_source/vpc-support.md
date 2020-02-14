# Use CodeBuild with Amazon Virtual Private Cloud<a name="vpc-support"></a>

Typically, resources in an VPC are not accessible by AWS CodeBuild\. To enable access, you must provide additional VPC\-specific configuration information as part of your CodeBuild project configuration\. This includes the VPC ID, the VPC subnet IDs, and the VPC security group IDs\. VPC\-enabled builds are then able to access resources inside your VPC\. For more information about setting up a VPC in Amazon VPC, see the [VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html)\.

**Note**  
 VPC connectivity from CodeBuild is not supported in Windows\. 

**Topics**
+ [Use Cases](#use-cases)
+ [Enabling Amazon VPC Access in Your CodeBuild Projects](#enabling-vpc-access-in-projects)
+ [Best Practices for VPCs](#best-practices-for-vpcs)
+ [Troubleshooting Your VPC Setup](#troubleshooting-vpc)
+ [Use VPC Endpoints](use-vpc-endpoints-with-codebuild.md)
+ [AWS CloudFormation VPC Template](cloudformation-vpc-template.md)
+ [Use CodeBuild with a Proxy Server](use-proxy-server.md)

## Use Cases<a name="use-cases"></a>

VPC connectivity from AWS CodeBuild builds makes it possible to:
+ Run integration tests from your build against data in an Amazon RDS database that's isolated on a private subnet\.
+ Query data in an Amazon ElastiCache cluster directly from tests\.
+ Interact with internal web services hosted on Amazon EC2, Amazon ECS, or services that use internal Elastic Load Balancing\.
+ Retrieve dependencies from self\-hosted, internal artifact repositories, such as PyPI for Python, Maven for Java, and npm for Node\.js\.
+ Access objects in an Amazon S3 bucket configured to allow access through an Amazon VPC endpoint only\.
+ Query external web services that require fixed IP addresses through the Elastic IP address of the NAT gateway or NAT instance associated with your subnet\(s\)\.

Your builds can access any resource that's hosted in your VPC\.

## Enabling Amazon VPC Access in Your CodeBuild Projects<a name="enabling-vpc-access-in-projects"></a>

Include these settings in your VPC configuration:
+ For **VPC ID**, choose the VPC ID that CodeBuild uses\.
+ For **Subnets**, choose a private subnet with NAT translation that includes or has routes to the resources used CodeBuild\.
+ For **Security Groups**, choose the security groups that CodeBuild uses to allow access to resources in the VPCs\.

**Create a build project \(console\)**

For information about creating a build project, see [Create a Build Project \(Console\)](create-project.md#create-project-console)\. When you create or change your CodeBuild project, in **VPC**, choose your VPC ID, subnets, and security groups\. 

**Create a build project \(AWS CLI\)**

For information about creating a build project, see [Create a Build Project \(AWS CLI\)](create-project.md#create-project-cli)\. If you are using the AWS CLI with CodeBuild, the service role used by CodeBuild to interact with services on behalf of the IAM user must have the following policy attached: [Allow CodeBuild Access to AWS Services Required to Create a VPC Network Interface](auth-and-access-control-iam-identity-based-access-control.md#customer-managed-policies-example-create-vpc-network-interface)\.

The *vpcConfig* object should include your *vpcId*, *securityGroupIds*, and *subnets*\.
+ *vpcId*: Required value\. The VPC ID that CodeBuild uses\. To get a list of all Amazon VPC IDs in your region, run this command:

  ```
  aws ec2 describe-vpcs
  ```
+ *subnets*: Required value\. The subnet IDs that include resources used by CodeBuild\. To obtain these IDs, run this command:

  ```
  aws ec2 describe-subnets --filters "Name=vpc-id,Values=<vpc-id>" --region us-east-1
  ```
**Note**  
Replace us\-east\-1 with your region\.
+ *securityGroupIds*: Required value\. The security group IDs used by CodeBuild to allow access to resources in the VPCs\. To obtain these IDs, run this command:

  ```
  aws ec2 describe-security-groups --filters "Name=vpc-id,Values=<vpc-id>" --region us-east-1
  ```
**Note**  
Replace us\-east\-1 with your region\.

## Best Practices for VPCs<a name="best-practices-for-vpcs"></a>

Use this checklist when setting up a VPC to work with CodeBuild\.
+ Set up your VPC with public and private subnets and a NAT gateway\. For more information, see [VPC with Public and Private Subnets \(NAT\)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html)\.
**Important**  
You need a NAT gateway or NAT instance in order to use CodeBuild with your Amazon VPC so that CodeBuild can reach public endpoints \(for example, to execute CLI commands when running builds\)\. You cannot use the internet gateway instead of a NAT gateway or a NAT instance because CodeBuild does not support assigning elastic IP addresses to the network interfaces that it creates, and auto\-assigning a public IP address is not supported by Amazon EC2 for any network interfaces created outside of Amazon EC2 instance launches\. 
+ Include multiple Availability Zones with your VPC\.
+ Make sure that your security groups have no inbound \(ingress\) traffic allowed to your builds\. For more information, see [Security Groups Rules](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#SecurityGroupRules)\.
+ Set up separate subnets for your builds\.
+ When you set up your CodeBuild projects to access your VPC, choose private subnets only\. 

For more information about setting up a VPC in Amazon VPC, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html)\.

For more information about using AWS CloudFormation to configure an Amazon VPC to use the CodeBuild VPC feature, see the [AWS CloudFormation VPC Template](cloudformation-vpc-template.md)\.

## Troubleshooting Your VPC Setup<a name="troubleshooting-vpc"></a>

When troubleshooting VPC issues, use the information that appears in the error message to help you identify, diagnose, and address issues\.

The following are some guidelines to assist you when troubleshooting a common CodeBuild VPC error: "Build does not have internet connectivity\. Please check subnet network configuration"\. 

1. [Make sure that your internet gateway is attached to VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway)\.

1. [Make sure that the route table for your public subnet points to the internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html#route-tables-internet-gateway)\.

1. [Make sure that your network ACLs allow traffic to flow](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#SecurityGroupRules)\.

1. [Make sure that your security groups allow traffic to flow](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html#SecurityGroupRules)\.

1. [Troubleshoot your NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-troubleshooting)\.

1. [Make sure that the route table for private subnets points to the NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html#route-tables-nat)\.

1. Make sure that the service role used by CodeBuild to interact with services on behalf of the IAM user has the permissions in [ this policy](https://docs.aws.amazon.com/codebuild/latest/userguide/auth-and-access-control-iam-identity-based-access-control.html#customer-managed-policies-example-create-vpc-network-interface)\. For more information, see [Create a CodeBuild Service Role](setting-up.md#setting-up-service-role)\. 

   If CodeBuild is missing permissions, you might receive an error that says, "Unexpected EC2 error: UnauthorizedOperation\." This error can occur if CodeBuild does not have the Amazon EC2 permissions required to work with an Amazon VPC\.