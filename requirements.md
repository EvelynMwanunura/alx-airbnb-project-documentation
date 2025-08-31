# Airbnb Clone Backend - Requirements Specification

This document defines the detailed requirements for three core backend features of the Airbnb Clone: **User Authentication**, **Property Management**, and **Booking System**.

---

## **1. User Authentication**

### **Overview**

Handles secure user registration, login, and profile management for both guests and hosts.

### **API Endpoints**

| Method   | Endpoint             | Description                          |
| -------- | -------------------- | ------------------------------------ |
| **POST** | `/api/auth/register` | Register a new user (guest or host). |
| **POST** | `/api/auth/login`    | Authenticate user and return token.  |
| **GET**  | `/api/auth/profile`  | Retrieve authenticated user profile. |
| **PUT**  | `/api/auth/profile`  | Update user profile details.         |

### **Input Specifications**

**Registration Request**

```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "Password123",
  "role": "host"
}
```

**Login Request**

```json
{
  "email": "jane@example.com",
  "password": "Password123"
}
```

### **Output Specifications**

**Successful Login/Registration**

```json
{
  "status": "success",
  "token": "jwt-token",
  "user": {
    "id": "U1001",
    "name": "Jane Doe",
    "email": "jane@example.com",
    "role": "host"
  }
}
```

**Error**

```json
{
  "status": "error",
  "message": "Invalid credentials"
}
```

### **Validation Rules**

- Email must be in valid format and unique.
- Password must:
  - Be at least **8 characters**.
  - Include uppercase, lowercase, and a number.
- Role must be either `guest` or `host`.

### **Performance Criteria**

- Authentication requests processed in **<500ms**.
- Token expiration: **24 hours**.
- System supports at least **1000 concurrent sessions**.

---

## **2. Property Management**

### **Overview**

Allows hosts to create, update, and delete property listings, and lets guests view and search properties.

### **API Endpoints**

| Method     | Endpoint              | Description                                       |
| ---------- | --------------------- | ------------------------------------------------- |
| **POST**   | `/api/properties`     | Create a new property listing.                    |
| **GET**    | `/api/properties`     | Fetch all properties with pagination and filters. |
| **GET**    | `/api/properties/:id` | Fetch details of a specific property.             |
| **PUT**    | `/api/properties/:id` | Update details of a specific property.            |
| **DELETE** | `/api/properties/:id` | Remove a property listing.                        |

### **Input Specifications**

**Create Property Request**

```json
{
  "title": "Beachfront Villa",
  "description": "Luxury villa with pool and ocean view",
  "location": "Cape Town",
  "price": 250,
  "amenities": ["Wi-Fi", "Pool", "Air Conditioning"],
  "availability": {
    "startDate": "2025-09-01",
    "endDate": "2025-12-31"
  }
}
```

### **Output Specifications**

**Success**

```json
{
  "status": "success",
  "propertyId": "P2025",
  "message": "Property created successfully."
}
```

**Error**

```json
{
  "status": "error",
  "message": "Missing required fields."
}
```

### **Validation Rules**

- Title, description, and location cannot be empty.
- Price must be a positive number.
- Availability dates must follow `YYYY-MM-DD` format and `startDate` must be before `endDate`.

### **Performance Criteria**

- Property search results must return in **<800ms** with pagination enabled.
- System supports at least **50,000 property records** efficiently.

---

## **3. Booking System**

### **Overview**

Handles property booking creation, cancellation, and retrieval, ensuring accurate availability tracking.

### **API Endpoints**

| Method   | Endpoint                   | Description                           |
| -------- | -------------------------- | ------------------------------------- |
| **POST** | `/api/bookings`            | Create a booking.                     |
| **GET**  | `/api/bookings`            | Get all bookings for a specific user. |
| **PUT**  | `/api/bookings/:id/cancel` | Cancel an existing booking.           |

### **Input Specifications**

**Create Booking Request**

```json
{
  "propertyId": "P2025",
  "guestId": "U1002",
  "startDate": "2025-09-15",
  "endDate": "2025-09-20"
}
```

### **Output Specifications**

**Success**

```json
{
  "status": "success",
  "bookingId": "B3030",
  "message": "Booking confirmed."
}
```

**Error**

```json
{
  "status": "error",
  "message": "Property is not available for selected dates."
}
```

### **Validation Rules**

- Start date must be before end date.
- Booking dates must not overlap with existing confirmed bookings.
- User must be authenticated.

### **Performance Criteria**

- Booking confirmation processed within **1 second**.
- System supports at least **100 bookings per second** under peak load.
