# Bike rental service for tourists or locals.

* * *

### Technology Stack:

*   **Programming Language**: TypeScript
*   **Web Framework**: Express.js
*   **ODM & Validation Library**: zod, Mongoose for MongoDB

### Main Part: (50 Marks)

### Models:

1. **User Model**:
    *   **name**: The name of the user.
    *   **email**: The contact email address.
    *   **password**: The account password.
    *   **phone**: The contact phone number.
    *   **address**: The physical address.
    *   **role**: admin / user
2. **Bike Model**:
    *   **name**: The name of the bike model.
    *   **description**: A brief description of the bike.
    *   **pricePerHour**: The rental price per hour.
    *   **isAvailable**: Indicates if the bike is available for rental (default: true).
    *   **cc**: The engine capacity of the bike in cubic centimeters.
    *   **year**: The manufacturing year of the bike.
    *   **model**: The model of the bike.
    *   **brand**: The brand of the bike.
3. **Booking Model**:
    *   **user**: Reference to the User model.
    *   **bike**: Reference to the Bike model.
    *   **startTime**: The start time of the rental.
    *   **returnTime**: The return time of the rental.
    *   **totalCost**: The total cost of the rental.
    *   **isReturned**: Indicates if the bike has been returned (default: false).

### API Endpoints:

**User Routes**:

1. **Sign Up**
    *   **Route**: /api/auth/signup (POST)
    *   **Request Body**:

```perl
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "phone": "1234567890",
    "address": "123 Main St, Anytown"
  }
  
```

*       *   **Response**:

```json
  {
    "success": true,
    "statusCode": 201,
    "message": "User registered successfully",
    "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1f5",
      "name": "John Doe",
      "email": "john@example.com",
      "phone": "1234567890",
      "address": "123 Main St, Anytown",
      "createdAt": "2024-06-10T13:26:51.289Z",
      "updatedAt": "2024-06-10T13:26:51.289Z",
      "__v": 0
    }
  }
  
```

1. **User Login**
    *   **Route**: /api/auth/login (POST)
    *   **Request Body**:

```perl
  {
    "email": "john@example.com",
    "password": "password123"
  }
  
```

*       *   **Response**:

```json
  {
    "success": true,
    "statusCode": 200,
    "message": "User logged in successfully",
    "token": "jwt_token",
    "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1c3",
      "name": "John Doe",
      "email": "john@example.com",
      "phone": "1234567890",
      "address": "123 Main St, Anytown"
    }
  }
  
```

1. **Get Profile**
    *   **Route**: /api/users/me (GET)
    *   **Request Headers**: Authorization: Bearer jwt\_token
    *   **Response**:

```json
  {
    "success": true,
    "statusCode": 200,
    "message": "User profile retrieved successfully",
    "data": {
        "_id": "6666ff917181b8e5ffe04f91",
        "name": "admin",
        "email": "admin@gmail.com",
        "phone": "1234567890",
        "address": "123 Main St, Anytown",
        "role": "admin",
        "createdAt": "2024-06-10T13:28:49.260Z",
        "updatedAt": "2024-06-10T13:28:49.260Z",
        "__v": 0
    }
  }
  
```

1. **Update Profile**
    *   **Route**: /api/users/me (PUT)
    *   **Request Headers**: Authorization: Bearer jwt\_token
    *   **Request Body**:

```json
  {
    "name": "John Updated",
    "phone": "0987654321"
  }
  
```

*       *   **Response**:

```json
  {
    "success": true,
    "statusCode": 200,
    "message": "Profile updated successfully",
    "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1c5",
      "name": "John Updated",
      "email": "john@example.com",
      "phone": "0987654321",
      "address": "123 Main St, Anytown"
    }
  }
  
```

**Bike Routes**:

1. **Create Bike (Admin Only)**
    *   **Route**: /api/bikes (POST)
    *   **Request Headers**: Authorization: Bearer jwt\_token
    *   **Request Body**:

```plain
  {
    "name": "Mountain Bike",
    "description": "A durable mountain bike for rough terrains.",
    "pricePerHour": 15,
    "cc": 250,
    "year": 2022,
    "model": "X1"

    "brand": "Yamaha"
  }
  
Resp
```

*       *   **nse**:

