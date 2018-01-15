# Setting Up the Amazon Redshift CLI<a name="setting-up-rs-cli"></a>

This section explains how to set up and run the AWS CLI command line tools for use in managing Amazon Redshift\. The Amazon Redshift command line tools run on the AWS Command Line Interface \(AWS CLI\), which in turn uses Python \([http://www\.python\.org/](http://www.python.org)\)\. The AWS CLI can be run on any operating system that supports Python\.

## Installation Instructions<a name="setting-up.installing-the-tools"></a>

To begin using the Amazon Redshift command line tools, you first set up the AWS CLI, and then you add configuration files that define the Amazon Redshift CLI options\.

If you have already installed and configured the AWS CLI for another AWS service, you can skip this procedure\.

**To install the AWS Command Line Interface**

1. Go to [Getting Set Up with the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html), and then follow the instructions for installing the AWS CLI\.

    For CLI access, you need an access key ID and secret access key\. Use IAM user access keys instead of AWS account root user access keys\. IAM lets you securely control access to AWS services and resources in your AWS account For more information about creating access keys, see [How Do I Get Security Credentials?](http://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *AWS General Reference*\. 

1. Create a file containing configuration information such as your access keys, default region, and command output format\. Then set the `AWS_CONFIG_FILE` environment variable to reference that file\. For detailed instructions, go to [Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the AWS Command Line Interface User Guide\.

1. Run a test command to confirm that the AWS CLI interface is working\. For example, the following command should display help information for the AWS CLI:

   ```
   aws help
   ```

   The following command should display help information for Amazon Redshift:

   ```
   aws redshift help
   ```

For reference material on the Amazon Redshift CLI commands, go to [Amazon Redshift](http://docs.aws.amazon.com/cli/latest/reference/redshift/index.html) in the AWS CLI Reference\.