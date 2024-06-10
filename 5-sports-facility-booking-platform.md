# #5 Sports Facility Booking Platform

You have been assigned to build the backend for a Sports facility booking platform. The main focus of this assignment is to implement Error Handling, CRUD operations, Authentication & Authorization ,Transaction & Rollback (if needed)

## Technology Stack:

*   Use TypeScript as the Programming Language.
*   Use Express.js as the web framework.
*   Use `Mongoose` as the Object Data Modeling (ODM) and validation library for MongoDB

## Main Part: (50 Marks)

## Model:

### User Model:

*   **name**: The name of the entity or user.
*   **email**: The contact email address.
*   **password**: The account password (Password must be stored in hash format ).
*   **phone**: The contact phone number.
*   **role**: The role of the user. (`admin`, `user`)
*   **address**: The physical address.

  

### Facility Model:

*   **name**: The title of the facility.
*   **description**: A brief description of the facility.
*   **pricePerHour**: The cost of booking the room per hour in the local currency.
*   **location:** The physical location of the facility.
*   **isDeleted**: Indicates whether the item has been marked as deleted (false means it is not deleted).

### Booking Model:

*   **date:** The date of the booking.
*   **startTime:** The start time of the booking.
*   **endTime:** The end time of the booking.
*   **user:** Identifier for the user who made the booking. **(reference to user model)**
*   **facility:** Identifier for the booked facility. **(reference to facility model)**
*   **payableAmount**: The calculated amount payable for the booking based on the duration and facility's price per hour.
*   **isBooked**: Indicates the booking status, whether it's `confirmed`, `unconfirmed`, or `canceled`.

  

### Explanation of `payableAmount` Calculation:

  

The `payableAmount` can be calculated using the following formula:

**payableAmount**\= (endTime−startTime)×pricePerHour

  

This assumes `endTime` and `startTime` are datetime objects and `pricePerHour` is a numeric value representing the cost per hour.

  

### Example Scenario:

*       *   **Facility's price per hour**: 20 TK
    *   **Booking start time**: 10:00 AM
    *   **Booking end time**: 1:00 PM

### Calculation:

1.     1. **Determine the duration of the booking in hours**:
        *   Start time: 10:00 AM
        *   End time: 1:00 PM
        *   Duration: 1:00 PM - 10:00 AM = 3 hours
2.     1. **Calculate the payable amount**:
        *   Payable amount = Duration in hours × Price per hour
        *   Payable amount = 3 hours × 20 tk / hour
        *   Payable amount = 60 TK

  

## API Endpoints

### User Routes

### 1\. User Sign Up

**Route**: `/api/auth/signup` (**POST**)

**Request Body:**

```json
 
{
  "name": "Programming Hero",
  "email": "web@programming-hero.com",
  "password": "programming-hero",
  "phone": "01322901105",
  "role": "admin",    // role can be user or admin
  "address": "Level-4, 34, Awal Centre, Banani, Dhaka"
} 
```

**Response**:

```json
{
  "success": true,
  "statusCode": 200,
  "message": "User registered successfully",
  "data": {
   "_id": "60d9c4e4f3b4b544b8b8d1c4",
   "name": "Programming Hero",
   "email": "web@programming-hero.com",
   "role": "admin",
   "phone": "01322901105",
   "address": "Level-4, 34, Awal Centre, Banani, Dhaka"
  }
}
```

###   

### 2\. User Login

**Route**: `/api/auth/login`(**POST**)

**Request Body:**

```perl
{
    "email": "web@programming-hero.com",
    "password": "programming-hero",
}
```

  

**Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "User loggedin successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c4",
    "name": "Programming Hero",
    "email": "web@programming-hero.com",
    "role": "admin",
    "phone": "01322901105",
    "address": "Level-4, 34, Awal Centre, Banani, Dhaka"
  }
}
```

###   

### **3\. Create a Facility (Only Accessible by Admin)**

**Route:** `/api/facility`(**POST**)

**Request Headers:**

```javascript
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c


