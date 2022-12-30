# Testing maximum concurrency of AWS Lambda functions when using Amazon SQS as an event source

This AWS sample demonstrates how the maximum concurrency configuration for SQS as an event source helps control the Lambda function concurrency. This SAM template defines two Lambda functions and two SQS queues - one with maximum concurrency set to 5, another with [reserved concurrency](https://docs.aws.amazon.com/lambda/latest/operatorguide/reserved-concurrency.html) set to 5. By sending the same traffic to the SQS queue, users can observe the difference in how Lambda processes the SQS messages through the provided CloudWatch dashboards. You can read more about this feature in [AWS documentation](tbd) and or in this [blog](tbd)

## Step to run the demo
Running this demo requires the [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and the [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html). After installing both CLIs, clone this GitHub repository and navigate to the root of the directory.
```
git clone https://github.com/aws-samples/aws-lambda-amazon-sqs-max-concurrency

cd aws-lambda-amazon-sqs-max-concurrency
```
### Deploying the SAM template
First build your SAM template with the [build](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-build.html) command to prepare for deployment in your AWS environment. 
```
sam build
```
Then use the [deploy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-deploy.html) command to deploy the resources in your account.
```
sam deploy --guided
```
After walking through the deploy command, you can track the progress through the CLI or by navigating to the [AWS CloudFormation](http://console.aws.amazon.com/cloudformation) page in your AWS console. It will take a few minutes to complete the provisioning of resources.

### Running the demo
The deployed Lambda function code simply sleeps for 10 seconds before returning a 200 response. This allows for reaching a high Lambda function concurrency number with only a small number of messages. You can add 25 messages to a queue using the following command.
```
for i in {1..25}; do aws sqs send-message --queue-url <queue URL> --message-body testing; done 
```
You will need to include your queue URL in the above command. The URLs are available in the Outputs tab in the console, or you can navigate to the SQS console to find the queue URLs. 

After sending messages to both queues, click on the dashboard URL available in the Outputs tab to navigate to the CloudWatch dashboard.

### Validating results
Both Lambda functions should have the same number of invocations, and the same concurrent invocations fixed at 5. However, you can see that the ReservedConcurrencyFunction experienced throttling, and messages were sent to the corresponding DLQ, whereas MaxConcurrencyFunction did not. 

### Clean up
To remove all the resources created while following this demo, use the [delete](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-delete.html) command and follow the prompts.
```
sam delete
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

