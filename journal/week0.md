# Week 0 — Billing and Architecture
## Required Homework/Tasks
## Getting the AWS CLI Working
##### We'll be using the AWS CLI often in this bootcamp, so we'll proceed to installing this account.

## Install AWS CLI
##### We are going to install the AWS CLI when our Gitpod enviroment lanuches.
##### We are are going to set AWS CLI to use partial autoprompt mode to make it easier to debug CLI commands.
##### The bash commands we are using are the same as the [AWS CLI Install Instructions](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
##### Update our .gitpod.yml to include the following task.
![image](https://user-images.githubusercontent.com/10183258/219793043-1ad2122c-7ee1-404f-956c-b3c552b5550a.png)

```
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```
I attempted to run the command by typing in <em>aws</em> but I recieved an error

```
C:\Users\moi>aws
'aws' is not recognized as an internal or external command,
operable program or batch file.
```
I was able to resolve the error by closing command prompt, and opening it again.

## Create a Budget
I created my own Budget for $1 because I cannot afford any kind of spend. I did not create a second Budget because I was concerned of 
budget spending going over the 2 budget free limit.
![image](https://user-images.githubusercontent.com/10183258/219794971-5088bb65-fde7-4473-b67c-1eb5e69721d5.png)

## Recreate Conceptual Diagram or Napkin Design
![image](https://user-images.githubusercontent.com/10183258/219795444-c0ebb51d-e445-4490-8385-e2e3402999c2.png)

## Recreate Logical Architectural Deisgn
![image](https://user-images.githubusercontent.com/10183258/219795483-703b23d2-8575-4e64-9b91-12e49675837d.png)

## Example of Referncing a file in the codebase
Example of me of referencing a file in my repo [budget.json.example](https://github.com/ahtealeb/aws-bootcamp-cruddur-2023/blob/week-0/aws/json/budget.json.example)

## Code Example
```
{
  "AlarmName": "DailyEstimatedCharges",
  "AlarmDescription": "This alarm would be triggered if the daily estimated charges exceeds 1$",
  "ActionsEnabled": true,
  "AlarmActions": [
      "arn:aws:sns:ca-central-1:***REMOVED***:billing-alarm"
  ],
  "EvaluationPeriods": 1,
  "DatapointsToAlarm": 1,
  "Threshold": 1,
  "ComparisonOperator": "GreaterThanOrEqualToThreshold",
  "TreatMissingData": "breaching",
  "Metrics": [{
      "Id": "m1",
      "MetricStat": {
          "Metric": {
              "Namespace": "AWS/Billing",
              "MetricName": "EstimatedCharges",
              "Dimensions": [{
                  "Name": "Currency",
                  "Value": "USD"
              }]
          },
          "Period": 86400,
          "Stat": "Maximum"
      },
      "ReturnData": false
  },
  {
      "Id": "e1",
      "Expression": "IF(RATE(m1)>0,RATE(m1)*86400,0)",
      "Label": "DailyEstimatedCharges",
      "ReturnData": true
  }]
}
```