```plain
  {
    "success": true,
    "statusCode": 200,
    "message": "Bike
added successfully",
    
data": {
      "
id": "60d9c4e4f3b
b544b8b8d1c4",
     
"name": "Mountain Bik
",
  
  
"description"
 "A durable mountain
bike for rough terrain
.",
      "pricePerHour": 15,
      "isAvaila
le": true,
    
 "cc": 250,
      "year": 2022,
      "model
: "X1",
      "brand": "Yamaha"
  
 }
  }
  
```

1. **Get All Bikes**
    *   **Route**: /api/bikes (GET)
    *   **Response**:

```plain
  {
    "success"
 true,
    "statusCode": 20
,
    "message": "Bikes retr
eved successfull
",
    "data": [
      

        "_id": "bike_
d",
        "name": "Moun
ain B
ke"

   
    "description": "A durable mountain bike for rou
h terrains.",
        
pricePerHour": 15,
     
  "isAvailable": true,
        "cc": 250,
        
year": 2022,
  
     "m
del": "X1",
        "brand"
 "Yamaha"
      },
      ...other bi
es...
    ]
  }
  
```

1. **Update Bike (Admin Only)**
    *   **Route**: /api/bikes/:id (PUT)

```plain
Req
est Headers: Authorization:
```

*       *   Bearer jwt\_token

```plain
Request Bod
```

*       *   :

```json
  {
```

```plain
    "pricePerHou
```

```json
": 20
  }
  
```

*       *   **Response**:

```plain
  {
  
 "success": true,
    "s
atusCode": 200,
    "messag
": "Bike
updated successfully",
  
 "dat
": 

   
  "_id": "bike_id",
      "name": "Mountain Bike",
      "description": "A durable mountain bike for rough terrains.",
      "pricePerHour": 20,
      "isAvailable": true,
      "cc": 250,
      "
ear": 2022,
      "model
: "X1",
      "brand": "Yamaha"
    }
  }
  
Delete
Bike (Admin O
```

1. **ly)**

```plain
Route: /api/bikes/:
```

*       *   d (DELETE)

```plain
Request Headers: Au
```

*       *   horization: Bearer jwt\_token
    *   **Response**:

```plain
  {
    "success": true,
    "stat
sCode": 200,
    "message":
"Bike deleted successfully
,
    "data": {
    
 "_id": "bike_id",
  
   "name": "Mountain
Bike",
      "description
: "A 
ura
le
mountain bike for rough terrains.",
      "pricePerHour": 20,
      "isAvailable": false,
      "cc": 250,
      "year": 2022,
      "model": "X1",
   
  "brand": "Yamaha"
    

  }
  
```

**Rental Routes**:

1. **Create Rental**

```plain
Route: /ap
/rentals (POS
```

*       *   )

```plain
Request Headers: Auth
```

*       *   rization: Bearer jwt\_token

```plain
Use
```

*       *   information should be taken from the token

```plain
Make sure to change the
status of bike availabili
```

*       *   y to false
    *   **Request Body**:

```json
  {
```

```plain
  
 "bikeId": "60d9
4e4f3b4b544b8b8d1c4
```

```json
,
```

```plain
    "startTime": "20
```

```json
4-06-10T09:00:00Z"
  }
  
```

```plain
R
spons
```

*       *   :

```plain
  {
    
success": true,
    "statusCode": 200,
    "message": "Rental created successfully",
  	"data": {
  		"_id": "60d9c4e4f3b4b544b8b8d1c4",
  		"userId": "60d9c4e4f3b4b544b8b8d1c3",
  		"bikeId": "60d9c4e4f3b4b544b8b8d1c4",
  		"startTime": "2024-06-10T09:00:00Z",
  		"returnTime": null,
  		"totalCost": 0,
  		"isReturned": false
  	}
  }
```

1. **Return Bike (Admin Only)**
    *   **Route**: /api/rentals/:id/return (PUT)
    *   **Request Headers**: Authorization: Bearer jwt\_token
    *   **Request Body**: Not needed
    *   Make sure to change the status of bike availability to true again
    *   **Hints**: The cost should be calculated based on the start and return time of the rental. For example, if the start time is "2024-06-10T09:00:00Z" and the return time is "2024-06-10T18:00:00Z" (current time), the total rental duration is 9 hours. If the price per hour is $15, the total cost will be 9 \* 15 = $135.
    *   **Response**:

```plain
  {
    "success": true,
    "statusCode": 200,
    "message": "Bike returned successfully",
    "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1c4",
      "userId": "60d9c4e4f3b4b544b8b8d1c3",
      "bikeId": "60d9c4e4f3b4b544b8b8d1c4",
      "startTime": "2024-06-10T09:00:00Z",
      "returnTime": "2024-06-10T18:00:00Z",// Current time when returning the bike
      "totalCost": 135, // 
alculated based on r
ntal duration
      "isR
turned": true
    }
  }
  
Get All Rentals for Use
 (My rentals)
```

*       *   **Route**: /api/rentals (GET)

```plain
Request Header
```

*       *   : Authorization: Bearer jwt\_token

```plain
  Response:
  {
    "success": true,
    "statusCode": 200,
   
"message": "Rentals retrieved successfully
,
    "data": [
      {
        "_id": "60d9c4e4f3b4b544b8b8d1c4",
        "user": "60d9c4
4f3b4b544b8b8d1c3",
        "bike": "60d9c4e4f3b4b544b8b8d1c4",
  
     "startTime": "2024-
6-10T
9:0
:0
Z",
        "returnTime": "2024-06-10T18:00:00Z",
        "totalCost": 135,
        "isReturned": true
      },
      ...other rentals..

    ]
  }
  
Bonus Part: 
```

### 10 Marks)

```plain
Middleware a
```

### d Error Handling:

1. **No Data Found**:

```plain
When finding da
a, if the dat
base co
lection is empty or does not match any dat
, return a generic message: "No data found.
```

*       *       *   **Response**:

```json
    {
      "success": false,
```

```plain
      "messag
```

```json
": "No Data Found",
      "data": []
    }
    
```

```plain
Error Ha
```

1. **dling**:

```plain
Implement proper error handling throug
out the application. Use 
lobal error handling middl
ware to 
atch and handle errors, p
ovidi
g a
pr
```

*       *   priate error responses error messages.
    *   Error Response Object should include the following properties:
        *   **success**: false
        *   **message**: Error Type (e.g., Validation Error, Cast Error, Duplicate Entry)
        *   **errorMessages**:

```prolog
    [
      {
        "path": "",
        "message": "Error message"
      }
    ]
    
```

*       *       *   **Sample Error Response**:

```swift
    {
      "success": false,
      "message": "E11000 duplicate key error collection:: email_1 dup key: { email: \\\\"user2@gmail.com\\\\" }",
      "errorMessages": [
        {
          "path": "",
          "message": "E11000 duplicate key error index: email_1 dup key: { email: \\\\"user2@gmail.com\\\\" }"
        }
      ],
      "stack": "error stack"
    }
    
```

1. **Not Found Route**:

```plain
Implement a global "Not Found" handl
r for u
matched routes. Whe
 a route is not found, respond wit
 a gene
ic me
sage
```

*       *   "Not Found."
        *   **Response**:

```json
    {
      "success": false,
      "statusCode": 404,
      "message": "Not Found"
    }
    
```

### Middleware:

1. **Authentication Middleware**:
    *   Ensures that the user is authenticated by checking the JWT token in the request header.
2. **Admin Middleware**:
    *   Ensures that only users with admin privileges can access certain routes.
3. **Global Error Handling Middleware**:
    *   Captures and handles all errors, sending a consistent error response format.

### Implementation Guide:

1. **Initialize Project**:
    *   Initialize a new TypeScript project with Express, Mongoose, and other necessary dependencies.
    *   Set up the folder structure with directories for models, controllers, routes, and middleware.
2. **Configure Mongoose**:
    *   Connect to a MongoDB database using Mongoose.
3. **Develop Models**:
    *   Define Mongoose schemas and models for User, Bike, and Rental.
4. **Set Up Authentication**:
    *   Implement user registration and login functionality.
    *   Secure routes with authentication middleware.
5. **Develop CRUD Operations**:
    *   Implement routes and controllers for CRUD operations on Bikes and Rentals.
6. **Implement Error Handling**:
    *   Create middleware for global error handling.

### Submission Instructions:

*   Ensure your code is well-documented and follows best practices.
*   Submit a link to your GitHub repository containing the complete project.

### Evaluation Criteria:

*   **Correctness**: The API endpoints work as described and handle edge cases properly.
*   **Code Quality**: The code is well-structured, readable, and follows best practices.
*   **Error Handling**: The application gracefully handles errors and provides meaningful error messages.
*   **Authentication & Authorization**: Secure handling of user authentication and role-based access control.
*   **Documentation**: The codebase and API are well-documented.