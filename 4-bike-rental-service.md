# Bike rental service for tourists or locals.

* * *

## Assignment Name: Bike Rental Reservation System Backend

You have been assigned the task of building the backend for a Bike Renting System. The main focus of this assignment is to implement error handling, CRUD operations, authentication, and authorization, Transaction & Rollback (If necessary)
---

### Story

---

In the vibrant coastal town of Cox's Bazar, famous for its long Inani beach, tourism was thriving. The town, with its scenic beauty and lively atmosphere, was a favorite spot for tourists and locals alike. Seeing an opportunity, Jhankar Mahbub, a local entrepreneur with a passion for technology, decided to start a bike rental service to cater to both tourists and locals.

Jhankar Mahbub envisioned a seamless bike rental system where users could easily rent bikes online. He wanted a backend system that would handle user registrations, bike availability, and booking management efficiently. To achieve this, he planned to hire a developer to build the backend for the Bike Rental Reservation System.

**Models:**

1. **User Model**
2. **Bike Model**
3. **Booking Model**

Jhankar's bike rental service quickly became popular. Tourists and locals loved the convenience of booking bikes online and exploring Cox's Bazar. With a robust backend system in place, Jhankar could efficiently manage his business, and his customers enjoyed a seamless rental experience.

---


### Technology Stack:

- **Programming Language**: TypeScript
- **Web Framework**: Express.js
- **ODM & Validation Library**: Zod, Mongoose for MongoDB

### Main Part: (50 Marks)

### Models:

1. **User Model**:
    - **name**: The name of the user.
    - **email**: The contact email address.
    - **password**: The account password.
    - **phone**: The contact phone number.
    - **address**: The physical address.
    - **role**: admin / user
2. **Bike Model**:
    - **name**: The name of the bike model.
    - **description**: A brief description of the bike.
    - **pricePerHour**: The rental price per hour.
    - **isAvailable**: Indicates if the bike is available for rental (default: true).
    - **cc**: The engine capacity of the bike in cubic centimeters.
    - **year**: The manufacturing year of the bike.
    - **model**: The model of the bike.
    - **brand**: The brand of the bike.
3. **Booking Model**:
    - **userId**: Reference to the User model.
    - **bikeId**: Reference to the Bike model.
    - **startTime**: The start time of the rental.
    - **returnTime**: The return time of the rental.
    - **totalCost**: The total cost of the rental.
    - **isReturned**: Indicates if the bike has been returned (default: false).

### API Endpoints:

## **User Routes**:

1. **Sign Up**
    - **Route**: /api/auth/signup (POST)
    - **Request Body**:
        
        ```json
        {
          "name": "John Doe",
          "email": "john@example.com",
          "password": "password123",
          "phone": "1234567890",
          "address": "123 Main St, Anytown",
          "role" : "admin"
        }
        
        ```
        
    - **Response**:
        
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
            "role" : "admin",
            "createdAt": "2024-06-10T13:26:51.289Z",
            "updatedAt": "2024-06-10T13:26:51.289Z",
            "__v": 0
          }
        }
        
        ```
        
2. **User Login**
    - **Route**: /api/auth/login (POST)
    - **Request Body**:
        
        ```json
        {
          "email": "john@example.com",
          "password": "password123"
        }
        
        ```
        
    - **Response**:
        
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
            "address": "123 Main St, Anytown",
            "role" : "admin"
          }
        }
        
        ```
        
3. **Get Profile**
    - **Route**: /api/users/me (GET)
    - **Request Headers**: Authorization: Bearer jwt_token
    - **Response**:
        
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
        
