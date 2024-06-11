# Car Rental Reservation System

## Assignment Name: Car Rental Reservation System Backend

You have been assigned the task of building the backend for a Car Renting System. The main focus of this assignment is to implement error handling, CRUD operations, authentication, and authorization, Transaction & Rollback (If necessary)

## Story

Imagine you own a car rental business. To keep track of your cars and manage customer rentals, you've decided to built a web app with a database designed based on the following models:

- User Model
- Car Model
- Booking Model

With these models as the foundation, you can build a powerful web app for your car rental business. This app will allow both admins and users to interact smoothly with the booking system:

Both users and administrators need to register and log in to the car rental web app before performing any actions. This ensures a secure and controlled environment for managing rentals.

**Admin Actions:**

**Car Management**: Admins can create new car entries in the system, specifying details like name, color, features, etc. They can also update existing car information to keep things accurate. Additionally, admins can perform "soft deletes" on cars that are no longer available for rent. This keeps a record of the car but removes it from active listings. 

**Booking Oversight**:  Admins have a comprehensive view of all ongoing and past bookings within the system. This allows them to monitor rental activity and identify any potential issues.


**Ride Cost Calculation**: For completed rentals (where the end time has been entered by admin), admins can calculate the total cost using startTime , endTime  and pricePerHour  to ensure accurate billing.

**User’s Actions:**

**Book a Ride**: Users can select their pick-up entering carId and startTime to book the perfect car for their needs.

**Rental Histor**y: They can easily access their booking history, allowing them to review past rentals.

---


## Technology Stack:

*   Use TypeScript as the programming language.
*   Use Express.js as the web framework.
*   Use Mongoose as the Object Data Modeling (ODM) and validation library for MongoDB

## Main Part: (50 Marks)

  

## **Model:**

### **User Model:**

*   **name**: The name of the entity or user.
*   **email**: The contact email address.
*   **role**: There will be two types of users:
    *   user
    *   admin
*   **password**: The account password.
*   **phone**: The contact phone number.
*   **address**: The physical address.

  

### **Car Model:**

*   **name**: The name of the car.
*   **description**: A brief description of what the car entails.
*   **color:** The color of the car.
*   **isElectric**: Boolean to indicate if the car is electric.
*   **status**: The availability of the car. By default, the status will be `available.`
*   **features**: An array listing the features of the car (e.g., \["Bluetooth", "AC", "Sunroof"\]).
*   **pricePerHour**: The cost per hour of the booking in the local currency.
*   **isDeleted**: Indicates whether the car has been marked as deleted (false means it is not deleted).

  

### **Booking Model:**

*   **date**: The date of the booking.
*   **user**: Identifier for the user. **_(reference to user model)_**
*   **car**: Identifier for the booked car. **_(reference to car model)_**
*   **startTime**: The start time of the booking. The time will be in 24hr format.
*   **endTime:** The end time of the booking. The time will be in 24hr format.
*   **totalCost**: The total cost will be calculated using `startTime`, `endTime` and `pricePerHour` data. By default totalCost will be `0`.
*   **isBooked**: Indicates the booking status, whether it's `unconfirmed` or `confirmed` . By default, it will be `unconfirmed`.

  

## API Endpoints

### 1\. Sign Up

**Route**: `/api/auth/signup` (**POST**)

**Request Body:**

```json
{
  "name": "John Doe",
  "email": "johndoe@example.com",
  "role": "user",  // role can be user or admin
  "password": "password123",
  "phone": "1234567890",
  "address": "123 Main St, City, Country"
 
}
```

**Response**:

```json
{
  "success": true,
  "statusCode": 201,
  "message": "User registered successfully",
  "data": {
    "_id": "6071f0fbf98b210012345678",
    "name": "John Doe",
    "email": "johndoe@example.com",
    "role": "user",
    "phone": "1234567890",
    "address": "123 Main St, City, Country",
    "createdAt": "2024-06-10T12:00:00.000Z",
    "updatedAt": "2024-06-10T12:00:00.000Z"
  }
}
```

###   

### 2\. Sign In

**Route**: `/api/auth/signin`(**POST**)

**Request Body:**

```json
{
  "email": "johndoe@example.com",
  "password": "password123"
}
```

**Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "User logged in successfully",
  "data": {
    "_id": "6071f0fbf98b210012345678",
    "name": "John Doe",
    "email": "johndoe@example.com",
    "role": "user",
    "phone": "1234567890",
    "address": "123 Main St, City, Country",
    "createdAt": "2024-06-10T12:00:00.000Z",
    "updatedAt": "2024-06-10T12:00:00.000Z"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9... (your JWT token)"
}
```

### 3\. Create a Car (Only accessible to the Admin)

**Route**: `/api/cars`(**POST**)

**Request Headers:**

```javascript
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! Do not copy and apply directly from the module. If you blindly follow the modules, you will be a copy master, not a developer.
```

  

**Request Body:**

```json
{
  "name": "Tesla Model 3",
  "description": "An electric car with advanced technology and performance.",
  "color": "White",
  "isElectric": true,
  "features": ["AC", "Bluetooth", "Long Range Battery"],
  "pricePerHour": 500
}
```

**Response:**

```json
{
  "success": true,
  "statusCode": 201,
  "message": "Car created successfully",
  "data": {
     "_id": "608a6d8d03a1b40012abcdef",
    "name": "Tesla Model 3",
    "description": "An electric car with advanced technology and performance.",
    "color": "White",
    "isElectric": true,
    "features": ["AC", "Bluetooth", "Long Range Battery"],
    "pricePerHour": 500,
    "status": "available",
    "isDeleted": false,
    "createdAt": "2024-04-28T12:00:00.000Z",
    "updatedAt": "2024-04-28T12:00:00.000Z"
  }
}
```

### 4\. Get All Cars

**Route**: `/api/cars`(**GET**)

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Cars retrieved successfully",
  "data": [
    {
      "_id": "608a6d8d03a1b40012abcdef",
      "name": "Tesla Model 3",
      "description": "An electric car with advanced technology and performance.",
      "color": "White",
      "isElectric": true,
      "features": ["AC", "Bluetooth", "Long Range Battery"],
      "pricePerHour": 500,
      "status": "available",
      "isDeleted": false,
      "createdAt": "2024-04-28T12:00:00.000Z",
      "updatedAt": "2024-04-28T12:00:00.000Z"
    },
    // more data
  ]
}
```

  

### 5\. Get A Car

**Route**: `/api/cars/:id`(**GET**)

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "A Car retrieved successfully",
  "data": {
    "_id": "608a6d8d03a1b40012abcdef",
    "name": "Tesla Model 3",
    "description": "An electric car with advanced technology and performance.",
    "color": "White",
    "isElectric": true,
    "features": ["AC", "Bluetooth", "Long Range Battery"],
    "pricePerHour": 500,
    "status": "available",
    "isDeleted": false,
    "createdAt": "2024-04-28T12:00:00.000Z",
    "updatedAt": "2024-04-28T12:00:00.000Z"
  }
}
```

  

### **6\. Update A Car (Only Accessible to the Admin)**

**Route:** `/api/cars/:id`(**PUT**)

**Request Headers:**

```javascript
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! 
```

**Request Body:**

```json
{
     "color": "Black",  // we will have to update all the fields including name, description, color, isElectric, features, pricePerHour, etc.
}
```

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Car updated successfully",
  "data": {
    "_id": "608a6d8d03a1b40012abcdef",
    "name": "Tesla Model 3",
    "description": "An electric car with advanced technology and performance.",
    "color": "Black",
    "isElectric": true,
    "features": ["AC", "Bluetooth", "Long Range Battery"],
    "pricePerHour": 500,
    "status": "available",
    "isDeleted": false,
    "createdAt": "2024-04-28T12:00:00.000Z",
    "updatedAt": "2024-04-29T12:00:00.000Z"
  }   
}
```

###   

### **7\. Delete A Car (Only Accessible to the Admin)**

**Route:** `/api/cars/:id`(**DELETE**) \[SOFT DELETE\]

**Request Headers:**

