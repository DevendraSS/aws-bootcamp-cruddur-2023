# Week 0 — Billing and Architecture

## Creating Gitpod workspace
Created a gitpod account to open the project in gitpod workspace

## Installing AWS CLI
Created a task in `gitpod.yml` to install `AWS CLI` using the below code:

```
tasks:
  - name: aws-cli
    env:
      AWS_CLI_AUTO_PROMPT: on-partial
    init: |
      cd /workspace
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
      cd $THEIA_WORKSPACE_ROOT
```

## Creating a new user in IAM Users console
- Opening `IAM User console`
- Enabled console access to user
- Gave administrative access to the user
- Creating `Access key` inside the security credentials after clicking into the user
- Saving the `credentials.csv` file

## Setting environment variables
In the terminal we'll paste this command one-by-one:

```
export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=us-east-1

```
To permanently save our credentials in our gitpod environment everytime we relaunch we'll use:

```
gp env AWS_ACCESS_KEY_ID=""
gp env AWS_SECRET_ACCESS_KEY=""
gp env AWS_DEFAULT_REGION=us-east-1

```
## Checking to see if the AWS CLI is working as expected

```
aws sts get-caller-identity

```
## Setting Billing alerts
- Hovering to the billing page inside the root account
- Under billing prefrences choose `Receive Billing Alerts`
- Saving the preferences

## Creating a Billing alarm

### Creating an SNS topic which we'll alert us in case of overbilling

```
aws sns create-topic --name billing-alarm

```
This shall return a TopicARN

Creating a subscription by supplying the TopicARN and our email

```
aws sns subscribe \
    --topic-arn TopicARN \
    --protocol email \
    --notification-endpoint your@email.com
```
### Creating the alarm using aws cli
Update the json config file with the TopicARN
```
aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm_config.json

```

## Create AWS Budget

Fetch your AWS account id

```
aws sts get-caller-identity --query Account --output text

```
Another way of creating budget

```
aws budgets create-budget \
    --account-id AccountID \
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json

```


