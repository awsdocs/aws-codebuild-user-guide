# Use CodeBuild with a Proxy Server<a name="use-proxy-server"></a>

 You can use AWS CodeBuild with a proxy server to regulate HTTP and HTTPS traffic to and from the internet\. To run CodeBuild with a proxy server, you install a proxy server in a public subnet and CodeBuild in a private subnet in an Amazon Virtual Private Cloud \(Amazon VPC\)\. 

There are two primary use cases for running CodeBuild in a proxy server: 
+  It eliminates the use of a NAT gateway or NAT instance in your Amazon VPC\. 
+  It lets you specify the URLs that instances in the proxy server can access and the URLs to which the proxy server denies access\.

 You can use CodeBuild with two two types of proxy servers\. For both, the proxy server runs in a public subnet and CodeBuild runs in a private subnet\. 
+  **Transparent proxy**: If you use a transparent proxy server, CodeBuild does not know that it is communicating with a proxy server, so it does not require special configuration\. 
+  **Explicit proxy**: If you use an explicit proxy server, CodeBuild is aware of the proxy server and you must configure `HTTP_PROXY` and `HTTPS_PROXY` environment variables in CodeBuild\. 

**Topics**
+ [Components Required to Run CodeBuild in a Proxy Server](#use-proxy-server-transparent-components)
+ [Run CodeBuild in a Transparent Proxy Server](#run-codebuild-in-transparent-proxy-server)
+ [Run CodeBuild in an Explicit Proxy Server](#run-codebuild-in-explicit-proxy-server)

## Components Required to Run CodeBuild in a Proxy Server<a name="use-proxy-server-transparent-components"></a>

 You need these components to run AWS CodeBuild in a transparent or explicit proxy server: 
+  An Amazon VPC\. 
+  One public subnet in your Amazon VPC for the proxy server\. 
+  One private subnet in your Amazon VPC for CodeBuild\. 
+  An internet gateway that allows communcation between the VPC and the internet\. 

 The following diagram shows how the components interact\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/codebuild-proxy-transparent.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

### Set Up an Amazon VPC, Subnets, and an API Gateway<a name="use-proxy-server-transparent-setup"></a>

 The following steps are required to run AWS CodeBuild in a transparent or explicit proxy server\. 

1. Create a VPC\. For information, see [Creating a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC)\.

1. Create two subnets in your VPC\. One is a public subnet named Public Subnet in which your proxy server runs\. The other is a private subnet named Private Subnet in which CodeBuild runs\. 

   For information, see [Creating a Subnet in Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet)\.

1.  Create and attach an internet gateway to your VPC\. For more information, see [Creating and Attaching an Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html#Add_IGW_Attach_Gateway)\. 

1.  Add a rule to the default route table that routes outgoing traffic from the VPC \(0\.0\.0\.0/0\) to the internet gateway\. For information, see [Adding and Removing Routes from a Route Table](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html#AddRemoveRoutes)\. 

1.  Add a rule to the default security group of your VPC that allows ingress SSH traffic \(TCP 22\) from your VPC \(0\.0\.0\.0/0\)\. 

1.  Follow the instructions in [Launching an Instance Using the Launch Instance Wizard](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) to launch an Amazon Linux Amazon EC2 instance\. When you run the wizard, choose the following options: 
   +  In **Choose an Instance Type**, choose an Amazon Linux Amazon Machine Image \(AMI\)\. 
   +  In **Subnet**, choose the public subnet you created earlier in this topic\. If you used the suggested name, it is **Public Subnet**\. 
   +  In **Auto\-assign Public IP**, choose **Enable**\. 
   +  On the **Configure Security Group** page, for **Assign a security group**, choose **Select an existing security group**\. Next, choose the default security group\. 
   +  After you choose **Launch**, choose an existing key pair or create one\. 

    Choose the default settings for all other options\. 

1.  After your Amazon EC2 instance is running, disable source/destination checks\. For information, see [Disabling Source/Destination Checks](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html#EIP_Disable_SrcDestCheck)\. 

1.  Create a route table in your VPC\. Add a rule to the route table that routes traffic destined for the internet to your proxy server\. Associate this route table with your private subnet\. This is required so that outbound requests from instances in your private subnet, where CodeBuild runs, is always routed through the proxy server\. 

### Install and Configure a Proxy Server<a name="use-proxy-server-squid-install"></a>

 There are many proxy servers from which to choose\. An open\-source proxy server, Squid, is used here to demonstrate how AWS CodeBuild runs in a proxy server\. You can apply the same concepts to other proxy servers\. 

 To install Squid, use a yum repo by running the following commands: 

```
sudo yum update -y
sudo yum install -y squid
```

 After you install Squid, you edit its `squid.conf` file\. Instructions for editing this file are included later in this topic\. 

### Configure Squid for HTTPS Traffic<a name="use-proxy-server-squid-configure-https"></a>

 For HTTPS, the HTTP traffic is encapsulated in a Transport Layer Security \(TLS\) connection\. Squid uses a feature called [SslPeekAndSplice](https://wiki.squid-cache.org/Features/SslPeekAndSplice) to retrieve the Server Name Indication \(SNI\) from the TSL initiation that contains the requested internet host\. This is required so Squid does not need to unencrypt HTTPS traffic\. To enable SslPeekAndSplice, Squid requires a certificate\. Create this certificate using OpenSSL: 

```
sudo mkdir /etc/squid/ssl
cd /etc/squid/ssl
sudo openssl genrsa -out squid.key 2048
sudo openssl req -new -key squid.key -out squid.csr -subj "/C=XX/ST=XX/L=squid/O=squid/CN=squid"
sudo openssl x509 -req -days 3650 -in squid.csr -signkey squid.key -out squid.crt
sudo cat squid.key squid.crt | sudo tee squid.pem
```

**Note**  
 For HTTP, Squid does not require configuration\. From all HTTP/1\.1 request messages, it can retrieve the host header field, which specifies the internet host that is being requested\. 

## Run CodeBuild in a Transparent Proxy Server<a name="run-codebuild-in-transparent-proxy-server"></a>

 To run AWS CodeBuild in a transparent proxy server, you must configure the proxy server with access to the websites and domains it interacts with\. 

### Configure Squid as a Transparent Proxy Server<a name="use-proxy-server-transparent-squid-configure"></a>

 To configure a proxy server to be transparent, you must grant it access to the domains and websites you want it to access\. To run AWS CodeBuild with a transparent proxy server, you must grant it access to amazonaws\.com\. You must also grant access to other websites CodeBuild uses\. These vary depending on how you create your CodeBuild projects\. Example websites are those for repositories such as GitHub, Bitbucket, Yum, and Maven\. To grant Squid access to specific domains and websites, use a command similar to the following to update the `squid.conf` file\. This sample command grants access to amazonaws\.com, github\.com, and bitbucket\.com\. You can edit this sample to grant access to other websites\. 

```
cat | sudo tee /etc/squid/squid.conf ≪EOF
visible_hostname squid
#Handling HTTP requests
http_port 3129 intercept
acl allowed_http_sites dstdomain .amazonaws.com
#acl allowed_http_sites dstdomain domain_name [uncomment this line to add another domain]
http_access allow allowed_http_sites
#Handling HTTPS requests
https_port 3130 cert=/etc/squid/ssl/squid.pem ssl-bump intercept
acl SSL_port port 443
http_access allow SSL_port
acl allowed_https_sites ssl::server_name .amazonaws.com
acl allowed_https_sites ssl::server_name .github.com
acl allowed_https_sites ssl::server_name .bitbucket.com
#acl allowed_https_sites ssl::server_name [uncomment this line to add another website]
acl step1 at_step SslBump1
acl step2 at_step SslBump2
acl step3 at_step SslBump3
ssl_bump peek step1 all
ssl_bump peek step2 allowed_https_sites
ssl_bump splice step3 allowed_https_sites
ssl_bump terminate step2 all
http_access deny all
EOF
```

 Incoming requests from instances in the private subnet must redirect to the Squid ports\. Squid listens on port 3129 for HTTP traffic \(instead of 40\) and 3130 for HTTPS traffic \(instead of 443\)\. Use the iptables command to properly route traffic: 

```
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 3129
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 3130
sudo service iptables save
sudo service squid start
```

### Create a CodeBuild Project<a name="use-proxy-server-transparent-create-acb-project"></a>

 After you configure your proxy server, you can use it with AWS CodeBuild in a private subnet without additional configuration\. Every HTTP and HTTPS request goes through the public proxy server\. Use the following command to view the Squid proxy access log: 

```
sudo tail -f /var/log/squid/access.log
```

## Run CodeBuild in an Explicit Proxy Server<a name="run-codebuild-in-explicit-proxy-server"></a>

 To run AWS CodeBuild with in an explicit proxy server, you must configure the proxy server to allow or deny traffic to and from external sites, and then configure the `HTTP_PROXY` and `HTTPS_PROXY` environment variables\. 

### Configure Squid as an Explicit Proxy Server<a name="use-proxy-server-explicit-squid-configure"></a>

 To configure the Squid proxy server to be explicit, you must make the following modifications to its `/etc/squid/squid.conf` file: 
+  Remove the following default access control list \(ACL\) rules\. 

  ```
  acl localnet src 10.0.0.0/8     
  acl localnet src 172.16.0.0/12  
  acl localnet src 192.168.0.0/16 
  acl localnet src fc00::/7       
  acl localnet src fe80::/10
  ```

   Add the following in place of the default ACL rules you removed\. The first line allows requests from your Amazon VPC\. The next two lines grant your proxy server access to destination URLs that might be used by AWS CodeBuild\. Modify the regular expression in the last line to specify Amazon S3 buckets in a specifc region\. For example, use the command `acl download_src dstdom_regex .*s3\.us-west-1\.amazonaws\.com` to grant access to Amazon S3 buckets in the `us-west-1` region\.

  ```
  acl localnet src 10.1.0.0/16 #Only allow requests from within the VPC
  acl allowed_sites dstdomain .github.com
  acl allowed_sites dstdomain .bitbucket.com
  acl download_src dstdom_regex .*\.amazonaws\.com
  ```
+  Replace `http_access allow localnet` with the following: 

  ```
  http_access allow localnet allowed_sites
  http_access allow localnet download_src
  ```
+  Insert the following in the `squid.conf` file before the `http_access deny all` statement: 

  ```
  https_port 3130 cert=/etc/squid/ssl/squid.pem ssl-bump intercept
  acl SSL_port port 443
  http_access allow SSL_port
  acl allowed_https_sites ssl::server_name .amazonaws.com
  acl step1 at_step SslBump1
  acl step2 at_step SslBump2
  acl step3 at_step SslBump3
  ssl_bump peek step1 all
  ssl_bump peek step2 allowed_https_sites
  ssl_bump splice step3 allowed_https_sites
  ssl_bump terminate step2 all
  ```
+  Execute the following commands: 

  ```
  sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 3130
  sudo service squid restart
  ```

### Create a CodeBuild Project<a name="use-proxy-server-explicit-create-acb-project"></a>

 To run AWS CodeBuild with your explicit proxy server, set its `HTTP_PROXY` and `HTTPS_PROXY` environment variables with the private IP address of the Amazon EC2 instance you created for your proxy server and port 3128: 

```
$ export HTTP_PROXY=http://your-ec2-private-ip-address:3128
$ export HTTPS_PROXY=http://your-ec2-private-ip-address:3128
```

 Use the following command to view the Squid proxy access log: 

```
sudo tail -f /var/log/squid/access.log
```