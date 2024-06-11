# Meeting Room Booking System for Co-working spaces

## Story:

Suppose you have an agency where you offer co-working spaces for meetings and discussions. To streamline the booking process, you've decided to develop a web application for managing room reservations. Starting with the backend, you've crafted schemas based on the following models:

1. **User Model**
2. **Room Model**
3. **Slot**
4. **Booking Model**

With these models in place, you empower both admins and users to interact seamlessly with the booking system:

*   **Admin Actions:** Administrators have the privilege of creating, updating, and deleting rooms. They can specify details like the room's name, room number, floor number, capacity, price per slot, and available amenities. Additionally, admins are responsible for creating time slots for each room. They set the date, start time, and end time for these slots, ensuring that users have a range of options to choose from. Through the web interface, admins can effortlessly manage the co-working space inventory and slot availability, ensuring accurate and up-to-date information for users.
*   **User Interactions:** On the user side, individuals can create bookings by selecting from the available time slots for their desired meeting times. They input the date and select specific slots for their sessions, along with their preferred room selection. The system automatically calculates the total amount based on the number of slots selected and the price per slot. Users receive real-time feedback on the availability of rooms and slots, ensuring smooth booking experiences without conflicts.

As the development progresses, you implement robust validation and error handling mechanisms to enhance the application's reliability. Users receive informative messages in case of booking conflicts or validation errors, guiding them towards successful interactions with the platform.

  

## Technology Stack:

*   Use TypeScript as the programming language.
*   Use Express.js as the web framework.
*   Use Mongoose as the Object Data Modeling (ODM) and validation library for MongoDB

## Main Part: (50 Marks)

## Models:

### User Model:

*   `name`: The name of the user.
*   `email`: The contact email address.
*   `password`: The account password.
*   `phone`: The contact phone number.
*   `address`: The physical address.
*   `role`: The role of the user, can be `user` or `admin`.

### Room Model:

*   `name`: The name of the meeting room.
*   `roomNo` : The unique number of the room.
*   `floorNo` : The level of the meeting room where it is located.
*   `capacity`: The maximum number of people the room can accommodate.
*   `pricePerSlot`: The individual cost of a single slot.
*   `amenities`: An array of amenities available in the room (e.g., "Projector", "Whiteboard"). Don't use enum.
*   `isDeleted`: Boolean to indicates whether the room has been marked as deleted (false means it is not deleted).

### Slot Model

*   `room` : Reference to the specific room being booked.
*   `date`: Date of the booking.
*   `startTime`: Start time of the slot.
*   `endTime`: End time of the slot.
*   `isBooked`: Boolean to indicate whether the slot has been marked as booked (false means it is not booked).

### Booking Model:

*   `room`: Identifier for the booked room (a reference to room model).
*   `slots`: An array containing the slot IDs (a reference to the booking slots).
*   `user`: Identifier for the user who booked the room (a reference to the user model).
*   `totalAmount` : The total amount of the bill is calculated based on the selected number of slots.
*   `isConfirmed`: Indicates the booking status, whether it's `confirmed`, `unconfirmed`, or `canceled`

  

### User Routes

  

1. **User Sign Up**
    -   _*Route:*_ `/api/auth/signup` (POST)
    -   **Request Body:**

