![image](https://github.com/Titli03/Serverless/assets/89897324/582f171a-2b1b-493c-ae8a-97fa7df38af5)

# Serverless CRUD Operation and Load test

This project will demonstrate the creation of CRUD microservice using Serverless services, Lambda function, API gateway and DynamoDB.

You can create a Lambda function (LambdaFunctionOverHttps) using the AWS Lambda console. Next, you create a DynamoDB (lambda-apigateway) table using the DynamoDB console. Next, you create an REST API (DynamoDBOperations) using  the API Gateway console and define one method (POST) on it. Then you test your API.

## Design Overview:

When you invoke your REST API, API Gateway routes the request to your Lambda function. The method (POST) for REST API (DynamoDBManager) is backed by a Lambda function (LambdaFunctionOverHttps). That is, when you call the API through an HTTPS endpoint, Amazon API Gateway invokes the Lambda function. The Lambda function will run the logic (as per the code updated in the function) interacts with DynamoDB and returns a response to API Gateway. API Gateway then returns a response to you.

The POST method on the DynamoDBManager resource supports the following DynamoDB operations:

- Create, update, and delete an item.

- Read an item.

- Scan an item.

- Other operations (echo, ping), not related to DynamoDB, that you can use for testing.

The request payload you send in the POST request identifies the DynamoDB operation and provides necessary data. For example:

The following is a sample request payload for a DynamoDB create item operation: 

![image](https://github.com/Titli03/Serverless/assets/89897324/29216be2-8086-462e-ba8c-25c15e23d7e3)



The following is a sample request payload for a DynamoDB read item operation:

![image](https://github.com/Titli03/Serverless/assets/89897324/ad551c74-acf0-4186-bf33-d6764b6a8865)


## Setup
### Create Lambda IAM Role
Create the execution role that gives your function permission to access AWS resources.

To create an execution role

1. Open the roles page in the IAM console.

2. Choose Create role.

3. Create a role with the following properties.
- Trusted entity – Lambda.
- Role name – lambda-apigateway-role.
- Permissions – Custom policy with permission to DynamoDB and CloudWatch Logs. This custom policy has the permissions that the function needs to write data to DynamoDB and upload logs.

![image](https://github.com/Titli03/Serverless/assets/89897324/7b5820c6-0db3-47cb-95e1-4ecf4ca286a4)
![image](https://github.com/Titli03/Serverless/assets/89897324/79283ef6-be21-407a-95db-4716ad26491d)




### Create Lambda Function
To create the function

1. Click "Create function" in AWS Lambda Console

2. Select "Author from scratch". Use name LambdaFunctionOverHttps , select Python 3.12 as Runtime. Under Permissions, select "Use an existing role", and select lambda-apigateway-role that we created, from the drop down

3. Click "Create function"

![image](https://github.com/Titli03/Serverless/assets/89897324/275c8f1c-9831-45f1-bd60-c5e16930ccd4)




4. Replace the boilerplate coding with the following code snippet and click "Save"

![image](https://github.com/Titli03/Serverless/assets/89897324/83cf137e-a46d-497c-9857-1eb1010f9817)

![image](https://github.com/Titli03/Serverless/assets/89897324/8b708898-b2cc-43af-aece-682d983f59b5)



### Test Lambda Function
Let's test our newly created function. We haven't created DynamoDB and the API yet, so we'll do a sample echo operation. The function should output whatever input we pass.



1. Click the arrow on "Select a test event" and click "Configure test events"
![image](https://github.com/Titli03/Serverless/assets/89897324/cde854e0-e6fd-4b5d-ae41-cc4d6b0bbfcd)




2. Paste the following JSON into the event. The field "operation" dictates what the lambda function will perform. In this case, it'd simply return the payload from input event as output. Click "Create" to save

![image](https://github.com/Titli03/Serverless/assets/89897324/d5854c6e-3df7-4c6f-ab4b-8c6563044a71)

![image](https://github.com/Titli03/Serverless/assets/89897324/5a553c5d-2374-4be3-a547-bf6ac040cf8d)






3. Click "Test", and it will execute the test event. You should see the output in the console.

![image](https://github.com/Titli03/Serverless/assets/89897324/e864db15-d5b4-4598-a2f5-c954caa23956)




We're all set to create DynamoDB table and an API using our lambda as backend!

### Create DynamoDB Table
Create the DynamoDB table that the Lambda function uses.

To create a DynamoDB table

1. Open the DynamoDB console.

2. Choose Create table.

3. Create a table with the following settings.

   Table name – lambda-apigateway

   Primary key – id (string)

4. Choose Create.

![image](https://github.com/Titli03/Serverless/assets/89897324/f16b9167-9ec9-4521-8df4-81ed3fd5bcd6)



### Create API
To create the API

1. Go to API Gateway console

2. Click Create API

![image](https://github.com/Titli03/Serverless/assets/89897324/f5dc85d5-fd90-46bd-b423-4fb630633b03)


3. Scroll down and select "Build" for REST API

![image](https://github.com/Titli03/Serverless/assets/89897324/0a4e5f56-89fc-4c20-b863-3a5019b2bb93)


4. Give the API name as "DynamoDBOperations", keep everything as is, click "Create API"

![image](https://github.com/Titli03/Serverless/assets/89897324/bcebe7bd-623f-46a9-915a-807ddd32ba19)


5. Each API is collection of resources and methods that are integrated with backend HTTP endpoints, Lambda functions, or other AWS services. Typically, API resources are organized in a resource tree according to the application logic. At this time you only have the root resource, but let's add a resource next

Click "Actions", then click "Create Resource"

![image](https://github.com/Titli03/Serverless/assets/89897324/f070002a-9e24-4eb9-abee-bc623f43a4d1)



6. Input "DynamoDBManager" in the Resource Name, Resource Path will get populated. Click "Create Resource"

![image](https://github.com/Titli03/Serverless/assets/89897324/50f9dc14-429c-42f6-8e0c-04235bfb8451)


7. Let's create a POST Method for our API. With the "/dynamodbmanager" resource selected, Click "Actions" again and click "Create Method".

![image](https://github.com/Titli03/Serverless/assets/89897324/f9906c7f-a7a5-4f52-bb6c-0b9d81e1e7fe)



8. Select "POST" from drop down , then click checkmark

![image](https://github.com/Titli03/Serverless/assets/89897324/fc4ef20b-794e-4873-8b6e-ebb9a5197216)


9. The integration will come up automatically with "Lambda Function" option selected. Select "LambdaFunctionOverHttps" function that we created earlier. As you start typing the name, your function name will show up, Select and click "Save". A popup window will come up to add resource policy to the lambda to be invoked by this API. Click "Ok"

![image](https://github.com/Titli03/Serverless/assets/89897324/b8f682e8-9a22-4477-9a25-b4c546c3c941)


Our API-Lambda integration is done!

### Deploy the API
In this step, you deploy the API that you created to a stage called prod.

1. Select "Deploy API"

2. Now it is going to ask you about a stage. Select "[New Stage]". Give "Prod" as "Stage name". Click "Deploy"

![image](https://github.com/Titli03/Serverless/assets/89897324/fa312aba-a546-4a1b-bd12-5d6c07b950a2)

3. We're all set to run our solution! To invoke our API endpoint, we need the endpoint URL. In the "Stages" screen, expand the stage "Prod", select "POST" method, and copy the "Invoke URL" from screen

![image](https://github.com/Titli03/Serverless/assets/89897324/2b3ff88e-bf65-4647-bc2c-6bd298ac2d2b)


### Running our solution
1. The Lambda function supports using the create operation to create an item in your DynamoDB table. To request this operation, use the following JSON

![image](https://github.com/Titli03/Serverless/assets/89897324/52507e7e-c3bf-4771-97b7-7d5b26e8d5a7)


2. To execute our API from local machine, we are going to use Postman and Curl command. You can choose either method based on your convenience and familiarity.

To run this from Postman, select "POST" , paste the API invoke url. Then under "Body" select "raw" and paste the above JSON. Click "Send". API should execute and return "HTTPStatusCode" 200.

![image](https://github.com/Titli03/Serverless/assets/89897324/884d6609-9eff-4412-bc9b-c98254cf102c)


To run this from terminal using Curl, run the below

    $ curl -X POST -d "{\"operation\":\"create\",\"tableName\":\"lambda-apigateway\",\"payload\":{\"Item\":{\"id\":\"1\",\"name\":\"Bob\"}}}" https://$API.execute-api.$REGION.amazonaws.com/prod/DynamoDBManager
3. To validate that the item is indeed inserted into DynamoDB table, go to Dynamo console, select "lambda-apigateway" table, select "Explore item" tab, and the newly inserted item should be displayed.

![image](https://github.com/Titli03/Serverless/assets/89897324/ba87f45c-946e-4ea2-a081-dec4ced2983b)


4. To get all the inserted items from the table, we can use the "list" operation of Lambda using the same API. Pass the following JSON to the API, and it will return all the items from the Dynamo table

![image](https://github.com/Titli03/Serverless/assets/89897324/6e398b39-56ad-4a72-91fe-92174facd141)



## Performance test using Postman
1. Install native desktop version postman tool and sign in to an account.

2. Create new collection and post method.

3. Run collection in performance test mode

![image](https://github.com/raffaello76/Serverless01/assets/89897324/be126b02-493d-49ec-8af4-9523160c5b3c)


5. Below is the report for the average response time and error rate where the configuration for Lambda function was 128 MB Memory and 10 sec time out.

![image](https://github.com/raffaello76/Serverless01/assets/89897324/61e2b2e9-2776-4521-a6e4-91e8fecb3153)


6. Then I updated the Lambda configuration by increasing 1024 MB Memory and 10 sec time out and now if I run the performance test you can see the average response time is 283 ms (decreased) as per below performance report.

![image](https://github.com/raffaello76/Serverless01/assets/89897324/b81368d0-c211-4a93-bb1a-f6fa1048c635)


## Cleanup 
### Cleaning up DynamoDB
1. To delete the table, from DynamoDB console, select the table "lambda-apigateway", and click "Delete table"

![image](https://github.com/raffaello76/Serverless01/assets/89897324/ecca41bb-1eaa-483d-8887-d8a2a485f702)


3. To delete the Lambda, from the Lambda console, select lambda "LambdaFunctionOverHttps", click "Actions", then click Delete

![image](https://github.com/raffaello76/Serverless01/assets/89897324/c09c31a0-cdb2-4531-9e10-a3b3ea8805d2)


5. To delete the API we created, in API gateway console, under APIs, select "DynamoDBOperations" API, click "Actions", then "Delete"

![image](https://github.com/raffaello76/Serverless01/assets/89897324/f9d5efcf-2d3c-4161-ace7-f4a91d8dfd35)
