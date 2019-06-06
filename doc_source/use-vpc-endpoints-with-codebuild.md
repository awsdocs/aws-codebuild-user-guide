# Use VPC Endpoints<a name="use-vpc-endpoints-with-codebuild"></a>

 You can improve the security of your builds by configuring AWS CodeBuild to use an interface VPC endpoint\. Interface endpoints are powered by PrivateLink, a technology that enables you to privately access Amazon EC2 and CodeBuild by using private IP addresses\. PrivateLink restricts all network traffic between your managed instances, CodeBuild, and Amazon EC2 to the Amazon network\. \(Managed instances don't have access to the internet\.\) Also, you don't need an internet gateway, NAT device, or virtual private gateway\. You are not required to configure PrivateLink, but it's recommended\. For more information about PrivateLink and VPC endpoints, see [Accessing AWS Services Through PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html#what-is-privatelink)\. 

## Before You Create VPC Endpoints<a name="vpc-endpoints-before-you-begin"></a>

 Before you configure VPC endpoints for AWS CodeBuild, be aware of the following restrictions and limitations\. 

**Note**  
 Use an [Amazon VPC NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html) if you want to use CodeBuild with AWS services that do not support Amazon VPC PrivateLink connections\. 
+  VPC endpoints support Amazon\-provided DNS through Amazon Route 53 only\. If you want to use your own DNS, you can use conditional DNS forwarding\. For more information, see [DHCP Option Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) in the *Amazon VPC User Guide*\. 
+  VPC endpoints currently do not support cross\-region requests\. Make sure that you create your endpoint in the same region as any Amazon S3 buckets that store your build input and output\. You can use the Amazon S3 console or the [ get\-bucket\-location](https://docs.aws.amazon.com/cli/latest/reference/s3api/get-bucket-location.html) command to find the location of your bucket\. Use a region\-specific Amazon S3 endpoint to access your bucket \(for example, `mybucket.s3-us-west-2.amazonaws.com`\)\. For more information about region\-specific endpoints for Amazon S3, see [Amazon Simple Storage Service](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) in Amazon Web Services General Reference*Amazon Web Services General Reference*\. If you use the AWS CLI to make requests to Amazon S3, set your default region to the same region as your bucket, or use the `--region` parameter in your requests\.

## Creating VPC Endpoints for CodeBuild<a name="creating-vpc-endpoints"></a>

Use [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) to create the endpoint `com.amazonaws.region.codebuild`\. This is a VPC endpoint for AWS CodeBuild\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/vpc-endpoint.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

 *region* represents the region identifier for an AWS Region supported by CodeBuild, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported regions, see [CodeBuild](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the * AWS General Reference*\. The endpoint is prepopulated with the region you specified when you signed in to AWS\. If you change your region, the VPC endpoint is updated accordingly\. 

## Create a VPC Endpoint Policy for CodeBuild<a name="creating-vpc-endpoint-policy"></a>

 You can create a policy for Amazon VPC endpoints for AWS CodeBuild in which you can specify:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources that can have actions performed on them\. 

The following example policy specifies that all principals can only start and view builds for the `project-name` project\. 

```
{
    "Statement": [
        {
            "Action": [
                "codebuild:ListBuildsForProject",
                "codebuild:StartBuild",
                "codebuild:BatchGetBuilds"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:codebuild:region-ID:account-ID:project/project-name",
            "Principal": "*"
        }
    ]
}
```

 For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html)\. 