```javascript
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! 
```

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Car Deleted successfully",
  "data": {
    "_id": "608a6d8d03a1b40012abcdef",
    "name": "Tesla Model 3",
    "description": "An electric car with advanced technology and performance.",
    "color": "Black",
    "isElectric": true,
    "features": ["AC", "Bluetooth", "Long Range Battery"],
    "pricePerHour": 500,
    "status": "available",
    "isDeleted": true,
    "createdAt": "2024-04-28T12:00:00.000Z",
    "updatedAt": "2024-05-29T12:00:00.000Z"
  }   
}
```

  

### **8\. Get All Bookings (Accessible to the Admin)**

**Route:** `/api/bookings`(**GET**)

**Query Parameters:**

*   `carId`: ID of the car for which availability needs to be checked.
*   `date`: The specific date for which availability needs to be checked (format: YYYY-MM-DD).
*   `isBooked`: Filter the bookings based on their booking status. Possible values are:
    *   `unconfirmed`: Booking that are unconfirmed.
    *   `confirmed`: Booking that are confirmed.

  

Example Request:

`/api/bookings?carId=608a6d8d03a1b40012abcdef&date=2024-06-15&isBooked=unconfirmed`

  

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Bookings retrieved successfully",
  "data": [
    {
      "_id": "60d9c4e4f3b4b544b8b8d1c7",
      "date": "2024-06-15",
      "startTime": "13:00",
      "endTime": null, // it will be null by default, when booked. It will be updated by the admin, when the car is returned.
      "user": {
          "_id": "6071f0fbf98b210012345688",
          "name": "Tom",
          "email": "tom@example.com",
          "role": "user",
          "phone": "1234567890",
          "address": "123 Main St, City, Country",
      },
      "car": {
        "_id": "608a6d8d03a1b40012abcdef",
        "name": "Tesla Model 3",
        "description": "An electric car with advanced technology and performance.",
        "color": "White",
        "isElectric": true,
        "features": ["AC", "Bluetooth", "Long Range Battery"],
        "pricePerHour": 500,
        "status": "unavailable",
        "isDeleted": false,
        "createdAt": "2024-04-28T12:00:00.000Z",
        "updatedAt": "2024-04-28T12:00:00.000Z"
      },
      "totalCost": 0, // it will be 0 by default, when booked. It will be calculate and update by the admin, when the car is returned.
      "isBooked": "confirmed", 
      "createdAt": "2024-04-28T12:00:00.000Z",
      "updatedAt": "2024-05-29T12:00:00.000Z"
    },
    //...additional bookings...
  ]
}
```

  

### **9\. Book a Car (Only Accessible to the User)**

**Route:** `/api/bookings`(**POST**)

**Request Headers:**

```javascript
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token!
```

  

**Request Body:**

```json
{
   "carId": "60d9c4e4f3b4b544b8b8d1c7",
   "date": "2024-06-15",
   "startTime": "13:00",
}
```

  

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Car booked successfully",
  "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1c7",
      "date": "2024-06-15",
      "startTime": "13:00",
      "endTime": null, // it will be null by default, when booked. It will be updated by the admin, when the car is returned.
      "user": {
          "_id": "6071f0fbf98b210012345688",
          "name": "Tom",
          "email": "tom@example.com",
          "role": "user",
          "phone": "1234567890",
          "address": "123 Main St, City, Country",
      },
      "car": {
        "_id": "608a6d8d03a1b40012abcdef",
        "name": "Tesla Model 3",
        "description": "An electric car with advanced technology and performance.",
        "color": "White",
        "isElectric": true,
        "features": ["AC", "Bluetooth", "Long Range Battery"],
        "pricePerHour": 500,
        "status": "unavailable", 
        "isDeleted": false,
        "createdAt": "2024-04-28T12:00:00.000Z",
        "updatedAt": "2024-04-28T12:00:00.000Z"
      },
      "totalCost": 0, // it will be 0 by default, when booked. It will be calculate and update by the admin, when the car is returned.
      "isBooked": "confirmed", 
      "createdAt": "2024-04-28T12:00:00.000Z",
      "updatedAt": "2024-05-29T12:00:00.000Z"
    }
}
```

  

### **10\. Get User's Bookings (Only Accessible To the User)**

**Route:** `/api/bookings/my-bookings`(**GET**)

**Request Headers:**

```javascript
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! 


```

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "My Bookings retrieved successfully",
  "data": [
    {
      "_id": "60d9c4e4f3b4b544b8b8d1c7",
      "date": "2024-06-15",
      "startTime": "13:00",
      "endTime": "15:00",
      "user": {
          "_id": "6071f0fbf98b210012345688",
          "name": "Tom",
          "email": "tom@example.com",
          "role": "user",
          "phone": "1234567890",
          "address": "123 Main St, City, Country",
      },
      "car": {
        "_id": "608a6d8d03a1b40012abcdef",
        "name": "Tesla Model 3",
        "description": "An electric car with advanced technology and performance.",
        "color": "White",
        "isElectric": true,
        "features": ["AC", "Bluetooth", "Long Range Battery"],
        "pricePerHour": 500,
        "status":"unavailable",
        "isDeleted": false,
        "createdAt": "2024-04-28T12:00:00.000Z",
        "updatedAt": "2024-04-28T12:00:00.000Z"
      },
      "totaCost":1000,
      "isBooked": "confirmed ",
      "createdAt": "2024-04-28T12:00:00.000Z",
      "updatedAt": "2024-05-29T12:00:00.000Z"
    },
   // ...additional bookings...
  ]
}
```