```json
{
  "name": "Programming Hero",
  "email": "web@programming-hero.com",
  "password": "ph-password",
  "phone": "1234567890",
  "role": "admin", //role can be user or admin
  "address": "123 Main Street, City, Country"
}
  
```

    -   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "User registered successfully",
  "data": {
    "_id": "60629b8e8cfcd926384b6e5e",
    "name": "Programming Hero",
    "email": "web@programming-hero.com",
    "phone": "1234567890",
    "role": "admin",
    "address": "123 Main Street, City, Country"
  }
}
```

**2\. User Login**

*       *   **Route:** `/api/auth/login` (POST)
    *   **Request Body:**

```json
{
    "email": "web@programming-hero.com",
    "password": "ph-password",
}
```

Response:

```json
{
    "success": true,
    "statusCode": 200,
    "message": "User logged in successfully",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI2MDYyOWI4ZThjZmNkOTI2Mzg0YjZlNWUiLCJuYW1lIjoiUHJvZ3JhbW1pbmcgSGVyb3MiLCJlbWFpbCI6IndlYkBwcm9ncmFtbWluZy1oZXJvLmNvbSIsInBob25lIjoiMTIzNDU2Nzg5MCIsInJvbGUiOiJhZG1pbiIsImFkZHJlc3MiOiIxMjMgTWFpbiBTdHJlZXQsIENpdHksIENvdW50cnkiLCJpYXQiOjE2MjQ1MTY2MTksImV4cCI6MTYyNDUyMDYxOX0.kWrEphO6lE9P5tvzrNBwx0sNogNuXpdyG-YoN9fB1W8",
    "data": {
        "_id": "60629b8e8cfcd926384b6e5e",
        "name": "Programming Hero",
        "email": "web@programming-hero.com",
        "phone": "1234567890",
        "role": "admin",
        "address": "123 Main Street, City, Country"
    }
}
```

###   

### Room Routes

  

**3\. Create Room (Only Accessible by Admin)**

*       *   **Route:** `/api/rooms` (POST)
    *   **Request Headers:**

```plain
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! Do not copy and apply directly from the module. If you blindly follow the modules, you will be a copy master, not a developer.
```

*       *   **Request Body:**

```json
{
  "name": "Conference Room",
  "roomNo": 201,
  "floorNo": 1,
  "capacity": 20,
  "pricePerSlot": 100,
  "amenities": ["Projector", "Whiteboard"]
}


```

*       *   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Room added successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c5",
    "name": "Conference Room",
    "roomNo": 201,
    "floorNo": 1,
    "capacity": 20,
    "pricePerSlot": 100,
    "amenities": ["Projector", "Whiteboard"],
    "isDeleted": false
  }
}


```

**4\. Get a Room**

*       *   **Route:** `/api/rooms/:id` (GET)
    *   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Room retrieved successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c5",
    "name": "Conference Room",
    "roomNo": 201,
    "floorNo": 1,
    "capacity": 20,
    "pricePerSlot": 100,
    "amenities": ["Projector", "Whiteboard"],
    "isDeleted": false
  }
}


```

**5\. Get All Rooms**

*       *   **Route:** `/api/rooms` (GET)
    *   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Rooms retrieved successfully",
  "data": [
    {
      "_id": "60d9c4e4f3b4b544b8b8d1c5",
      "name": "Conference Room",
      "roomNo": 201,
      "floorNo": 1,
      "capacity": 20,
      "pricePerSlot": 100,
      "amenities": ["Projector", "Whiteboard"],
      "isDeleted": false
    },
    {
      "_id": "60d9c4e4f3b4b544b8b8d1c6",
      "name": "Meeting Room",
      "roomNo": 301,
      "floorNo": 2,
      "capacity": 10,
      "pricePerSlot": 200,
      "amenities": ["Whiteboard"],
      "isDeleted": false
    }
    // Other available rooms
  ]
}
```

  

**6\. Update Room (Only Accessible by Admin)**

*       *   **Route:** `/api/rooms/:id` (PUT)
    *   **Request Headers:**

```plain
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! Do not copy and apply directly from the module. If you blindly follow the modules, you will be a copy master, not a developer.
```

*       *   **Request Body:**

```json
{
  "pricePerSlot": 200 //we can update any field dynamically, (e.g., name, roomNo, floorNo, capacity, pricePerSlot, amenities)..
}
```

*       *   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Room updated successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c5",
    "name": "Conference Room",
    "roomNo": 201,
    "floorNo": 1,
    "capacity": 20,
    "pricePerSlot": 200,
    "amenities": ["Projector", "Whiteboard"],
    "isDeleted": false
  }
}
```

**7\. Delete a Room (Soft Delete, Only Accessible by Admin)**

*       *   **Route:** `/api/rooms/:id` (DELETE)
    *   **Request Headers:**

```plain
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! Do not copy and apply directly from the module. If you blindly follow the modules, you will be a copy master, not a developer.
```

*       *   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Room deleted successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c5",
    "name": "Conference Room",
    "roomNo": 201,
    "floorNo": 1,
    "capacity": 20,
    "pricePerSlot": 200,
    "amenities": ["Projector", "Whiteboard"],
    "isDeleted": true
  }
}
```

