Process & analyze the sales data of tech gadgets in near real-time as CDC using AWS Cloud services

# Tech Stack
1. Python
2. DynamoDB
3. DynamoDB Streams
4. Kinesis Streams
5. Event Bridge Pipe
6. Kinesis Firehose
7. AWS Lambda
8. Glue Crawler
9. Athena
10. S3

# DynamoDB
Create DynamoDB table : OrderTable: -
![image](https://github.com/user-attachments/assets/49742527-fe69-4414-b104-36d4244c28c4)


Run mock_data_generator_for_dynamodb.py to create real-time source dummy data from the user with DynamoDB access role
![image](https://github.com/user-attachments/assets/0bb5723a-c59d-4cc6-8ecf-d9da92c90331)


# DynamoDB Streams
Capture item-level changes in your table, and push the changes to a DynamoDB stream. You then can access the change information through the DynamoDB Streams API.
![image](https://github.com/user-attachments/assets/f89f7719-a826-48b2-a10b-0848564e1932)

# Kinesis
Create Kinesis Data Stream: kinesis-sales-data

![image](https://github.com/user-attachments/assets/5ac032ed-ab8e-4029-94b6-6d9b78a87156)


Use the **EventBridge Pipe** to connect the _**DynamoDB Streams' captured events**_ to a target _**Kinesis data Stream**_ with optional filtering and enrichment.
![image](https://github.com/user-attachments/assets/75a98d54-62f4-4852-a18c-f1af99592da4)

![image](https://github.com/user-attachments/assets/e6631342-0221-42c0-9097-6cf009725dae)

![image](https://github.com/user-attachments/assets/90a42ced-d386-4748-840f-6c4e4fdad397)

Add permission to the execution role of this EventBridge Pipe: -
1.	Kinesis role
2.	DynamoDB role


Now as we insert(run the data generation script) or update the items in dynamoDB table -> the same will be updated in the kinesis stream: -
![image](https://github.com/user-attachments/assets/e6949a6f-e96c-4ba2-90bd-f727891d94bb)

Records in Kinesis Steam in JSON format: -

![image](https://github.com/user-attachments/assets/657c2b10-e765-46ee-a950-e12104e4a300)

# Kinesis Data Firehose
Use Amazon Firehose to put a chunk of the Kinesis stream into S3 in batches after adding the required transformation/cleansing using lambda : transformation_layer_with_lambda.py: -

![image](https://github.com/user-attachments/assets/e130ae87-fe2c-458b-87ab-14f13ae3512f)


![image](https://github.com/user-attachments/assets/747b422b-ef44-40f8-a082-7146d9c9388e)


![image](https://github.com/user-attachments/assets/81a460b1-d98f-4bac-b104-5423822762ae)


# S3
Data in the S3 bucket will look like this: -

![image](https://github.com/user-attachments/assets/0cb65c75-40d3-4f94-932a-ffdc1a094515)

![image](https://github.com/user-attachments/assets/59f44035-a7c3-416e-915f-4d65a6f46542)


# Glue Crawler
Create and run a crawler with JSON classifiers to crawl through the latest partitioned data in the above S3 bucket

![image](https://github.com/user-attachments/assets/a9ff6db0-84af-40f7-8fb3-844d780a65fc)

![image](https://github.com/user-attachments/assets/d133d4b8-c757-415f-906d-406e47903def)

2 Projection Tables will be created as below: -
![image](https://github.com/user-attachments/assets/133673fb-804c-4cf0-a626-650b8ef1076d)

# Athena

Table - projection_2024
![image](https://github.com/user-attachments/assets/0b26f625-9f4e-401a-8c49-1cbe17e40e16)

Table - projection_processing_failed
![image](https://github.com/user-attachments/assets/3bb53f18-2eba-47f3-a4c6-6a765ab3fa4b)
