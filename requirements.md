# 📋 Backend Feature Requirements

This document outlines the technical and functional specifications for key backend features of the Airbnb Clone project.

---

## 🔐 1. User Authentication

### Functional Requirements
- Allow users to register and log in securely
- Support JWT-based sessions
- Include OAuth options (Google, Facebook)

### API Endpoints
- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/auth/profile`

### Input/Output Specs
- **Register Input**: 
```json
{ 
    "first_name": "John", 
    "last_name": "Doe", 
    "gender": "Male",
    "user_type": "Host", 
    "email": "johndoe@example.com", 
    "password": "JohnDoe123?"
}
```
- **Login Input**: 
```json
{ 
    "email": "johndoe@example.com",  
    "password": "JohnDoe123?"
}
```
- **Output**: 
```json
{ 
    "status_code": 200,
    "access": "jkjkjadjbadkhkncdhja873wqei82wqy3ewybd", 
    "refresh": "hvhvahvvghj73t7t3h7hv872hbd83h2ete2vda2",
    "user": {
    "first_name": "John", 
    "last_name": "Doe", 
    "gender": "Male",
    "user_type": "Host", 
    "email": "johndoe@example.com" 
    }
}
```

### Validation Rules
- `email` must be valid format
- `password` must be at least 8 characters
- `email` must be unique

### Performance Criteria
- Response time < 300ms
- Token generation must be secure and scalable

---

## 🏠 2. Property Management

### Functional Requirements
- Hosts can add, edit, and delete listings
- Listings include title, description, price, location, images

### API Endpoints
- `POST /api/properties`
- `PUT /api/properties/:id`
- `DELETE /api/properties/:id`
- `GET /api/properties`

### Input/Output Specs
- **Create Property Input**: 
```json
{
  "title": "Cozy Apartment in Downtown",
  "description": "A modern, fully furnished apartment close to all amenities.",
  "price": 85.00,
  "location": "Lagos, Nigeria",
  "amenities": ["WiFi", "Air Conditioning", "Kitchen"],
  "images": [
    "https://example.com/image1.jpg",
    "https://example.com/image2.jpg"
  ]
}
```
- **Update Property Input**: 
```json
{
  "title": "Updated Apartment Title",
  "description": "Updated description with new features.",
  "price": 90.00,
  "location": "Victoria Island, Lagos",
  "amenities": ["WiFi", "Pool"],
  "images": [
    "https://example.com/newimage1.jpg"
  ]
}
```
- **Output (for both create and update)**: 
```json
{
  "status_code": 200,  
  "propertyId": "abc123xyz",
  "message": "Property created/updated successfully"
}
```
- **Delete Output (DELETE /api/properties/:id)**: 
```json
{
  "status_code": 200,
  "propertyId": "abc123xyz",
  "message": "Property deleted successfully"
}
```
- **Get Properties Output (GET /api/properties)**: 
```json
{
    "status_code": 200,
    "properties": [
        {
            "propertyId": "abc123xyz",
            "title": "Cozy Apartment in Downtown",
            "price": 85.00,
            "location": "Lagos, Nigeria",
            "images": [
            "https://example.com/image1.jpg"
            ]
        },
        {
            "propertyId": "def456uvw",
            "title": "Beachside Villa",
            "price": 150.00,
            "location": "Lekki, Lagos",
            "images": [
            "https://example.com/villa.jpg"
            ]
        }
    ]
}
```

### Validation Rules
- `price` must be a positive number
- `location` must be a valid string
- `images` must be valid URLs or file uploads
- `title` and `description` must not be empty

### Performance Criteria
- Image upload latency < 1s
- Listing retrieval supports pagination

---

## 📅 3. Booking System

### Functional Requirements
- Guests can book properties for specific dates
- Prevent double bookings
- Allow cancellations

### API Endpoints
- `POST /api/bookings`
- `GET /api/bookings/:userId`
- `DELETE /api/bookings/:id`

### Input/Output Specs
- **Create Booking Input (POST /api/bookings)**: 
```json
{
  "propertyId": "abc123xyz",
  "userId": "user789def",
  "startDate": "2025-09-10",
  "endDate": "2025-09-15"
}
```
- **Create Booking Output**: 
```json
{
  "status_code": 200,
  "bookingId": "book456ghi",
  "message": "Booking successfully created"
}
```
- **- Get Bookings Output (GET /api/bookings/:userId)**: 
```json
{
    "status_code": 200,
    "bookings": [
        {
            "bookingId": "book456ghi",
            "propertyId": "abc123xyz",
            "startDate": "2025-09-10",
            "endDate": "2025-09-15",
            "status": "confirmed"
        },
        {
            "bookingId": "book789jkl",
            "propertyId": "def456uvw",
            "startDate": "2025-10-01",
            "endDate": "2025-10-05",
            "status": "cancelled"
        }
    ]
}
```
- **- Delete Booking Output (DELETE /api/bookings/:id)**: 
```json
{
  "status_code": 200,
  "bookingId": "book456ghi",
  "message": "Booking successfully cancelled"
}
```

### Validation Rules
- `startDate` and `endDate` must be valid ISO 8601 dates
- Dates must not overlap with existing bookings for the same property
- `userId` must be authenticated and authorized
- `propertyId` must exist and be available

### Performance Criteria
- Booking confirmation < 500ms
- Conflict checks must scale with traffic

---

## 📌 Notes
All endpoints follow RESTful conventions and return appropriate HTTP status codes. Authentication is required for protected routes.