```

You must include "**Bearer**" at the beginning of the token! Do not copy and apply directly from the module. If you blindly follow the modules, you will be a copy master, not a developer.

  

**Request Body:**

```json
{
    "name": "Tennis Court",
    "description": "Outdoor tennis court with synthetic surface.",
    "pricePerHour": 30,
    "location": "456 Sports Ave, Springfield",
    "isDeleted": false
}
```

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Facility added successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c5",
    "name": "Tennis Court",
    "description": "Outdoor tennis court with synthetic surface.",
    "pricePerHour": 30,
    "location": "456 Sports Ave, Springfield",
    "isDeleted": false,
  }  
}
```

###   

### **4\. Get a Facility**

**Route:** `/api/facilities/:id`(**GET**)

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Facility retrieved successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c5",
    "name": "Tennis Court",
    "description": "Outdoor tennis court with synthetic surface.",
    "pricePerHour": 30,
    "location": "456 Sports Ave, Springfield",
    "isDeleted": false
   }
}
```

  

### **5\. Get All Services**

**Route:** `/api/facilities`(**GET**)

**Response Body:**

```plain
{
  "success": true,
  "statusCode": 200,
  "message": "Facilities retrieved successfully",
  "data": [
    {
    "_id": "60d9c4e4f3b4b544b8b8d1c5",
    "name": "Tennis Court",
    "description": "Outdoor tennis court with synthetic surface.",
    "pricePerHour": 30,
    "location": "456 Sports Ave, Springfield",
    "isDeleted": false
   },
     {
    "_id": "60d9c4e4f3b4b544b8b8d1c5",
    "name": "Tennis Court",
    "description": "Outdoor tennis court with synthetic surface.",
    "pricePerHour": 30,
    "location": "456 Sports Ave, Springfield",
    "isDeleted": false
   }
  ]
}
```

  

### **6\. Update Services (Only Accessible by Admin)**

**Route:** `/api/facilities/:id`(**PUT**)

**Request Headers:**

```plain
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! 
```

**Request Body:**

```json
{
    "pricePerHour": 70
}
```

**Response Body:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Facility updated successfully",
  "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1c5",
      "name": "Tennis Court",
       "description": "Outdoor tennis court with synthetic surface.",
       "pricePerHour": 70,
       "location": "456 Sports Ave, Springfield",
       "isDeleted": false
  }   
}
```

  

### **7\. Delete a Service (Only Accessible by Admin)**

**Route:** `/api/facilities/:id`(**DELETE**) \[SOFT DELETE \]

**Request Headers:**

```plain
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
  "message": "Facility deleted successfully",
  "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1c5",
      "name": "Tennis Court",
       "description": "Outdoor tennis court with synthetic surface.",
       "pricePerHour": 70,
       "location": "456 Sports Ave, Springfield",
       "isDeleted":true  //soft delete.
  }
}
```

  

### 8.**Create Slot (Only Accessible by Admin)**

**Route:** `/api/facilities/slots`(**POST**)

**Request Headers:**

```plain
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! 
```

**Request Body:**

```plain
{
    "facility": "60d9c4e4f3b4b544b8b8d1c5",
    "date": "2024-06-15",
    "startTime": "09:00",
    "endTime": "14:00"
}
```

