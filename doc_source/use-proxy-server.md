# Use CodeBuild with a Proxy Server<a name="use-proxy-server"></a>

 You can use AWS CodeBuild with a proxy server to regulate HTTP and HTTPS traffic to and from the internet\. To run CodeBuild with a proxy server, you install a proxy server in a public subnet and CodeBuild in a private subnet in an Amazon Virtual Private Cloud \(Amazon VPC\)\. 

There are two primary use cases for running CodeBuild in a proxy server: 
+  It eliminates the use of a NAT gateway or NAT instance in your Amazon VPC\. 
+  It lets you specify the URLs that instances in the proxy server can access and the URLs to which the proxy server denies access\.

 You can use CodeBuild with two types of proxy servers\. For both, the proxy server runs in a public subnet and CodeBuild runs in a private subnet\. 
+  **Explicit proxy**: If you use an explicit proxy server, you must configure `NO_PROXY`, `HTTP_PROXY`, and `HTTPS_PROXY` environment variables in CodeBuild at the project level\. For more information, see [Change a Build Project's Settings in CodeBuild ](change-project.md) and [Create a Build Project in CodeBuild](create-project.md)\. 
+  **Transparent proxy**: If you use a transparent proxy server, no special configuration is required\. 

**Topics**
+ [Components Required to Run CodeBuild in a Proxy Server](#use-proxy-server-transparent-components)
+ [Run CodeBuild in an Explicit Proxy Server](#run-codebuild-in-explicit-proxy-server)
+ [Run CodeBuild in a Transparent Proxy Server](#run-codebuild-in-transparent-proxy-server)
+ [Run a Package Manager and Other Tools in a Proxy Server](#use-proxy-server-tools)

## Components Required to Run CodeBuild in a Proxy Server<a name="use-proxy-server-transparent-components"></a>

 You need these components to run AWS CodeBuild in a transparent or explicit proxy server: 
+  An Amazon VPC\. 
+  One public subnet in your Amazon VPC for the proxy server\. 
+  One private subnet in your Amazon VPC for CodeBuild\. 
+  An internet gateway that allows communcation between the VPC and the internet\. 

 The following diagram shows how the components interact\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/images/codebuild-proxy-transparent.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/codebuild/latest/userguide/)

### Set Up a VPC, Subnets, and a Network Gateway<a name="use-proxy-server-transparent-setup"></a>

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

## Run CodeBuild in an Explicit Proxy Server<a name="run-codebuild-in-explicit-proxy-server"></a>

**Topics**
+ [Configure Squid as an Explicit Proxy Server](#use-proxy-server-explicit-squid-configure)
+ [Create a CodeBuild Project](#use-proxy-server-explicit-create-acb-project)
+ [Explicit Proxy Server Sample `Squid.conf` File](#use-proxy-server-explicit-sample-squid-conf)

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

   Add the following in place of the default ACL rules you removed\. The first line allows requests from your Amazon VPC\. The next two lines grant your proxy server access to destination URLs that might be used by AWS CodeBuild\. Modify the regular expression in the last line to specify Amazon S3 buckets or a CodeCommit repository in an AWS Region\. For example:
  + If your source is Amazon S3, use the command acl download\_src dstdom\_regex \.\*s3\\\.us\-west\-1\\\.amazonaws\\\.comto grant access to Amazon S3 buckets in the `us-west-1` Region\.
  +  If your source is AWS CodeCommit, use `git-codecommit.<your-region>.amazonaws.com` to whitelist an AWS Region\. 

  ```
  acl localnet src 10.1.0.0/16 #Only allow requests from within the VPC
  acl allowed_sites dstdomain .github.com #Allows to download source from GitHub
  acl allowed_sites dstdomain .bitbucket.com #Allows to download source from Bitbucket
  acl download_src dstdom_regex .*\.amazonaws\.com #Allows to download source from Amazon S3 or CodeCommit
  ```
+  Replace `http_access allow localnet` with the following: 

  ```
  http_access allow localnet allowed_sites
  http_access allow localnet download_src
  ```
+  For allowing CodeBuild to upload logs and artifacts. There are two methods:

1.  Before the `http_access deny all` statement, insert the following statements\. They allow CodeBuild to access CloudWatch and Amazon S3\. Access to CloudWatch is required so that CodeBuild can create CloudWatch logs\. Access to Amazon S3 is required for uploading artifacts and Amazon S3 caching\. 

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
    After you save `squid.conf`, execute the following: 

    ```
    sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 3130
    sudo service squid restart
    ```

1.  Add proxy configuration to your buildspec file\.

    ```
    version: 0.2
    proxy:
      upload-artifacts: yes
      logs: yes
    phases:
      build:
        commands:
          - command
    ```

**Note**  
If you receive a RequestError timeout error, see [ RequestError timeout error when running CodeBuild in a proxy server](troubleshooting.md#code-request-timeout-error)\.

For more information, see [Explicit Proxy Server Sample `Squid.conf` File](#use-proxy-server-explicit-sample-squid-conf) later in this topic\.

### Create a CodeBuild Project<a name="use-proxy-server-explicit-create-acb-project"></a>

 To run AWS CodeBuild with your explicit proxy server, set its `HTTP_PROXY` and `HTTPS_PROXY` environment variables with the private IP address of the Amazon EC2 instance you created for your proxy server and port 3128 at the project level\. The private IP address looks like `http://your-ec2-private-ip-address:3128`\. For more information, see [Create a Build Project in CodeBuild](create-project.md) and [Change a Build Project's Settings in CodeBuild ](change-project.md)\.

 Use the following command to view the Squid proxy access log: 

```
sudo tail -f /var/log/squid/access.log
```

### Explicit Proxy Server Sample `Squid.conf` File<a name="use-proxy-server-explicit-sample-squid-conf"></a>

 The following is an example of how a `squid.conf` file that is configured for an explicit proxy server might look\. 

```
  acl localnet src 10.0.0.0/16 #Only allow requests from within the VPC
  # add all URLS to be whitelisted for download source and commands to be executed in build environment
  acl allowed_sites dstdomain .github.com    #Allows to download source from github
  acl allowed_sites dstdomain .bitbucket.com #Allows to download source from bitbucket
  acl allowed_sites dstdomain ppa.launchpad.net #Allows to execute apt-get in build environment
  acl download_src dstdom_regex .*\.amazonaws\.com #Allows to download source from S3 or CodeCommit
  acl SSL_ports port 443
  acl Safe_ports port 80		# http
  acl Safe_ports port 21		# ftp
  acl Safe_ports port 443		# https
  acl Safe_ports port 70		# gopher
  acl Safe_ports port 210		# wais
  acl Safe_ports port 1025-65535	# unregistered ports
  acl Safe_ports port 280		# http-mgmt
  acl Safe_ports port 488		# gss-http
  acl Safe_ports port 591		# filemaker
  acl Safe_ports port 777		# multiling http
  acl CONNECT method CONNECT
  #
  # Recommended minimum Access Permission configuration:
  #
  # Deny requests to certain unsafe ports
  http_access deny !Safe_ports
  # Deny CONNECT to other than secure SSL ports
  http_access deny CONNECT !SSL_ports
  # Only allow cachemgr access from localhost
  http_access allow localhost manager
  http_access deny manager
  # We strongly recommend the following be uncommented to protect innocent
  # web applications running on the proxy server who think the only
  # one who can access services on "localhost" is a local user
  #http_access deny to_localhost
  #
  # INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
  #
  # Example rule allowing access from your local networks.
  # Adapt localnet in the ACL section to list your (internal) IP networks
  # from where browsing should be allowed
  http_access allow localnet allowed_sites
  http_access allow localnet download_src
  http_access allow localhost
  # Add this for CodeBuild to access CWL end point, caching and upload artifacts S3 bucket end point
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
  # And finally deny all other access to this proxy
  http_access deny all
  # Squid normally listens to port 3128
  http_port 3128
  # Uncomment and adjust the following to add a disk cache directory.
  #cache_dir ufs /var/spool/squid 100 16 256
  # Leave coredumps in the first cache dir
  coredump_dir /var/spool/squid
  #
  # Add any of your own refresh_pattern entries above these.
  #
  refresh_pattern ^ftp:		1440	20%	10080
  refresh_pattern ^gopher:	1440	0%	1440
  refresh_pattern -i (/cgi-bin/|\?) 0	0%	0
  refresh_pattern .		0	20%	4320
```

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

 Incoming requests from instances in the private subnet must redirect to the Squid ports\. Squid listens on port 3129 for HTTP traffic \(instead of 80\) and 3130 for HTTPS traffic \(instead of 443\)\. Use the iptables command to properly route traffic: 

```
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 3129
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 3130
sudo service iptables save
sudo service squid start
```

### Create a CodeBuild Project<a name="use-proxy-server-transparent-create-acb-project"></a>

 After you configure your proxy server, you can use it with AWS CodeBuild in a private subnet without more configuration\. Every HTTP and HTTPS request goes through the public proxy server\. Use the following command to view the Squid proxy access log: 

```
sudo tail -f /var/log/squid/access.log
```

## Run a Package Manager and Other Tools in a Proxy Server<a name="use-proxy-server-tools"></a>

 To execute a tool, such as a package manager, in a proxy server:

1.  Whitelist the tool in your proxy server by adding statements to your `squid.conf` file\. 

1.  Add a line to your buildspec file that points to the private endpoint of your proxy server\. 

 The following examples demonstrate how to do this for `apt-get`, `curl`, and `maven`\. If you use a different tool, use the same concepts by whitelisting it in the `squid.conf` file and adding a command to your buildspec file to make CodeBuild aware of your proxy server's endpoint\. 

**Run `apt-get` in a Proxy Server**

1. Add the following statements to your `squid.conf` file to whitelist `apt-get` in your proxy server\. The first three lines allow `apt-get` to execute in the build environment\.

   ```
   acl allowed_sites dstdomain ppa.launchpad.net # Required for apt-get to execute in the build environment
   acl apt_get dstdom_regex .*\.launchpad.net # Required for CodeBuild to execute apt-get in the build environment
   acl apt_get dstdom_regex .*\.ubuntu.com    # Required for CodeBuild to execute apt-get in the build environment
   http_access allow localnet allowed_sites
   http_access allow localnet apt_get
   ```

1. Add the following statement in your buildspec file so that `apt-get` commands look for the proxy configuration in `/etc/apt/apt.conf.d/00proxy`\.

   ```
   echo 'Acquire::http::Proxy "http://<private-ip-of-proxy-server>:3128"; Acquire::https::Proxy "http://<private-ip-of-proxy-server>:3128"; Acquire::ftp::Proxy "http://<private-ip-of-proxy-server>:3128";' > /etc/apt/apt.conf.d/00proxy
   ```

**Run `curl` in a Proxy Server**

1.  Add the following to your `squid.conf` file to whitelist `curl` in your build environment\. 

   ```
   acl allowed_sites dstdomain ppa.launchpad.net # Required to execute apt-get in the build environment
   acl allowed_sites dstdomain google.com # Required for access to a webiste. This example uses www.google.com.
   http_access allow localnet allowed_sites
   http_access allow localnet apt_get
   ```

1.  Add the following statement in your buildspec file so `curl` uses the private proxy server to access the website you added to the `squid.conf`\. In this example, the website is `google.com`\. 

   ```
   curl -x <private-ip-of-proxy-server>:3128 https://www.google.com
   ```

**Run `maven` in a Proxy Server**

1.  Add the following to your `squid.conf` file to whitelist `maven` in your build environment\. 

   ```
   acl allowed_sites dstdomain ppa.launchpad.net # Required to execute apt-get in the build environment
   acl maven dstdom_regex .*\.maven.org # Allows access to the maven repository in the build environment
   http_access allow localnet allowed_sites
   http_access allow localnet maven
   ```

1. Add the following statement to your buildspec file\. 

   ```
   maven clean install -DproxySet=true -DproxyHost=<private-ip-of-proxy-server> -DproxyPort=3128
   ```