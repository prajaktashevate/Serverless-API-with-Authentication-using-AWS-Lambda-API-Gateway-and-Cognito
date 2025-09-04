📌 Project Objective :-
==============

Build a secure, scalable, serverless API that provides user authentication and authorization without managing servers, enabling rapid development and cost-effective deployment of web applications and mobile backends.

📌 Project Description:-
=============

This project implements a complete serverless authentication system using three core AWS services:
•	AWS Lambda: Executes your API business logic without server management
•	API Gateway: Manages HTTP requests, routing, and API security
•	Amazon Cognito: Handles user registration, authentication, and authorization
The system allows users to sign up, sign in, and access protected API endpoints using JWT tokens, all while running on AWS's managed infrastructure with automatic scaling and high availability.

📌 Architecture Flow :-
==
1. User Registration & Authentication Flow

   Client → API Gateway → Cognito User Pool → User registers/signs in → Receives JWT tokens (ID + Access)  


2. API Request Flow

Client (with JWT token) → API Gateway → Cognito Authorizer → Lambda Function → Response

3. Detailed Step-by-Step Process
   

Registration/Login:

1.	Client sends credentials to API Gateway endpoint
2.	API Gateway routes to Cognito User Pool
3.	Cognito validates credentials and returns JWT tokens
4.	Client stores tokens for future API calls

Protected API Access:
1.	Client includes JWT token in Authorization header
2.	API Gateway receives request and validates token with Cognito
3.	If valid, request is forwarded to Lambda function
4.	Lambda processes business logic and returns response
5.	API Gateway sends response back to client

<img width="1522" height="1020" alt="image" src="https://github.com/user-attachments/assets/25712091-e8f0-4ac3-98a3-418cb2fbfe86" />



📌 Benefits of This Setup:-
=

✅ Cost Efficiency

✅ Security & Compliance

✅  Scalability & Performance

✅ Development Speed

✅ Flexibility