###   

### Slot Routes

  

8\. **Create Slot (Only Accessible by Admin)**

**Route:** `/api/slots`(**POST**)

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
    "room": "60d9c4e4f3b4b544b8b8d1c5",
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
            "room": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "09:00",
            "endTime": "10:00",
            "isBooked": false
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c7",
            "room": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "10:00",
            "endTime": "11:00",
            "isBooked": false
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c8",
            "room": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "11:00",
            "endTime": "12:00",
            "isBooked": false
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c9",
            "room": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "12:00",
            "endTime": "13:00",
            "isBooked": false
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1ca",
            "room": "60d9c4e4f3b4b544b8b8d1c5",
            "date": "2024-06-15",
            "startTime": "13:00",
            "endTime": "14:00",
            "isBooked": false
        }
    ]
}


```

  

**Hints for creating slots:**

1. **Retrieve Slot Duration**: Assume the slot duration is provided or retrieved from the database. For this example, we'll use a slot duration of 60 minutes.
2. **Parse Request Body**: Extract the necessary information from the request body:
    *   Start time: "09:00"
    *   End time: "14:00"
    *   Slot duration: 60 minutes
3. **Calculate the Total Duration**:
    *   Convert the start time and end time to minutes since midnight.
    *   Calculate the total duration between the start and end times in minutes.
4. **Generate Slot Time Intervals**:
    *   Determine the number of slots by dividing the total duration by the service duration.
    *   Generate start and end times for each slot.
5. **Example Calculation - Step-by-Step Breakdown**:
    1. **Slot Duration**: 60 minutes
    2. **Start Time and End Time**:
        *   Start Time: "09:00"
        *   End Time: "14:00"
    3. **Convert Times to Minutes**:
        *   "09:00" → 9 \* 60 = 540 minutes since midnight
        *   "14:00" → 14 \* 60 = 840 minutes since midnight
    4. **Calculate Total Duration**:
        *   Total Duration: 840 minutes - 540 minutes = 300 minutes
    5. **Number of Slots**:
        *   Number of Slots: 300 minutes / 60 minutes per slot = 5 slots
    6. **Generate Slot Time Intervals**:
        *   Slot 1: Start Time: "09:00", End Time: "10:00"
        *   Slot 2: Start Time: "10:00", End Time: "11:00"
        *   Slot 3: Start Time: "11:00", End Time: "12:00"
        *   Slot 4: Start Time: "12:00", End Time: "13:00"
        *   Slot 5: Start Time: "13:00", End Time: "14:00"

  

**9\. Get available slots**

**Route:** `/api/slots/availability`(**GET**)

**Query Parameters:**

*   `date`: The specific date for which available slots are requested (format: YYYY-MM-DD).
*   `roomId`: ID of the room for which available slots are requested.

  

> Special Remarks

If we hit `/api/slots/availability` without any query params then we should get all the slots that are not booked ( isBooked: false)

  

**Request endpoint example**

`/api/slots/availability?date=2024-06-15&roomId=60d9c4e4f3b4b544b8b8d1c5`

or

`/api/slots/availability`

  

**Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Available slots retrieved successfully",
    "data": [
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c6",
            "room": {
                "_id": "60d9c4e4f3b4b544b8b8d1c5",
                "name": "Conference Room",
                "roomNo": 201,
                "floorNo": 1,
                "capacity": 20,
                "pricePerSlot": 100,
                "amenities": ["Projector", "Whiteboard"],
                "isDeleted": false
            },
            "date": "2024-06-15",
            "startTime": "09:00",
            "endTime": "10:00",
            "isBooked": false
        },
        {
            "_id": "60d9c4e4f3b4b544b8b8d1c7",
            "room": {
                "_id": "60d9c4e4f3b4b544b8b8d1c5",
                "name": "Conference Room",
                "roomNo": 201,
                "floorNo": 1,
                "capacity": 20,
                "pricePerSlot": 100,
                "amenities": ["Projector", "Whiteboard"],
                "isDeleted": false
            },
            "date": "2024-06-15",
            "startTime": "10:00",
            "endTime": "11:00",
            "isBooked": false
        }
    ]
}



```

