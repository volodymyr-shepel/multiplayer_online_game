## Architecture Components

### User Authentication

1. **Amazon Cognito**: 
    - **Sign-Up and Verification**: 
        - Users sign up using the Amazon Cognito UI.
        - An email verification code is sent to the user.
        - Upon successful verification, a "successful sign-up" trigger in the user pool is activated.

2. **AWS Lambda**:
    - **User Subscription**: 
        - The successful sign-up trigger invokes a Lambda function.
        - This Lambda function subscribes the new user to an Amazon SNS topic for notifications.

### Data Management

1. **Amazon S3**:
    - Used for storing user profile images.
    
2. **Amazon DynamoDB**:
    - Stores user data, match history, and ranking information.
    - Backend interacts to retrieve data about match history and leader board.

### Game Results Processing

1. **Amazon API Gateway**:
    - **Endpoint Invocation**:
        - When a game finishes, the backend calls an API Gateway endpoint.
        - This endpoint invokes a POST method to handle the results.

2. **AWS Lambda**:
    - **Results Processing**:
        - The API Gateway triggers a Lambda function.
        - This Lambda function:
            - Updates the match history in DynamoDB.
            - Updates user rankings in DynamoDB.
            - Publishes a message to an Amazon SNS topic to notify all subscribers about the ranking update.

3. **Amazon SNS**:
    - **Notification**:
        - Subscribers to the SNS topic receive email notifications about ranking updates.

## Workflow Summary

1. User signs up via Cognito UI and verifies email.
2. Post-verification, a Lambda function subscribes the user to the SNS topic.
3. The backend interacts with S3 and DynamoDB using AWS credentials.
4. Upon game completion:
    - The frontend invokes an API Gateway endpoint.
    - A Lambda function processes the results, updates DynamoDB, and publishes an SNS message.
5. Subscribers receive email notifications about ranking updates.
