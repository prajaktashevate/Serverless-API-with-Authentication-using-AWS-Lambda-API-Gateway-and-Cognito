1. Create a Cognito User Pool

     1.	Go to AWS Management Console → Cognito → User Pools.
     2.	Click Create User Pool.
     3.	Configure:
        
            o	Pool name: UserPool-sgrkv6
       	
            o	Sign-in options → Choose Email (or Username, or both).
       	
            o	Enable Self sign-up if you want users to register.
       	
            o	Configure password policy (defaults are fine for now).
       	
4.	Create a User Pool Client (App client):
   
            o	Example: AWSCognito-Project.
  	
            o	Do not enable client secret if this will be called from browsers/mobile apps.
  	
6.	Save:
   
            o	User Pool ID (e.g., ap-south-1_XXXXXX)

            o	App Client ID (e.g., 2jhq4d1fabc123example)

 <img width="940" height="428" alt="image" src="https://github.com/user-attachments/assets/1b233bb3-1542-4629-b3c3-7fb9e3c02081" />

________________________________________
2. Create a Lambda Function
   
           1.	Go to AWS Management Console → Lambda → Create function.
   
           2.	Select Author from scratch.
   
                  o	Function name: Lambdaproject
                  o	Runtime: Python 3.12 (or Node.js if you prefer).
   
3.	Example Python code:
   
import json

def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": json.dumps({
            "message": "Hello from Lambda!",
            "input": event
        })
    }

4.	Click Deploy.
   <img width="940" height="438" alt="image" src="https://github.com/user-attachments/assets/a22b9518-bb87-4214-a647-c0bd989943e9" />

   <img width="940" height="406" alt="image" src="https://github.com/user-attachments/assets/d6522805-14ae-4f58-a2d4-d15b3f99b297" />


 
 

________________________________________
3. Create an API Gateway
               1.	Go to Amazon API Gateway → Create API → REST API.
                   (You can also use HTTP API for a simpler, faster option).
   
2.	Create a resource: /hello.
   
3.	Create a method: GET.
   
         o	Integration type: Lambda Function.
         o	Choose your Lambda (Lambdaproject).
4.	Deploy the API:
   
         o	Create a Stage (e.g., dev).
  	
6.	Copy the Invoke URL:
           	https://<api-id>.execute-api.ap-south-1.amazonaws.com
 <img width="940" height="416" alt="image" src="https://github.com/user-attachments/assets/17ebbe44-685e-41a7-a015-3f81748e0ff5" />

________________________________________
4. Add Cognito Authorizer to API Gateway
   
             1.	In API Gateway, go to Authorizers → Create new authorizer.
             2.	Configure:
                      o	Type: Cognito
                      o	Name: ClientA
                      o	Choose your Cognito User Pool
                      o	Region: same region as pool (ap-south-1)

3.	Save it.

4.	Go to your API method (/hello → GET) →
                           o	Click Method Request
                           o	Under Authorization, select ClientA (your Cognito authorizer).

5.	Deploy API again to apply changes.
<img width="940" height="273" alt="image" src="https://github.com/user-attachments/assets/6edde9f4-fa10-444c-a16c-be0b6c09ac96" />

________________________________________
5. Test the Setup

A) Get a JWT Token

Cognito Hosted UI

•	URL format:

•	https://<your-domain>.auth.<region>.amazoncognito.com/login?

•	client_id=<APP_CLIENT_ID>&

•	response_type=token&

•	scope=openid&

•	redirect_uri=https://oauth.pstmn.io/v1/callback

•	Log in → You’ll get redirected with #id_token=...&access_token=....

<img width="940" height="546" alt="image" src="https://github.com/user-attachments/assets/4c2df274-d781-4a44-a8d7-c6d6106a6caa" />

 
________________________________________
B) Call API with Token

Use AccessToken in the Authorization header:

✅ If token is valid → You’ll get Lambda’s response.

❌ If token is invalid/expired → API Gateway returns 401 Unauthorized.
 
 <img width="940" height="764" alt="image" src="https://github.com/user-attachments/assets/434e7d42-dd5b-4e11-ab79-0da64c309fd4" />

 <img width="940" height="273" alt="image" src="https://github.com/user-attachments/assets/620a32b5-787c-490c-b6d5-7d85fe3a8792" />

________________________________________
6. Final Workflow

1.	User signs up/signs in with Cognito → receives JWT token.

2.	User sends request to API Gateway with Authorization: Bearer <token>.

3.	API Gateway Authorizer validates token with Cognito.

4.	If valid → forwards request to Lambda.

5.	Lambda runs code → returns response.