**Response Body:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Slots created successfully",
    "data": [
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c6",
            "facility": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "09:00",
            "endTime": "10:00", //look at the starting point
            "isBooked": "available"
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c7",
            "facility": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "10:00",
            "endTime": "11:00",
            "isBooked": "available"
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c7",
            "facility": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "11:00",
            "endTime": "12:00",
            "isBooked": "available"
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c7",
            "facility": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "12:00",
            "endTime": "13:00",
            "isBooked": "available"
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c7",
            "facility": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "13:00",
            "endTime": "14:00",  //look at the ending point
            "isBooked": "available"
        }
    ]
}
```

### **Hints for creating slots:**

  

1. **Retrieve Service Duration**: Assume the service duration is provided or retrieved from the database. For this example, we'll use a service duration of 60 minutes.
2. **Parse Request Body**: Extract the necessary information from the request body:
    *   Start time: "09:00"
    *   End time: "14:00"
    *   Service duration: 60 minutes
3. **Calculate the Total Duration**:
    *   Convert the start time and end time to minutes since midnight.
    *   Calculate the total duration between the start and end times in minutes.
4. **Generate Slot Time Intervals**:
    *   Determine the number of slots by dividing the total duration by the service duration.
    *   Generate start and end times for each slot.
5. **Example Calculation**
    1. **Step-by-Step Breakdown**:
    2. **Service Duration**: 60 minutes
    3. **Start Time and End Time**:
        *   Start Time: "09:00"
        *   End Time: "14:00"
    4. **Convert Times to Minutes**:
        *   "09:00" → 9 \* 60 + 0 = 540 minutes since midnight
        *   "14:00" → 14 \* 60 + 0 = 840 minutes since midnight
    5. **Calculate Total Duration**:
        *   Total Duration: 840 minutes - 540 minutes = 300 minutes
    6. **Number of Slots**:
        *   Number of Slots: 300 minutes / 60 minutes per slot = 5 slots
    7. **Generate Slot Time Intervals**:
        *   Slot 1: Start Time: "09:00", End Time: "10:00"
        *   Slot 2: Start Time: "10:00", End Time: "11:00"
        *   Slot 3: Start Time: "11:00", End Time: "12:00"
        *   Slot 4: Start Time: "12:00", End Time: "13:00"
        *   Slot 5: Start Time: "13:00", End Time: "14:00"

  

### **9\. Get available slots**

**Route:** `/api/slots/availability`(**GET**)

**Query Parameters:**

*   `date`: (Optional) The specific date for which available slots are requested (format: YYYY-MM-DD).
*   `facilityId`: (Optional) ID of the service for which available slots are requested.

  

**Request Example:**

```plain
  GET /api/slots/availability?date=2024-06-15&facilityId=60d9c4e4f3b4b544b8b8d1c5
```

  

**Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Available slots retrieved successfully",
    "data": [
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c6",
            "facility": 
                     {
                          "_id": "60d9c4e4f3b4b544b8b8d1c5",
                          "name": "Tennis Court",
                           "description": "Outdoor tennis court with synthetic surface.",
                           "pricePerHour": 70,
                           "location": "456 Sports Ave, Springfield",
                           "isDeleted": false
                      },
            "date": "2024-06-15",
            "startTime": "09:00",
            "endTime": "09:30",
            "isBooked": "available"
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c9",
            "facility": 
                       {
                          "_id": "60d9c4e4f3b4b544b8b8d1c5",
                          "name": "Tennis Court",
                           "description": "Outdoor tennis court with synthetic surface.",
                           "pricePerHour": 70,
                           "location": "456 Sports Ave, Springfield",
                           "isDeleted": false
                      },
            "date": "2024-06-15",
            "startTime": "10:00",
            "endTime": "10:30",
            "isBooked": "canceled"
        }
    ]
}
```

  

### **10\. Book a Service (Only Accessible by User)**

**Route:** `/api/bookings`(**POST**)

**Request Headers:**

```plain
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! 
```

**Request Body:**

```plain
{
  "slotId": "60d9c4e4f3b4b544b8b8d1c9", // Slot ID
  "serviceId": "60d9c4e4f3b4b544b8b8d1c5" // Service ID
  "make":"Toyota",
  "model":"Corona Premio",
  "year":"1997",
  "registrationNumber": "Chatto Metro Ga 11-4139",
}
```

**Response Body:**

```plain
{
  "success": true,
  "statusCode": 200,
  "message": "Slot booked successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c9",
    "user": {
      "name": "Programming Hero",
      "email": "web@programming-hero.com",
      "phone": "01322901105",
      "role":"user"
      "address": "Level-4, 34, Awal Centre, Banani, Dhaka"
    }, 
    "slot": {
      "_id": "60d9c4e4f3b4b544b8b8d1c7",
      "date": "2024-06-15",
      "time": "09:00 AM",
      "service": "60d9c4e4f3b4b544b8b8d1c5",
      "isBooked": "confirmed"
    },
    "service": {
       "name": "Basic Wash",
       "description": "Exterior wash and dry",
       "price": 700,
       "duration": 30,
       "isDeleted":false.
       },
   "make":"Toyota",
   "model":"Corona Premio",
   "year":"1997",
   "registrationNumber": "Chatto Metro Ga 11-4139",
   "isBooked": "confirmed"
  }
}
```

  

### **11\. Get All Bookings**

