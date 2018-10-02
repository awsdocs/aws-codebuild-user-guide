# Use VPC Endpoints<a name="use-vpc-endpoints-with-codebuild"></a>

 You can improve the security of your builds by configuring AWS CodeBuild to use an interface VPC endpoint\. Interface endpoints are powered by PrivateLink, a technology that enables you to privately access Amazon EC2 and AWS CodeBuild by using private IP addresses\. PrivateLink restricts all network traffic between your managed instances, AWS CodeBuild, and Amazon EC2 to the Amazon network \(managed instances don't have access to the internet\)\. Also, you don't need an Internet gateway, a NAT device, or a virtual private gateway\. You are not required to configure PrivateLink, but it's recommended\. For more information about Private Link and VPC endpoints, see [ Accessing AWS Services Through PrivateLink ](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html#what-is-privatelink)\. 

## Before You Create VPC Endpoints<a name="vpc-endpoints-before-you-begin"></a>

 Before you configure VPC endpoints for AWS CodeBuild, be aware of the following restrictions and limitations\. 

**Note**  
 The following services must communicate with the internet\. You can use VPC endpoints with AWS CodeBuild and these services with an [Amazon VPC NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html)\.   
 AWS CodeCommit, which might be a source repository\. 
 Amazon ECR, which might be used with a custom Docker image\. 
 Active Directory\. 
 Amazon CloudWatch Events and Amazon CloudWatch Logs\. 
+  VPC endpoints only support Amazon\-provided DNS through Amazon Route 53\. If you want to use your own DNS, you can use conditional DNS forwarding\. For more information, see [DHCP Option Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html) in the Amazon VPC User Guide\. 
+  VPC endpoints currently do not support cross\-region requests\. Ensure that you create your endpoint in the same region as any Amazon S3 buckets that store your build input and output\. You can find the location of your bucket by using the by using the Amazon S3 console, or by using the [ get\-bucket\-location](https://docs.aws.amazon.com/cli/latest/reference/s3api/get-bucket-location.html) command\. Use a region\-specific Amazon S3 endpoint to access your bucket; for example, `mybucket.s3-us-west-2.amazonaws.com`\. For more information about region\-specific endpoints for Amazon S3, see [Amazon Simple Storage Service \(Amazon S3\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) in Amazon Web Services General Reference*Amazon Web Services General Reference*\. If you use the AWS CLI to make requests to Amazon S3, set your default region to the same region as your bucket, or use the `--region` parameter in your requests\.

## Creating VPC Endpoints for AWS CodeBuild<a name="creating-vpc-endpoints"></a>

Use [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) to create the endpoint ** com\.amazonaws\.*region*\.codebuild**\. This is a VPC endpoint for the AWS CodeBuild service\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/vpc-endpoint.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

 **region** represents the region identifier for an AWS region supported by AWS CodeBuild, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported **region** values, see the Region column in the [ AWS CodeBuild table of regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#codebuild_region) in the * AWS General Reference*\. The endpoint is prepopulated with the region you specified when you logged into AWS\. If you change your region, then the VPC endpoint will update with the new region\. 