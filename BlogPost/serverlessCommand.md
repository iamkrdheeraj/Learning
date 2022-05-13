#### Install Via NPM:
```js
npm install -g serverless
```
#### Set Up Your AWS Account Credentials:
The Serverless Framework deploys to your own AWS account. You'll need to enable Serverless Framework to deploy to your AWS account by giving it access. Here is a guide to help you set up your credentials securely

Create A Service:
A "Service" is the Framework's project or app concept. You can create one from scratch or select an existing template by running.
```js:
serverless
```
Go through the onboarding flow and then navigate into the newly created directory.
```js:
cd my-new-service
```
#### Deploy A Service:
Use this when you have made changes to your Functions, Events or Resources in serverless.yml or you simply want to deploy all changes within your Service at the same time.

#### serverless deploy
Deploy A Function:
Use this to quickly upload and overwrite your AWS Lambda code on AWS, allowing you to develop faster.
```js:
serverless deploy function -f hello
```
Invoke The Function On AWS:
Invokes an AWS Lambda Function on AWS and returns logs.
```js:
serverless invoke -f hello -l
```
Invoke The Function Locally:
Invokes an AWS Lambda Function on your local machine and returns logs.
```js:
serverless invoke local -f hello -l
```
Stream Function Logs:
Open up a separate tab in your console and stream all logs for a specific Function using this command.
```js:
serverless logs -f hello -t
```
Remove The Service:
Removes all Functions, Events and Resources from your AWS account.
```js:
serverless remove
```