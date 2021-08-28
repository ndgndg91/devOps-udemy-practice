# AWS DevOps Practice

## CodeCommit
### Describe
- Version control like Git
- Git repositories can be expensive.
- The industry includes:
    - Github : free public repositories, paid private ones
    - BitBucket
    - Etc..
- And AWS CodeCommit
    - private Git repositories
    - No size limit on repositories ( scale seamlessly )
    - Fully managed, highly available
    - Code only in AWS Cloud account ⇒ increased security and compliance
    - Secure ( encrypted, access control, etc..)
    - Integrated with Jenkins / CodeBuild / other CI tools

## CodeBuild
### Documentation
https://docs.aws.amazon.com/ko_kr/codebuild/latest/userguide/welcome.html
https://docs.aws.amazon.com/ko_kr/codebuild/latest/userguide/sample-docker.html
### Describe
- Fully Managed build Service
- Alternative to other build tools such as Jenkins
- Continuous scaling ( no servers to manage or provision - no build queue)
- Pay for usage : the time it takes to complete the builds
- Leverages Docker under the hood for reproducible builds
- Posiibility to extend capabilities leveraging our own base Docker images
- Secure : Integration with KMS for encryption of build artifacts, IAM for build permissions, and VPC for network security, CloudTrail for API calls logging

- Environment Variable (ex. DB Url) can be printed by printenv command
- Parameter Store IN AWS Systems Manager (ex. DB Password)
- Service Role Policies In /policy/*.json

## CodeDeploy
- Each EC2 Machine (or On Premise machine) must be running the CodeDeploy Agent
- The agent is continuously polling AWS CodeDeploy for work to do
- CodeDeploy sends appspec.yml file.
- Application is pulled from Github or S3
- EC2 will run the deployment instructions
- CodeDeploy Agent will report of success / failure of deployment on the instance

### EC2 IAM Role
AmazonS3ReadOnlyAccess policy

### Installing the CodeDeploy agent on EC2
```
sudo yum update -y
sudo yum install -y ruby wget
wget https://aws-codedeploy-eu-west-1.s3.eu-west-1.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent status
```

### Security Group
- InBound
    - TCP 22 port SSH open
    - TCP 80 port HTTP open

### Way On Console
1. Create Application for EC2 / On premise 
2. Create Deployment Group ( set of EC2 )

    Tag Key:Value 로 Target EC2 Instance Filtering 이 가능

3. Create Deployment
4. Create S3 bucket and Create Version
    ```bash
    aws deploy push --application-name {CodeDeployApplicationName} \
    --s3-location s3://{bucketName}/{objectKey} \
    --ignore-hidden-files --region {AWS_REGION} --profile {AWS_PROFILE}
    ```
5. Create ServiceRole - AWSCodeDeployRole

### appsepc.yml && Environment Variables && Hooks
- https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html

### CodeDeploy Cloud Watch events Ann Monitoring
- https://docs.aws.amazon.com/codedeploy/latest/userguide/monitoring-cloudwatch-events.html
- https://aws.amazon.com/ko/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console
- manually install cloud watch agent, codedeploy agent

### CodeDeploy Trigger
- https://docs.aws.amazon.com/codedeploy/latest/userguide/monitoring-sns-event-notifications-create-trigger.html
- Trigger only support SNS

### CodeDeploy Roll Back
- https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-rollback-and-redeploy.html
- In-place
    - Roll back when a deployment fails - instance 중 하나라도 실패 시 모두 롤백
    - Roll back when alarm thresholds are met - ex) CPU utilization up to 100% → roll back
- https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups-configure-advanced-options.html