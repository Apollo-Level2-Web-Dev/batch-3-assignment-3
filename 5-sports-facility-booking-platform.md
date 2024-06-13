# Sports Facility Booking Platform

## Story
Once upon a time in a bustling city, there was Alex, a sports lover with a big dream. He wanted to create a simple way for people to book sports facilities hassle-free. With his trusty team of developers, they set out to make this dream a reality.

Using TypeScript, Express.js, and Mongoose, they built the backbone of their platform. They created models for users, facilities, and bookings, making sure to include all the necessary details.

Next came the fun part—designing the API endpoints. Users could sign up, log in, and book facilities with ease. Admins had extra powers to manage facilities and view all bookings.

But it wasn't all smooth sailing. They encountered challenges along the way, like handling errors and ensuring security. However, with determination and teamwork, they overcame each obstacle.

After months of hard work, their platform was finally ready. People could now book sports facilities with just a few clicks. Alex and his team celebrated their success, proud of what they had accomplished together. And so, their simple idea turned into a solution that made a big difference in the world of sports.

---
  

You are tasked with developing the backend for a sports facility booking platform. This assignment focuses on implementing the following key functionalities: Error Handling, CRUD operations, Authentication & Authorization, and Transaction & Rollback if needed.

  

## Technology Stack:

*   **Programming Language**: TypeScript
*   **Web Framework**: Express.js
*   **ODM & Validation Library**: Mongoose for MongoDB

  

## Main Requirements (50 Marks)

  

### Models:

  

**User Model:**

*   `name`: The name of the user.
*   `email`: The contact email address.
*   `password`: The account password (must be hashed).
*   `phone`: The contact phone number.
*   `role`: The role of the user (can be 'admin' or 'user').
*   `address`: The physical address.

  

**Facility Model:**

*   `name`: The title of the facility.
*   `description`: A brief description of the facility.
*   `pricePerHour`: The cost of booking the facility per hour.
*   `location`: The physical location of the facility.
*   `isDeleted`: Boolean indicating if the facility is marked as deleted (false means not deleted).

  

**Booking Model:**

*   `date`: The date of the booking.
*   `startTime`: The start time of the booking.
*   `endTime`: The end time of the booking.
*   `user`: Reference to the user who made the booking.
*   `facility`: Reference to the booked facility.
*   `payableAmount`: The calculated amount payable for the booking.
*   `isBooked`: Status of the booking (confirmed, unconfirmed, or canceled).

  

### Payable Amount Calculation:

  

Formula:

  

```plain
payableAmount = (endTime - startTime) * pricePerHour
```

  

Assume `endTime` and `startTime` are datetime objects and `pricePerHour` is a numeric value representing the cost per hour.

  

**Example:**

*   Facility's price per hour: 20 TK
*   Booking start time: 10:00 AM
*   Booking end time: 1:00 PM

  

Calculation:

*   Duration: 3 hours
*   Payable amount: 3 hours \* 20 TK/hour = 60 TK

  

### API Endpoints

  

#### User Routes

  

1. **User Sign Up**
*   **Route**: `POST /api/auth/signup`
*   **Request Body**:

```json
{
  "name": "Programming Hero",
  "email": "web@programming-hero.com",
  "password": "programming-hero",
  "phone": "01322901105",
  "role": "admin", // or 'user'
  "address": "Level-4, 34, Awal Centre, Banani, Dhaka"
}
```

* **Response:**
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

2. **User Login**
*   **Route**: `POST /api/auth/login`
*   **Request Body**:

```json
{
  "email": "web@programming-hero.com",
  "password": "programming-hero"
}
```

* **Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "User logged in successfully",
  "token": "JWT_TOKEN",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c4",
    "name": "Programming Hero",
    "email": "web@programming-hero.com",
    "role": "admin",
    "phone": "01322901105",
    "address": "Level-4, 34, Awal Centre, Ban Myeni, Dhaka"
  }
}
```

  


3. **Create a Facility (Admin Only)**
*   **Route**: `POST /api/facility`
*   **Headers**:

```plain
Authorization: Bearer JWT_TOKEN
```

```json
{
  "name": "Tennis Court",
  "description": "Outdoor tennis court with synthetic surface.",
  "pricePerHour": 30,
  "location": "456 Sports Ave, Springfield"
}
```

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
    "isDeleted": false
  }
}
```


  