##   

## **11\. Return The Car (Only Accessible To Admin)**


**Route:** `/api/cars/return`(PUT)

**Request Headers:**

```javascript
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token!
```

**Request Body:**

```json
{
   "bookingId": "60d9c4e4f3b4b544b8b8d1c7",
   "endTime": "15:00"
}
```

  

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Car booked successfully",
  "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1c7",
      "date": "2024-06-15",
      "startTime": "13:00",
      "endTime": "15:00",
      "user": {
          "_id": "6071f0fbf98b210012345688",
          "name": "Tom",
          "email": "tom@example.com",
          "role": "user",
          "phone": "1234567890",
          "address": "123 Main St, City, Country",
        },
      "car": {
        "_id": "608a6d8d03a1b40012abcdef",
        "name": "Tesla Model 3",
        "description": "An electric car with advanced technology and performance.",
        "color": "White",
        "isElectric": true,
        "features": ["AC", "Bluetooth", "Long Range Battery"],
        "pricePerHour": 500,
        "status": "available" // The status of the car is updated to "available" indicating it is now ready for booking, following its return. 
        "isDeleted": false,
        "createdAt": "2024-04-28T12:00:00.000Z",
        "updatedAt": "2024-04-28T12:00:00.000Z"
      },
      "totalCost":1000, //Calculated using the start time, end time, and price per hour.
      "isBooked": "confirmed",
      "createdAt": "2024-04-28T12:00:00.000Z",
      "updatedAt": "2024-05-29T12:00:00.000Z"
    }
}


```

## Hints for `totalCost` calculation:

*   **Convert Times to Hours:** The `startTime` and `endTime` are in `24 hour` format. Convert them to hours for calculating.
*   **Calculate Duration:** Subtract `startTime` from `endTime` to find the total duration in hours.
*   **Multiply by Price per Hour:** Once you have the duration in hours, multiply it by the`pricePerHour` to get the total cost.

  

## Bonus Part:

### **1\. No Data Found:**

When retrieving data, if the database collection is empty or no matching data is found, return the message: "No data found."

```elixir
{
  "success": false,
  "statusCode": 404,
  "message": "No Data Found",
  "data": []
}
```

### **2\. Error Handling:**

Implement proper error handling throughout the application. Use global error handling `middleware` to catch and handle errors, providing appropriate error responses with status codes and error messages.

  

**Error Response Object Should include the following properties:**

*   success → false
*   message → Error Type → Validation Error, Cast Error, Duplicate Entry
*   errorMessages
*   stack

  

#### **Sample Error Response**

```json
{
    "success": false,
    "message": "E11000 duplicate key error collection: univerity-management.students index:
                email_1 dup key: { email: \\"user2@gmail.com\\" }",
    "errorMessages": [
        {
            "path": "",
            "message": "E11000 duplicate key error collection: univerity-management.students
                        index: email_1 dup key: { email: \\"user2@gmail.com\\" }"
        }
    ],
    "stack": "MongoServerError: E11000 duplicate key error collection: univerity-
              management.students index: email_1 dup key: { email: \\"user2@gmail.com\\" }\\n 
              at H:\\\\next-level-development\\\\university-management-auth-
              service\\\\node_modules\\\\mongodb\\\\src\\\\operations\\\\insert.ts:85:25\\n 
              at H:\\\\next-level-development\\\\university-management-auth-
             service\\\\node_modules\\\\mongodb\\\\src\\\\cmap\\\\connection_pool.ts:574:11\\n  
             at H:\\\\next-level-development\\\\university-writeOrBuffer 
             (node:internal/streams/writable:391:12)"
}
```
 

### **3\. Not Found Route:**

Implement a global "Not Found" handler for unmatched routes. When a route is not found, respond with a generic message: "Not Found.”

```json
{
  "success": false,
  "statusCode": 404,
  "message": "Not Found",
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


