# AWS Cloud Cost Optimization Project: Identifying Stale Resources

## Overview

This project demonstrates an AWS Lambda function designed to optimize storage costs by identifying and deleting stale Elastic Block Store (EBS) snapshots. These snapshots are no longer associated with any active EC2 instances and can unnecessarily increase storage costs.

## Description

The Lambda function performs the following actions:
1. Retrieves all EBS snapshots owned by our AWS account.
2. Fetches a list of all active EC2 instances (both running and stopped).
3. Checks each snapshot to determine if it is associated with a volume that is no longer linked to any active EC2 instance.
4. Deletes any identified stale snapshots to reduce storage costs effectively.

## Prerequisites

Before starting, ensure we have an EC2 instance running with an associated volume and snapshots. The function is designed to clean up snapshots that remain after instances and volumes have been deleted but their associated snapshots were not removed.

## Setup Instructions

Follow these steps to deploy the Lambda function:

1. **Create the Lambda Function**:
   - Go to the AWS Lambda service.
   - Click on **Create function**.
   - Enter a function name and select a language (e.g., Python).
   - Click on **Create function**.
   - Remove any existing code in the Lambda code editor.
   - Copy and paste the Lambda function code from this repository into the editor.
   - Save the changes and deploy the function.
   - Click on the **Test** button and create a new test event with a name (e.g., `test`).
   - Click **Test** again. You may encounter a `Sandbox.Timedout` error.
   - To fix this, go to the **Configuration** tab, click on **Edit**, increase the timeout to 10 seconds, and save. [Note: Try to keep this as low as possible as AWS will also consider this as a parameter in your costing]

2. **Set Up Permissions**:
   - Go to the **Configuration** tab and click on **Permissions** to view the Execution Role.
   - Click on the role name link to modify permissions.
   - Click on **Add permissions** and choose **Attach policies**. [Note: Instead of giving the full access, we can create policy to just give the least required access. ]

3. **Create and Attach Policies**:
   - **For Snapshot Permissions**:
     - Go to IAM and create a new policy.
     - Choose **EC2** service and search for **snapshot**.
     - Select `DescribeSnapshots` and `DeleteSnapshot`.
     - Under **Resources**, select **All** and create the policy.
     - Attach this policy to the Lambda execution role.
   - **For Volume and Instance Permissions**:
     - Create another policy for EC2.
     - Under **Access Level**, select **List** and choose `DescribeVolumes` and `DescribeInstances`.
     - Create and attach this policy to the Lambda execution role as well.

4. **Re-test the Function**:
   - After attaching the necessary permissions, test the function again. You'll notice it won't delete the snapshot as it is attached to the existing EC2 Instance and Volume.
   - So to simulate an actual organizational scenario for cost optimization with stale resources, first, delete the EC2 instance, which will also delete the associated volume. However, you will notice that the snapshot still exists, which is now considered a stale resource. Next, run the Lambda function again, and this time it should identify and delete stale snapshots. To confirm, go to the EC2 snapshots section and verify that the snapshot has been removed.

## Automating with CloudWatch

To automate this process, we can set up a CloudWatch rule:
- Go to CloudWatch, then **Events** > **Rules**.
- Click on **Create Rule** and set up a schedule based on your requirements.
- This setup ensures that the Lambda function runs periodically, minimizing the risk of unnecessary storage costs due to stale snapshots.

By following these steps, you can efficiently manage AWS costs and maintain a clean environment as a DevOps engineer. This approach is highly effective in preventing mistakes and significantly reducing AWS resource costs.

Thanks, Bhanu