4. **Update a Facility (Admin Only)**
*   **Route**: `PUT /api/facility/:id`
*   **Headers**:

```plain
Authorization: Bearer JWT_TOKEN
```

```json
{
  "name": "Updated Tennis Court",
  "description": "Updated outdoor tennis court with synthetic surface.",
  "pricePerHour": 35,
  "location": "789 Sports Ave, Springfield"
}
```

* **Response**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Facility updated successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c5",
    "name": "Updated Tennis Court",
    "description": "Updated outdoor tennis court with synthetic surface.",
    "pricePerHour": 35,
    "location": "789 Sports Ave, Springfield",
    "isDeleted": false
  }
}
```

5. **Delete a Facility - Soft Delete (Admin Only)**
*   **Route**: `DELETE /api/facility/:id`
*   **Headers**:

```plain
      Authorization: Bearer JWT_TOKEN
```

*   **Response**:

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Facility deleted successfully",
  "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1c5",
      "name": "Updated Tennis Court",
      "description": "Updated outdoor tennis court with synthetic surface.",
      "pricePerHour": 35,
      "location": "789 Sports Ave, Springfield",
      "isDeleted": true
    }
}

```

**6\. Get All Facilities**

*  **Route**: `GET /api/facility`
*  **Response**:

```json
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
    }
  ]
}
```

  

#### Booking Routes

  
### 7\. Check Availability

Check the availability of time slots for booking on a specific date.

*   **Route**: `GET /api/check-availability`

#### Query Parameters

*   **date** (`string`, optional): The date for which availability is to be checked. Format: `YYYY-MM-DD`. If not provided, today's date will be used by default.

#### Response

  *   **success** (`boolean`): Indicates whether the request was successful.
  *   **statusCode** (`number`): HTTP status code of the response.
  *   **message** (`string`): Descriptive message indicating the outcome of the request.
  *   **data** (`Array` of `Object`): Array containing information about available time slots.

##### Time Slot Object

  *   **startTime** (`string`): The start time of the available slot.
  *   **endTime** (`string`): The end time of the available slot.

#### Example Request

```sql
GET /api/check-availability?date=2024-06-15
```

#### Example Response

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Availability checked successfully",
  "data": [
      {
          "startTime": "08:00",
          "endTime": "10:00"
      },
      {
          "startTime": "14:00",
          "endTime": "16:00"
      }
   ]
}
```

  

**Hints**:

*   Parse the optional `date` query parameter from the request URL. If not provided, use today's date.
*   Retrieve bookings for the specified date from your database using Mongoose.
*   Define a function to find available time range based on the bookings retrieved. Compare booked time range to the total available time slots for the day.
*   Return a response containing the available time slots in the specified format. Use `res.json()` to send the response.

  

  

**8\. Create a Booking (User Only)**

  *   **Route**: `POST /api/bookings`
  *   **Headers**:

```plain
Authorization: Bearer JWT_TOKEN
```

```json
{
  "facility": "60d9c4e4f3b4b544b8b8d1c5",
  "date": "2024-06-15",
  "startTime": "10:00",
  "endTime": "13:00"
}
```
* **Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Booking created successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c6",
    "facility": "60d9c4e4f3b4b544b8b8d1c5",
    "date": "2024-06-15",
    "startTime": "10:00",
    "endTime": "13:00",
    "user": "60d9c4e4f3b4b544b8b8d1c4",
    "payableAmount": 90,
    "isBooked": "confirmed"
  }
}
```

`If the facility is unavailable during the requested time slot, an error response is returned.`

**9\. View All Bookings (Admin Only)**

  *   **Route**: `GET /api/bookings`
  *   **Headers**:

```plain
Authorization: Bearer JWT_TOKEN
```