**Route:** `/api/bookings`(**GET**)

**Request Headers:**

```plain
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! 


```

**Response Body:**

```plain
{
  "success": true,
  "statusCode": 200,
  "message": "Bookings retrieved successfully",
  "data": [
    {
      "_id": "60d9c4e4f3b4b544b8b8d1ca",
      "user": {
        "name": "Programming Hero",
        "email": "web@programming-hero.com",
        "phone": "01322901105",
        "address": "Level-4, 34, Awal Centre, Banani, Dhaka"
      },
      "slot": {
        "_id": "60d9c4e4f3b4b544b8b8d1c7",
        "date": "2024-06-15",
        "time": "09:00 AM",
        "service": "60d9c4e4f3b4b544b8b8d1c5",
        "isBooked": "confirmed"
      },
      "service": {
         "name": "Basic Wash",
         "description": "Exterior wash and dry",
         "price": 700,
         "duration": 30,
         "isDeleted":false.
         },
     "make":"Toyota",
     "model":"Corona Premio",
     "year":"1997",
     "registrationNumber": "Chatto Metro Ga 11-4139",
     "isBooked": "confirmed"
    },
    //...additional bookings...
  ]
}
```

  

### **12\. Get User's Bookings (Only Accessible by User)**

**Route:** `/api/my-bookings`(**GET**)

**Request Headers:**

```plain
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! 


```

**Response Body:**

```plain
{
  "success": true,
  "statusCode": 200,
  "message": "Bookings retrieved successfully",
  "data": [
    {
      "_id": "60d9c4e4f3b4b544b8b8d1ca",
      "user": {
        "name": "Programming Hero",
        "email": "web@programming-hero.com",
        "phone": "01322901105",
        "role":"user"
        "address": "Level-4, 34, Awal Centre, Banani, Dhaka"
      },
      "slot": {
        "_id": "60d9c4e4f3b4b544b8b8d1c7",
        "date": "2024-06-15",
        "time": "09:00 AM",
        "service": "60d9c4e4f3b4b544b8b8d1c5",
        "isBooked": "confirmed"
      },
      "service": {
         "name": "Basic Wash",
         "description": "Exterior wash and dry",
         "price": 700,
         "duration": 30,
         "isDeleted":false.
         },
      "isBooked": "confirmed"
    },
    ...additional bookings...
  ]
}


```

###   

## Bonus Part:

### [**1.No**](http://1.no/) **Data Found:**

When retrieving data, if the database collection is empty or no matching data is found, return the message: "No data found."

```elixir
{
  "success": false,
  "message": "No Data Found",
  "data":[]
}
```

### **2.Error Handling:**

Implement proper error handling throughout the application. Use global error handling `middleware` to catch and handle errors, providing appropriate error responses with status codes and error messages.

  

**Error Response Object Should include the following properties:**

*   success → false
*   message → Error Type → Validation Error, Cast Error, Duplicate Entry
*   errorMessages
*   stack

  

**Sample Error Response**

```swift
   {
    "success": false,
    "message": "E11000 duplicate key error collection: univerity-management.students index: email_1 dup key: { email: \\"user2@gmail.com\\" }",
    "errorMessages": [
        {
            "path": "",
            "message": "E11000 duplicate key error collection: univerity-management.students index: email_1 dup key: { email: \\"user2@gmail.com\\" }"
        }
    ],
    "stack": "MongoServerError: E11000 duplicate key error collection: univerity-management.students index: email_1 dup key: { email: \\"user2@gmail.com\\" }\\n    at H:\\\\next-level-development\\\\university-management-auth-service\\\\node_modules\\\\mongodb\\\\src\\\\operations\\\\insert.ts:85:25\\n    at H:\\\\next-level-development\\\\university-management-auth-service\\\\node_modules\\\\mongodb\\\\src\\\\cmap\\\\connection_pool.ts:574:11\\n    at H:\\\\next-level-development\\\\university-writeOrBuffer (node:internal/streams/writable:391:12)"
}
```

###   

### **3.Not Found Route:**

Implement a global "Not Found" handler for unmatched routes. When a route is not found, respond with a generic message: "Not Found.”

```json
{
  "success": true,
  "statusCode": 404,
  "message": "Not Found",
}
```