# Marathon Hello World
The Marathon Hello World application is meant to be a simple example of how to familiarize yourself with DC/OS, as well as understand some of the artifacts required for deploying a service into Marathon.

This example utilizes a docker container which runs under Marathon management to dynamically sync objects between two S3 buckets.

## Prerequisites to run the example in your sandbox
1. A running DC/OS cluster
2. DC/OS CLI installed on your machine
3. Access to an AWS account

### Configuration
Follow the steps below to configure the hello world scripts for your sandbox:

1. Clone the repo down to your local machine
2. Update the __s3-policy.json__ file with a source and destination bucket
 * AWS S3 bucket names must be globally unique
 * The source and destination bucket names MUST match the CloudFormation template and Marathon definition
3. Attach the __s3-policy.json__ to an existing AWS user
4. Run the CloudFormation template __s3-cloudformation.json__ in your AWS account
 * The template is used to create two S3 buckets
 * The source and destination bucket names MUST match the names used in step #2
5. Update the Marathon definition __s3sync-marathon.json__ environment variables to match your configuration

### Usage
Before starting the service, upload a few small objects into the S3 source bucket. Please note the `aws s3 sync` CLI command is fairly **memory intensive**; therefore, ensure you have a significant amount of memory allocated to the service if you plan to sync thousands of objects.

Add your application to Marathon using the DC/OS Marathon CLI.
`dcos marathon app add s3sync/s3sync-marathon.json`

Verify the app is added with this command.
`dcos marathon app list`

### Verification
Once you deploy and run the marathon service, verify that the objects in the S3 source bucket have been synced with the AWS S3 destination bucket.

### Cleanup
- Delete the objects from the source and destination buckets
- Delete the CloudFormation stack
- Delete the running service out of Marathon