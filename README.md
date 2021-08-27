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
### Describe
- Fully Managed build Service
- Alternative to other build tools such as Jenkins
- Continuous scaling ( no servers to manage or provision - no build queue)
- Pay for usage : the time it takes to complete the builds
- Leverages Docker under the hood for reproducible builds
- Posiibility to extend capabilities leveraging our own base Docker images
- Secure : Integration with KMS for encryption of build artifacts, IAM for build permissions, and VPC for network security, CloudTrail for API calls logging

- Environment Variable (ex. DB Url)
- Parameter Store (ex. DB Password)

## CodeDeploy