* **Response:**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Bookings retrieved successfully",
  "data": [
    {
      "_id": "60d9c4e4f3b4b544b8b8d1c6",
      "facility": {
        "_id": "60d9c4e4f3b4b544b8b8d1c5",
        "name": "Tennis Court",
        "description": "Outdoor tennis court with professional-grade surface.",
        "pricePerHour": 30,
        "location": "123 Main Street",
        "isDeleted": false
      },
      "date": "2024-06-15",
      "startTime": "10:00",
      "endTime": "13:00",
      "user": {
        "_id": "60d9c4e4f3b4b544b8b8d1c4",
        "name": "Programming Hero",
        "email": "programming.hero@example.com",
        "phone": "+1234567890",
        "role": "user",
        "address": "456 Elm Street"
      },
      "payableAmount": 90,
      "isBooked": " confirmed"
    }
  ]
}
```

**10\. View Bookings by User (User Only)**

  *   **Route**: `GET /api/bookings/user`
  *   **Headers**:

```plain
Authorization: Bearer JWT_TOKEN
```

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Bookings retrieved successfully",
  "data": [
    {
      "_id": "60d9c4e4f3b4b544b8b8d1c6",
      "facility": {
        "_id": "60d9c4e4f3b4b544b8b8d1c5",
        "name": "Tennis Court",
        "description": "Outdoor tennis court with professional-grade surface.",
        "pricePerHour": 30,
        "location": "123 Main Street",
        "isDeleted": false
      },
      "date": "2024-06-15",
      "startTime": "10:00",
      "endTime": "13:00",
      "user": "60d9c4e4f3b4b544b8b8d1c4",
      "payableAmount": 90,
      "isBooked": " confirmed"
    }
  ]
}
```

**11\. Cancel a Booking (User Only)**

  *   **Route**: `DELETE /api/bookings/:id`
  *   **Headers**:

```plain
Authorization: Bearer JWT_TOKEN
```

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Booking cancelled successfully",
  "data": {
      "_id": "60d9c4e4f3b4b544b8b8d1c6",
      "facility": {
        "_id": "60d9c4e4f3b4b544b8b8d1c5",
        "name": "Tennis Court",
        "description": "Outdoor tennis court with professional-grade surface.",
        "pricePerHour": 30,
        "location": "123 Main Street",
        "isDeleted": false
      },
      "date": "2024-06-15",
      "startTime": "10:00",
      "endTime": "13:00",
      "user": "60d9c4e4f3b4b544b8b8d1c4",
      "payableAmount": 90,
      "isBooked": "canceled"
    }
}
```

  

## Bonus Part (10 Marks):

### **1\. No Data Found:**

When retrieving data, if the database collection is empty or no matching data is found, return the message: "No data found."

```json
{
  "success": false,
  "statusCode": 404,
  "message": "No Data Found",
  "data":[]
}
```

### **2.Error Handling:**

Implement proper error handling throughout the application. Use global error handling `middleware` to catch and handle errors, providing appropriate error responses with error messages.

**Error Response Object Should include the following properties:**

*   success → false
*   message → Error Type → Validation Error, Cast Error, Duplicate Entry
*   errorMessages
*   stack

  

**Sample Error Response**

```json
   {
    "success": false,
    "message": "E11000 duplicate key error collection: univerity-management.students index: email_1 dup key: { email: \\"user2@gmail.com\\" }",
    "errorMessages": [
        {
            "path": "",
            "message": "E11000 duplicate key error collection: project index: email_1 dup key: { email: \\"user2@gmail.com\\" }"
        }
    ],
    "stack": "MongoServerError: E11000 duplicate key error collection: project index: email_1 dup key: { email: \\"user2@gmail.com\\" }\\n    at H:\\\\next-level-development\\\\project-management-auth-service\\\\node_modules\\\\mongodb\\\\src\\\\operations\\\\insert.ts:85:25\\n    at H:\\\\next-level-development\\\\university-management-auth-service\\\\node_modules\\\\mongodb\\\\src\\\\cmap\\\\connection_pool.ts:574:11\\n    at H:\\\\next-level-development\\\\university-writeOrBuffer (node:internal/streams/writable:391:12)"
}
```

###   

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