###   

### Booking Routes

  

**10\. Create a Booking (Only Accessible by Authenticated User)**

*       *   **Route:** `/api/bookings` (POST)
    *   **Request Headers:**

```javascript
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! Do not copy and apply directly from the module. If you blindly follow the modules, you will be a copy master, not a developer.
```

*       *   **Request Body:**

```json
{
  "date": "2024-06-15",
  "slots": ["60d9c4e4f3b4b544b8b8d1c6", "60d9c4e4f3b4b544b8b8d1c7"],
  "room": "60d9c4e4f3b4b544b8b8d1c5",
  "user": "60d9c4e4f3b4b544b8b8d1c4"
}
```

*       *   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Booking created successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1c9",
    "date": "2024-06-15",
    "slots": [
      {
        "_id": "60d9c4e4f3b4b544b8b8d1c6",
        "room": "60d9c4e4f3b4b544b8b8d1c5",
        "date": "2024-06-15",
        "startTime": "09:00",
        "endTime": "10:00",
        "isBooked": true
      },
      {
        "_id": "60d9c4e4f3b4b544b8b8d1c7",
        "room": "60d9c4e4f3b4b544b8b8d1c5",
        "date": "2024-06-15",
        "startTime": "10:00",
        "endTime": "11:00",
        "isBooked": true
      }
    ],
    "room": {
      "_id": "60d9c4e4f3b4b544b8b8d1c5",
      "name": "Conference Room",
      "roomNo": 201,
      "floorNo": 1,
      "capacity": 20,
      "pricePerSlot": 100,
      "amenities": ["Projector", "Whiteboard"],
      "isDeleted": false
    },
    "user": {
      "_id": "60d9c4e4f3b4b544b8b8d1c4",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "phone": "1234567890",
      "address": "123 Main St, Anytown, USA",
      "role": "user"
    },
    "totalAmount": 200,
    "isConfirmed": "unconfirmed"
  }
}



```

  

  

**11\. Get All Bookings (Only Accessible by Admin)**

*       *   **Route:** `/api/bookings` (GET)
    *   **Request Headers:**

```plain
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! Do not copy and apply directly from the module. If you blindly follow the modules, you will be a copy master, not a developer.
```

*       *   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "All bookings retrieved successfully",
  "data": [
    {
    "_id": "60d9c4e4f3b4b544b8b8d1c9",
    "date": "2024-06-15",
    "slots": [
      {
        "_id": "60d9c4e4f3b4b544b8b8d1c6",
        "room": "60d9c4e4f3b4b544b8b8d1c5",
        "date": "2024-06-15",
        "startTime": "09:00",
        "endTime": "10:00",
        "isBooked": true
      },
      {
        "_id": "60d9c4e4f3b4b544b8b8d1c7",
        "room": "60d9c4e4f3b4b544b8b8d1c5",
        "date": "2024-06-15",
        "startTime": "10:00",
        "endTime": "11:00",
        "isBooked": true
      }
    ],
    "room": {
      "_id": "60d9c4e4f3b4b544b8b8d1c5",
      "name": "Conference Room",
      "roomNo": 201,
      "floorNo": 1,
      "capacity": 20,
      "pricePerSlot": 100,
      "amenities": ["Projector", "Whiteboard"],
      "isDeleted": false
    },
    "user": {
      "_id": "60d9c4e4f3b4b544b8b8d1c4",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "phone": "1234567890",
      "address": "123 Main St, Anytown, USA",
      "role": "user"
    },
    "totalAmount": 200,
    "isConfirmed": "unconfirmed"
  },
   // other bookings ( If any )
  ]
}


```

**12\. Get User's Bookings (Only Accessible by Authenticated User)**

*       *   **Route:** `/api/my-bookings`(**GET**)
    *   **Request Headers:**

```javascript
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! 
```

