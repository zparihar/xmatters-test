Test for Xmatters



This script is designed to spin up Instances in AWS EC2 when given specific parameters.
When fired up, it will use the AWS CLI (version 1.2.9) which is contained in most Linux Distribution Repositories.  AWS CLI is used to make API Calls to Amazon Web Services.
If you do not have awscli on your machine and you do not have it in the repository, please run the supplied Install script in the directory 'bash-version'.  It will prompt you to add your AWS ACCESS KEY and AWS Secret Key".

The tool takes in user supplied parameters where it does error checking.  I've currently limited the number of instances that you can spin up at a time to 10 and also limited the storage space to 10GB's.  All Instances are running Centos 7 - 64bit AMI's.  Since this is already Centos 7, Python version 2.7 is installed (it is also already specified in the Puppet configuration).

It attaches an EBS Block device to the instance.  The instance then formats the EBS Block into XFS and mounts it on the /data directory.  This is configured via a User Data script and also Puppet.

The user data script is auto generated and sent to all the instances for auto configuration.  Some of the configuration in the user data script is generated depending on the user input parameters.  The user data script prepares the instance by installing 'puppet' and 'git'.  It then proceeds to check out a Puppet Configuration from GitHub.  Puppet is in a masterless set up, and it configures the instances differently depending on whether you wanted it to be a PRODUCTION or DEVELOPMENT environment (specifed on the command line).   In the Instances that have been configured, the only real difference between PRODUCTIO and DEVELOPMENT is in the CRONTAB script.

A Ruby user Fact was created so that it can be used by FACTER in order for puppet to determine what type of APP Services would be on these instances.  You can view the Fact in this Launch tool.  Since no specifics were supplied about the type APP was to be used on these instances, I decided to specify 2 application types: 'awsproxyserver' and 'awswebserver'.  The configurations vary little between the 2.  The only difference is that it creates 2 different Django Projects: /home/ec2-user/webapp and /home/ec2-user/proxyapp.

Django is installed via an RPM through Puppet again done through Puppet.

This tool creates KEYPAIR and generates a PEM file and shows you the location of it on screen so that you can use it to log into any of the instances created.  The Public IP address and the Public DNS Names of the instances are provided on screen as well.


This Script was designed to be run on the command line

This Script is written and executed in BASH. 


Cheers,

Zubin