4. **Update Profile**
    - **Route**: /api/users/me (PUT)
    - **Request Headers**: Authorization: Bearer jwt_token
    - **Request Body**:
        
        ```json
        {
          "name": "John Updated",
          "phone": "0987654321"
        }
        
        ```
        
    - **Response**:
        
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
            "address": "123 Main St, Anytown",
            "role" : "admin"
          }
        }
        
        ```
        

## **Bike Routes**:

1. **Create Bike (Admin Only)**
    - **Route**: /api/bikes (POST)
    - **Request Headers**: Authorization: Bearer jwt_token
    - **Request Body**:
        
        ```json
        {
          "name": "Mountain Bike",
          "description": "A durable mountain bike for rough terrains.",
          "pricePerHour": 15,
          "cc": 250,
          "year": 2022,
          "model": "X1",
          "brand": "Yamaha"
        }
        
        ```
        
    - **Response**:
        
        ```json
        {
          "success": true,
          "statusCode": 200,
          "message": "Bike added successfully",
          "data": {
            "_id": "60d9c4e4f3b4b544b8b8d1c4",
            "name": "Mountain Bike",
            "description": "A durable mountain bike for rough terrains.",
            "pricePerHour": 15,
            "isAvailable": true,
            "cc": 250,
            "year": 2022,
            "model": "X1",
            "brand": "Yamaha"
          }
        }
        
        ```
        
2. **Get All Bikes**
    - **Route**: /api/bikes (GET)
    - **Response**:
        
        ```json
        {
          "success": true,
          "statusCode": 200,
          "message": "Bikes retrieved successfully",
          "data": [
            {
              "_id": "bike_id",
              "name": "Mountain Bike",
              "description": "A durable mountain bike for rough terrains.",
              "pricePerHour": 15,
              "isAvailable": true,
              "cc": 250,
              "year": 2022,
              "model": "X1",
              "brand": "Yamaha"
            },
            ...other bikes...
          ]
        }
        
        ```
        
3. **Update Bike (Admin Only)**
    - **Route**: /api/bikes/:id (PUT)
    - **Request Headers**: Authorization: Bearer jwt_token
    - **Request Body**:
        
        ```json
        {
          "pricePerHour": 20
        }
        
        ```
        
    - **Response**:
        
        ```json
        {
          "success": true,
          "statusCode": 200,
          "message": "Bike updated successfully",
          "data": {
            "_id": "bike_id",
            "name": "Mountain Bike",
            "description": "A durable mountain bike for rough terrains.",
            "pricePerHour": 20, // Updated price per hour
            "isAvailable": true,
            "cc": 250,
            "year": 2022,
            "model": "X1",
            "brand": "Yamaha"
          }
        }
        
        ```
        
4. **Delete Bike (Admin Only)**
    - **Route**: /api/bikes/:id (DELETE)
    - **Request Headers**: Authorization: Bearer jwt_token
    - **Response**:
        
        ```json
        {
          "success": true,
          "statusCode": 200,
          "message": "Bike deleted successfully",
          "data": {
            "_id": "bike_id",
            "name": "Mountain Bike",
            "description": "A durable mountain bike for rough terrains.",
            "pricePerHour": 20,
            "isAvailable": false,
            "cc": 250,
            "year": 2022,
            "model": "X1",
            "brand": "Yamaha"
          }
        }
        
        ```
        

## **Rental Routes**:

1. **Create Rental**
    - **Route**: /api/rentals (POST)
    - **Request Headers**: Authorization: Bearer jwt_token
    - User information should be extracted from the token
    - **Bike's availability status should be updated to false**
    - **Request Body**:
        
        ```json
        {
          "bikeId": "60d9c4e4f3b4b544b8b8d1c4",
          "startTime": "2024-06-10T09:00:00Z"
        }
        
        ```
        
    - **Response**:
        
        ```json
        {
          "success": true,
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

    **Important Note:** Upon creating a rental, ensure the bike's isAvailable status is set to false to indicate that it is currently rented out and not available for other users to rent.

1. **Return Bike (Admin Only)**
    - **Route**: /api/rentals/:id/return (PUT)
    - **Request Headers**: Authorization: Bearer jwt_token
    - **Request Body**: Not needed
    - **Bike's availability status should be updated to true**
    - **Hints**: The cost should be calculated based on the start and return time of the rental. For example, if the start time is "2024-06-10T09:00:00Z" and the return time is "2024-06-10T18:00:00Z" (current time), the total rental duration is 9 hours. If the price per hour is $15, the total cost will be 9 * 15 = $135.
    - **Response**:
        
        ```json
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
            "totalCost": 135, // Calculated based on rental duration
            "isReturned": true
          }
        }
        
        ```
        
2. **Get All Rentals for User (My rentals)**
    - **Route**: /api/rentals (GET)
    - **Request Headers**: Authorization: Bearer jwt_token
    - **Response**:
        
        ```json
        {
          "success": true,
          "statusCode": 200,
          "message": "Rentals retrieved successfully",
          "data": [
            {
              "_id": "60d9c4e4f3b4b544b8b8d1c4",
              "userId": "60d9c4e4f3b4b544b8b8d1c3",
              "bikeId": "60d9c4e4f3b4b544b8b8d1c4",
              "startTime": "2024-06-10T09:00:00Z",
              "returnTime": "2024-06-10T18:00:00Z",
              "totalCost": 135,
              "isReturned": true
            },
            ...other rentals...
          ]
        }
        
        ```
        

### Bonus Part: (10 Marks)

### Middleware and Error Handling:

1. **No Data Found**:
    - When finding data, if the database collection is empty or does not match any data, return a generic message: "No data found."
        - **Response**:
            
            ```json
            {
              "success": false,
              "message": "No Data Found",
              "data": []
            }
            
            ```
            
2. **Error Handling**:
    - Implement proper error handling throughout the application. Use global error handling middleware to catch and handle errors, providing appropriate error responses  error messages.
    - Error Response Object should include the following properties:
        - **success**: false
        - **message**: Error Type (e.g., Validation Error, Cast Error, Duplicate Entry)
        - **errorMessages**:
            
            ```json
            [
              {
                "path": "",
                "message": "Error message"
              }
            ]
            
            ```
            
        - **Sample Error Response**:
            
            ```json
            {
              "success": false,
              "message": "E11000 duplicate key error collection:: email_1 dup key: { email: \\"user2@gmail.com\\" }",
              "errorMessages": [
                {
                  "path": "",
                  "message": "E11000 duplicate key error index: email_1 dup key: { email: \\"user2@gmail.com\\" }"
                }
              ],
              "stack": "error stack"
            }
            
            ```
            
3. **Not Found Route**:
    - Implement a global "Not Found" handler for unmatched routes. When a route is not found, respond with a generic message: "Not Found."
        - **Response**:
            
            ```json
            {
              "success": false,
              "statusCode": 404,
              "message": "Not Found"
            }
            
            ```
            
### **4\. Authentication Middleware:**

Implement an Authentication Middleware to authenticate your application. Ensures that only user  and admin can access their own accessible routes.

```json
{
  "success": false,
  "statusCode": 401,
  "message": "You have no access to this route",
}
```

### **5\. Zod Validation:**
The API employs Zod for input validation, ensuring data consistency. When validation fails, a 400 Bad Request status code is returned, accompanied by detailed error messages specifying the erroneous fields and reasons.


