# AWS Cloud Cost Optimization
As we know, stale resources on AWS can lead to higher cloud cost. So, here we’ll create a Lambda function in python to delete unused EBS volume snapshots. The Lambda function is used to delete snapshots that are not associated with any volume. Lambda functions can also be scheduled to run automatically using CloudWatch events.