*       *   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "User bookings retrieved successfully",
  "data": [
       {
         "_id": "60d9c4e4f3b4b544b8b8d1ca",
         "date": "2024-06-15",
         "slots": [
              {
                "_id": "60d9c4e4f3b4b544b8b8d1c6",
                "room": "60d9c4e4f3b4b544b8b8d1c5",
                "date": "2024-06-15",
                "startTime": "09:00",
                "endTime": "10:00",
                "isBooked": true
              },
              {
                "_id": "60d9c4e4f3b4b544b8b8d1c7",
                "room": "60d9c4e4f3b4b544b8b8d1c5",
                "date": "2024-06-15",
                "startTime": "10:00",
                "endTime": "11:00",
                "isBooked": true
              }
            ],
            "room": {
              "_id": "60d9c4e4f3b4b544b8b8d1c5",
              "name": "Conference Room",
              "roomNo": 201,
              "floorNo": 1,
              "capacity": 20,
              "pricePerSlot": 100,
              "amenities": ["Projector", "Whiteboard"],
              "isDeleted": false
            },
      "totalAmount": 200,
      "isConfirmed": "unconfirmed"
    }
  ]
}


```

  

**13\. Update Booking (Only Accessible by Admin)**

*       *   **Route:** `/api/bookings/:id` (PUT)
    *   **Request Headers:**

```javascript
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! Do not copy and apply directly from the module. If you blindly follow the modules, you will be a copy master, not a developer.
```

*       *   **Request Body:**

```json
{
  "isConfirmed": "confirmed"
}
```

**Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Booking updated successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1ca",
    "date": "2024-06-15",
    "slots": ["60d9c4e4f3b4b544b8b8d1c6", "60d9c4e4f3b4b544b8b8d1c7"],
    "totalAmount": 200,
    "room": "60d9c4e4f3b4b544b8b8d1c5",
    "user": "60d9c4e4f3b4b544b8b8d1c4",
    "isConfirmed": "confirmed"
  }
}


```

  

**14\. Delete Booking (Soft Delete, Only Accessible by Admin)**

*       *   **Route:** `/api/bookings/:id` (DELETE)
    *   **Request Headers:**

```javascript
Authorization: 
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmF
tZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

You must include "Bearer" at the beginning of the token! Do not copy and apply directly from the module. If you blindly follow the modules, you will be a copy master, not a developer.
```

*       *   **Response:**

```json
{
  "success": true,
  "statusCode": 200,
  "message": "Booking deleted successfully",
  "data": {
    "_id": "60d9c4e4f3b4b544b8b8d1ca",
    "date": "2024-06-15",
    "slots": ["60d9c4e4f3b4b544b8b8d1c6", "60d9c4e4f3b4b544b8b8d1c7"],
    "totalAmount": 200,
    "room": "60d9c4e4f3b4b544b8b8d1c5",
    "user": "60d9c4e4f3b4b544b8b8d1c4",
    "isConfirmed": "confirmed",
    "isDeleted": true
  }
}


```

## Bonus Part: (10 Marks)

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

### **2\. Error Handling:**

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
            "message": "E11000 duplicate key error collection: univerity-management.students index: email_1 dup key: { email: \\"user2@gmail.com\\" }"
        }
    ],
    "stack": "MongoServerError: E11000 duplicate key error collection: univerity-management.students index: email_1 dup key: { email: \\"user2@gmail.com\\" }\\n    at H:\\\\next-level-development\\\\university-management-auth-service\\\\node_modules\\\\mongodb\\\\src\\\\operations\\\\insert.ts:85:25\\n    at H:\\\\next-level-development\\\\university-management-auth-service\\\\node_modules\\\\mongodb\\\\src\\\\cmap\\\\connection_pool.ts:574:11\\n    at H:\\\\next-level-development\\\\university-writeOrBuffer (node:internal/streams/writable:391:12)"
}
```

###   

### **3\. Not Found Route:**

Implement a global "Not Found" handler for unmatched routes. When a route is not found, respond with a generic message: "Not Found.”

```json
{
  "success": true,
  "statusCode": 404,
  "message": "Not Found",
}
```

  

### Additional Notes

*   The authentication token should be included in the `Authorization` header as `Bearer <token>`.
*   Only users with the `admin` role can create, update, and delete rooms and manage bookings.
*   The `isConfirmed` field for bookings indicates whether the booking is confirmed by an admin.
*   Soft delete for rooms and bookings is indicated by setting the `isDeleted` field to `true`.

This structure ensures the separation of concerns and role-based access control, providing a secure and efficient way to manage the meeting room booking system.
