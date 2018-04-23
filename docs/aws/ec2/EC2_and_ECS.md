# EC2 and ECS
These two services _are not_ the same, but they work very closely together. In fact, without EC2 instances to support it, ECS does nothing on it's own. **With that said, if you are using the EC2 launch model (as opposed to the Fargate Launch Type model), there is [_no additional cost for ECS if you are paying for EC2 services_](https://aws.amazon.com/ecs/pricing/)

## What is ECS
Amazon Elastic Container Service. 
- A highly-scalable, high-performance container orchestration service that supports Docker containers
  - Allows you to easily run and scale containerized applications on AWS
  - Eliminates the need for you to:
    - Install and operate your own container orchestration software
    - Manage and scale cluster of virtual machines
    - Schedule containers on those virtual machines

### The ECS API
- With simple API calls, you can:
  - Launch and stop Docker-enabled applications
  - Query the complete state of your application
  - Access many familiar features such as:
    - IAM Roles
    - Security Groups
    - Load Balancers
    - Amazon CloudWatch Events
    - AWS CloudFormation templates
    - AWS CloudTrail logs