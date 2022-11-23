# Testing maximum concurrency of AWS Lambda functions when using Amazon SQS as an event source

This AWS sample demonstrates how the maximum concurrency configuration for SQS as an event source helps control the Lambda function concurrency. This SAM template defines two Lambda functions and two SQS queues - one with maximum concurrency set to 5, another with [reserved concurrency](https://docs.aws.amazon.com/lambda/latest/operatorguide/reserved-concurrency.html) set to 5. By adding messages to the SQS queue, users can observe the difference in how Lambda processes the SQS messages  through the provided CloudWatch dashboards. You can read more about this feature in [AWS documentation and](tbd) or in this [blog](tbd)

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

