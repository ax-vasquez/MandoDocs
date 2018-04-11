# Getting Started with VPC
To get a hands-on introduction to Amazon VPC, complete the exercise [Getting Started](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/GetStarted.html).
- The exercise guides you through the steps to
  - Create a nondefault VPC with a public subnet
  - Launch an instance into your subnet

## Additional Resources
Below is a table of useful guides related to Amazon VPC

| Resource | Description |
| ---------- | ------------ |
| [Amazon Virtual Private Cloud Connectivity Options](http://media.amazonwebservices.com/AWS_Amazon_VPC_Connectivity_Options.pdf) | A whitepaper that provides an overview for the options of network connectivity |
| [Amazon VPC Forum](https://forums.aws.amazon.com/forum.jspa?forumID=58) | A community-based forum for discussing technical questions related to Amazon VPC |
| [AWS Developer Resources](http://aws.amazon.com/resources/) | A central starting point to find documentation, code samples, release notes, and other information to help you create innovative applications with AWS |
| [AWS Support Center](https://console.aws.amazon.com/support/home#/) | The home page for AWS support |
| [Contact Us](http://aws.amazon.com/contact-us/) | A central contact point for inquiries concerning AWS billing, accounts, and events |

## Using Amazon VPC with Other AWS Services
Amazon VPC integrates with manu other AWS services; some require a VPC in your account to carry out certain functions. Below are examples of services that use Amazon VPC.

| Service | Relevant Topic |
| -------- | --------------- |
| AWS Data Pipeline | [Launching Resources for Your Pipeline into a VPC](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-resources-vpc.html) |
| Amazon EC2 | [Amazon EC2 and Amazon VPC](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html) |
| Auto Scaling | [Auto Scaling and Amazon VPC](http://docs.aws.amazon.com/autoscaling/latest/userguide/autoscalingsubnets.html) |
| Elastic Beanstalk | [Using AWS Elastic Beanstalk with Amazon VPC](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo-vpc.html) |
| Elastic Load Balancing | [Setting up Elastic Load Balancing](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/setting-up-elb.html) |
| Amazon ElastiCache | [Using ElastiCache with Amazon VPC](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/ManagingVPC.html) |
| Amazon EMR | [Select a Subnet for the Cluster](http://docs.aws.amazon.com/emr/latest/DeveloperGuide/emr-plan-vpc-subnet.html) |
| AWS OpsWorks | [Running a Stack in a VPC](http://docs.aws.amazon.com/opsworks/latest/userguide/workingstacks-vpc.html) |
| Amazon RDS | [Amazon RDS and Amazon VPC](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSVPC.html) |
| Amazon Redshift | [Managing Clusters in a VPC](http://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-vpc.html) |
| Route 53 | [Working with Private Hosted Zones](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zones-private.html) |
| Amazon WorkSpaces | [Create and Configure Your VPC](http://docs.aws.amazon.com/workspaces/latest/adminguide/gsg_create_vpc.html) |

## Accessing Amazon VPC
The VPC can be accessed via the provided web-based user interface, the Amazon VPC console. Additionally, you can access the Amazon VPC by:
1. AWS Command Line Interface (AWS CLI)
    - Provides commands for a broad set of AWS Services
    - Supported on 
      - Windows
      - MacOS
      - Linux/Unix
    - To get started, see [AWS Command Line Interface User Guide](http://docs.aws.amazon.com/cli/latest/userguide/)
    - **For more information about the commands for Amazon VPC, see [ec2](http://docs.aws.amazon.com/cli/latest/reference/ec2/)**
2. AWS Tools for Windows PowerShell
    - Provides commands for a broad set of AWS services for those who script in the PowerShell environment
    - To get started, see [AWS Tools for Windows PowerShell User Guide](http://docs.aws.amazon.com/powershell/latest/userguide/)