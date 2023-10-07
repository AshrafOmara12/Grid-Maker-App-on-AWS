# Grid-Maker-App-on-AWS

TASK 1.2: CREATE A DYNAMODB TABLE

` aws dynamodb create-table \
  --table-name GridBuilder \
  --attribute-definitions \
      AttributeName=uniqueGridId,AttributeType=S \
      AttributeName=s3Key,AttributeType=S \
  --key-schema \
      AttributeName=uniqueGridId,KeyType=HASH \
      AttributeName=s3Key,KeyType=RANGE \
  --provisioned-throughput \
      ReadCapacityUnits=5,WriteCapacityUnits=5 `

To create a compressed .zip deployment package of the application and its dependencies, run the following command:

` cd ~/environment/api-backend-manual/add_image ; zip ~/environment/api-backend-manual/add_image app.py `

TASK 2.2: CREATE A LAMBDA FUNCTION

` aws lambda create-function \
--function-name add_image \
--runtime python3.9 \
--timeout 30 \
--handler app.lambda_handler \
--role $LAMBDA_ROLE \
--environment Variables={SOURCE_BUCKET=$SOURCE_BUCKET} \
--zip-file fileb://~/environment/api-backend-manual/add_image.zip `


Task 3: Create an API by using API Gateway

` uniqueGridId=`date +%s` ; echo ${uniqueGridId} `
` baseUrl='placeholder-for-invoke-url' `

Invoke the API Gateway by using the following commands:

` cd ~/environment/api-backend-manual/source `

` curl -X POST --data-binary @image01.jpg "${baseUrl}/add_image?uniqueGridId=${uniqueGridId}" `

View the entries that were created in DynamoDB 

` aws dynamodb scan --table-name GridBuilder `

To engage the API to create the grid image and S3 presigned URL while formatting the output using jQuery, run the following command:

` curl -s -X POST "${baseUrl}/generate_grid?uniqueGridId=${uniqueGridId}" | jq -r '"\nMessage: " + .message, "\nPresigned_URL: " + .presigned_url, "\n"' `