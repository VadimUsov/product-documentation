---
title: connect-aws-cli-s3cmd-and-aws-sdk
displayName: Connect external command lines
published: true
order: 10
toc:
   --1--AWS CLI: "aws-cli"
   --2--Install: "install-aws-cli"
   --2--Configure CLI: "configure-your-storage-for-use-with-aws-cli"
   --1--S3cmd: "s3cmd"
   --2--Install S3cmd: "install-s3cmd"
   --2--Configure S3cmd: "configure-your-storage-for-use-with-s3cmd"
   --3--With wizard: "configure-with-the-wizard"
   --3--Without wizard: "configure-without-the-wizard"
   --1--AWS SDK: "aws-sdk"
   --2--Connect: "connect-aws-sdk"
   --2--Manage your storage: "manage-storage-with-aws-sdk"
   --3--Configure and create a bucket: "configure-storage-and-create-an-s3-bucket"
   --3--Set CORS policy: "set-cors-policy-on-a-bucket"
   --3--Add objects: "add-objects-to-a-bucket"
   --3--Get a list of objects: "get-a-list-of-objects"
---
With [Gcore Object Storage](https://www.gcore.com/storage), you can utilize S3-compatible software, such as AWS CLI and S3cmd.

  
    •  
    •  
  
    •  
    •  
        •  
        •  
  
    •  
    •  
        •        •  
        •  
        •   

AWS CLI 
--------

AWS CLI, or the AWS Command Line Interface, is the software for managing AWS services and has been integrated with our Storage for managing your buckets using commands from [Amazon's official documentation](https://docs.aws.amazon.com/cli/latest/reference/).

### Install AWS CLI 

1\. Follow the instructions in the [Amazon article](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) to install the latest version of the AWS CLI.

2\. To verify that the installation was successful, run the _aws --version_ command in the terminal.

$ aws --version 

The installation was successful if you see the version and your operating system in the output. For example:

aws-cli/2.7.24 Python/3.8.8 Windows/10 exe/AMD64 prompt/off 

If the terminal cannot find the "aws" command, there may have been an issue during the installation. Please refer to the "[Troubleshooting AWS CLI errors](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-troubleshooting.html)" guide for assistance.

### Configure your storage for use with AWS CLI

1\. Enter the _aws configure_ command, and a configuration wizard will be launched.

2\. You need to provide the following information:

*   **Access Key**: Enter the access key you received after creating the storage in your account.
*   **Secret Key**: Enter the secret key you received after creating the storage in your account.
*   **Default region name**: Enter your [storage region](https://www.gcore.com/support/articles/360002112138/), for example, _s-ed1_.

**Note**: Do not change the other parameters.

3\. To verify the configuration was successful, run the following command:

$ aws s3 ls --endpoint-url=https://s-ed1.cloud.gcore.lu

Replace _https://s-ed1.cloud.gcore.lu_ with your storage endpoint, which can be found in the "[Service URL. S3 service URLs and default region names](https://www.gcore.com/support/articles/360002112138/)" guide.

Since there are no buckets in the storage, the command will not list anything. However, if there are no errors, you have configured the credentials correctly.

S3cmd
-----

S3cmd is a free command line tool for uploading, retrieving, and managing data in Amazon S3 and other cloud storage services. Please use the commands described in the [official S3cmd documentation](https://s3tools.org/usage).

### Install S3cmd 

1\. Go to the "[Download](https://s3tools.org/download)" section in the S3cmd documentation.

2\. Download the repository for your operating system.

3\. Install the latest version of S3cmd.

### Configure your storage for use with S3cmd

#### **Configure with the wizard**

1\. Enter the following command to launch the configuration wizard:

$ s3cmd --configure

2\. You need to specify the following information:

*   **Access Key**: Enter the access key you received after creating the storage in your account.
*   **Secret Key**: Enter the secret key you received after creating the storage in your account.
*   **S3 Endpoint**: Enter your [storage URL](https://www.gcore.com/support/articles/360002112138/), for example, _s-ed1.cloud.gcore.lu_.
*   **DNS-style bucket+hostname**: Enter your storage URL, for example, _s-ed1.cloud.gcore.lu_. Optionally specify the port template for accessing a bucket in the format: _s-ed1.cloud.gcore.lu:80_.

**Note**: Do not change the other parameters.

S3cmd will try to connect to your storage. If the data is entered correctly, you'll receive the message:

Success. Your access key and secret key worked fine :-)

S3cmd will save the entered information in the _~/.s3cfg_ file in the following format:

\[default\]  
access\_key = 123ABC456DEF...  
secret\_key = EXAMPLE...  
host\_base = s-ed1.cloud.gcore.lu  
host\_bucket = s-ed1.cloud.gcore.lu

#### **Configure without the wizard**

This method allows you to enter configuration data in one line without using a wizard.

Enter the command:

s3cmd --access\_key 12\*\*\*\*\*6DEF --secret\_key EX\*\*\*\*\* --host s-ed1.cloud.gcore.lu --host-bucket s-ed1.cloud.gcore.lu

Where:

*   _12\*\*\*\*\*6DEF_ is the access key you received after creating the storage in your account.
*   _EXAMPLE\*\*\*\*\*_ is the secret key you received after creating the storage in your account.
*   _s-ed1.cloud.gcore.lu_ is the [storage URL](https://www.gcore.com/support/articles/360002112138/).

AWS SDK
-------

The methods described below are relevant for [AWS JavaScript SDK version 2.742.0](https://github.com/aws/aws-sdk-js/releases/tag/v2.742.0).

### Connect AWS SDK

To connect the interface with your storage, add AWS SDK to your HTML page according to the following example:

<html>   
    <head>   
        <script src="[https://sdk.amazonaws.com/js/aws-sdk-2.742.0.min.js](https://sdk.amazonaws.com/js/aws-sdk-2.742.0.min.js)"></script>   
        <script src="./js/index2.js"></script>   
    </head>   
    <body>   
        <h1>List of files</h1>   
        <ul id="list">   
        </ul>   
    </body>   
</html> 

### Manage storage with AWS SDK

#### **Configure storage and create an S3 bucket**

Open the configuration file (_./js/index2.js_) and specify the data of your storage and future bucket:

**var** s3BucketName = "test";   
**var** host = "https://s-ed1.cloud.gcore.lu";   
**var** access\_key = "1234";   
**var** secret\_key = "5678";   
    
AWS.config.accessKeyId = access\_key;   
AWS.config.secretAccessKey = secret\_key;   
AWS.config.endpoint = host;   
  
**var** s3 = **new** AWS.S3({   
    sslEnabled: **true**   
  }); 

Where:

*   _test_ is the name of the future bucket;
*   _https://s-ed1.cloud.gcore.lu_ is the storage URL;
*   _1234_ is the access key you received when creating the storage in the Control panel.
*   _5678_ is the secret key you received when creating the storage in the Control panel.

#### **Set CORS policy on a bucket**

Below is the wildcard policy example. It describes a configuration that allows cross-origin GET HEAD PUT, POST, and DELETE requests from all sources.

**Note**: This configuration can be insecure.

<CORSConfiguration>   
<CORSRule>   
    <ID>Allow   
    everything</ID>   
    <AllowedOrigin>\*</AllowedOrigin>   
    <AllowedMethod>GET</AllowedMethod>   
    <AllowedMethod>HEAD</AllowedMethod>   
    <AllowedMethod>PUT</AllowedMethod>   
    <AllowedMethod>POST</AllowedMethod>   
    <AllowedMethod>DELETE</AllowedMethod>   
    <AllowedHeader>\*</AllowedHeader>   
    <MaxAgeSeconds>30</MaxAgeSeconds>   
</CORSRule>   
</CORSConfiguration>

After the configuration is completed, apply it to the bucket with the following command:

s3cmd setcors cors.xml s3://<bucket\_name> 

#### **Add objects to a bucket**

The example shows how to add two objects (_test\_file1_ and _test\_file2_) to the _s3test_ bucket.

**var** params1 = {   
   Bucket: s3test, Key: 'test\_file1',   
   Body: "test"   
 };   
**var** params2 = {   
   Bucket: s3test, Key: 'test\_file2',   
   Body: "test"   
 };   
 **var** request = s3.putObject(params1);   
 request.send(**function** (err, data) {   
   **if** (err) console.log("Error:", err.code, err.message);   
   **else** console.log(data);   
 });   
 **var** request = s3.putObject(params2);   
 request.send(**function** (err, data) {   
   **if** (err) console.log("Error:", err.code, err.message);   
   **else** console.log(data);   
 }); 

#### **Get a list of objects**

The example shows how to get a list of files in the _test\_2_ bucket.

params = {   
    Bucket: "test\_2"   
   };   
s3.listObjects(params, **function**(err, data) {   
    **if** (err) **return**;   
      data.Contents.map(**function**(info){   
        **var** ul = document.getElementById("list");   
        **var** li = document.createElement("li");   
        li.innerText = info.Key + " " + info.LastModified;   
        ul.append(li);   
      });   
  });