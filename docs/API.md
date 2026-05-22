# API Documentation

## Base URL

Development: `http://localhost:5000/api`
Production: `https://your-domain.com/api`

## Authentication

All protected endpoints require a JWT token in the Authorization header:

```
Authorization: Bearer <jwt_token>
```

### Login
- **Endpoint**: `POST /auth/login`
- **Body**: 
  ```json
  {
    "email": "student@example.com",
    "password": "password123"
  }
  ```
- **Response**:
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 86400
  }
  ```

### Register
- **Endpoint**: `POST /auth/register`
- **Body**:
  ```json
  {
    "email": "newstudent@example.com",
    "password": "SecurePassword123!",
    "first_name": "John",
    "last_name": "Doe",
    "phone": "+1-555-0123"
  }
  ```
- **Response**: `201 Created`
  ```json
  {
    "id": 1,
    "email": "newstudent@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "student",
    "created_at": "2024-01-15T10:30:00Z"
  }
  ```

### Logout
- **Endpoint**: `POST /auth/logout`
- **Authentication**: Required
- **Response**: `200 OK`

## Applications

### Create Application
- **Endpoint**: `POST /applications`
- **Authentication**: Required
- **Body**:
  ```json
  {
    "title": "Merit Scholarship Application 2024",
    "description": "My application for the annual merit scholarship",
    "gpa": 3.85,
    "school_name": "Lincoln High School",
    "grade_level": 12
  }
  ```
- **Response**: `201 Created`
  ```json
  {
    "id": 1,
    "user_id": 1,
    "title": "Merit Scholarship Application 2024",
    "status": "draft",
    "gpa": 3.85,
    "school_name": "Lincoln High School",
    "grade_level": 12,
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-01-15T10:30:00Z"
  }
  ```

### Get All Applications (Current User)
- **Endpoint**: `GET /applications`
- **Authentication**: Required
- **Query Parameters**: 
  - `status` - Filter by status (draft, submitted, under_review, approved, rejected)
  - `page` - Page number (default: 1)
  - `per_page` - Items per page (default: 20)
- **Response**: `200 OK`
  ```json
  {
    "data": [
      {
        "id": 1,
        "title": "Merit Scholarship Application 2024",
        "status": "draft",
        "created_at": "2024-01-15T10:30:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "per_page": 20,
      "total": 5
    }
  }
  ```

### Get Application Details
- **Endpoint**: `GET /applications/{id}`
- **Authentication**: Required
- **Response**: `200 OK`
  ```json
  {
    "id": 1,
    "user_id": 1,
    "title": "Merit Scholarship Application 2024",
    "description": "My application for the annual merit scholarship",
    "status": "draft",
    "gpa": 3.85,
    "school_name": "Lincoln High School",
    "grade_level": 12,
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-01-15T10:30:00Z",
    "documents": [
      {
        "id": 1,
        "filename": "transcript.pdf",
        "file_size": 102400,
        "uploaded_at": "2024-01-15T10:31:00Z"
      }
    ]
  }
  ```

### Update Application
- **Endpoint**: `PUT /applications/{id}`
- **Authentication**: Required
- **Body**: Same as create, any fields to update
- **Response**: `200 OK` - Updated application object

### Submit Application
- **Endpoint**: `POST /applications/{id}/submit`
- **Authentication**: Required
- **Response**: `200 OK`
  ```json
  {
    "id": 1,
    "status": "submitted",
    "submitted_at": "2024-01-15T14:30:00Z"
  }
  ```

### Delete Application
- **Endpoint**: `DELETE /applications/{id}`
- **Authentication**: Required
- **Restrictions**: Can only delete draft applications
- **Response**: `204 No Content`

## Documents

### Upload Document
- **Endpoint**: `POST /applications/{id}/documents`
- **Authentication**: Required
- **Content-Type**: `multipart/form-data`
- **Body**:
  ```
  file: <binary file>
  description: "Course transcript" (optional)
  ```
- **Allowed Types**: PDF, DOC, DOCX, JPG, PNG
- **Max Size**: 10 MB
- **Response**: `201 Created`
  ```json
  {
    "id": 1,
    "filename": "transcript_abc123.pdf",
    "file_type": "pdf",
    "file_size": 102400,
    "uploaded_at": "2024-01-15T10:31:00Z"
  }
  ```

### Download Document
- **Endpoint**: `GET /documents/{id}/download`
- **Authentication**: Required
- **Response**: File binary data with appropriate Content-Type

### Delete Document
- **Endpoint**: `DELETE /documents/{id}`
- **Authentication**: Required
- **Response**: `204 No Content`

## Reviews (Admin/Reviewer Only)

### Get Applications for Review
- **Endpoint**: `GET /admin/applications?status=under_review`
- **Authentication**: Required (reviewer/admin role)
- **Response**: `200 OK`

### Submit Review
- **Endpoint**: `POST /admin/applications/{id}/reviews`
- **Authentication**: Required (reviewer/admin role)
- **Body**:
  ```json
  {
    "score": 85,
    "comments": "Strong application with excellent GPA",
    "recommendation": "approve"
  }
  ```
- **Response**: `201 Created`
  ```json
  {
    "id": 1,
    "application_id": 1,
    "reviewer_id": 2,
    "score": 85,
    "recommendation": "approve",
    "created_at": "2024-01-15T15:30:00Z"
  }
  ```

## User Management

### Get Current User Profile
- **Endpoint**: `GET /users/me`
- **Authentication**: Required
- **Response**: `200 OK`
  ```json
  {
    "id": 1,
    "email": "student@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "student",
    "phone": "+1-555-0123",
    "date_of_birth": "2006-05-15",
    "is_verified": true,
    "created_at": "2024-01-10T10:00:00Z"
  }
  ```

### Update Profile
- **Endpoint**: `PUT /users/me`
- **Authentication**: Required
- **Body**: Any updatable fields (not email or password)
- **Response**: `200 OK` - Updated user object

### Change Password
- **Endpoint**: `POST /users/me/change-password`
- **Authentication**: Required
- **Body**:
  ```json
  {
    "current_password": "OldPassword123!",
    "new_password": "NewPassword456!"
  }
  ```
- **Response**: `200 OK`

## Error Responses

### 400 Bad Request
```json
{
  "error": "Invalid request",
  "details": "Email is required"
}
```

### 401 Unauthorized
```json
{
  "error": "Unauthorized",
  "details": "Invalid or expired token"
}
```

### 403 Forbidden
```json
{
  "error": "Forbidden",
  "details": "You do not have permission to access this resource"
}
```

### 404 Not Found
```json
{
  "error": "Not found",
  "details": "Application with ID 999 not found"
}
```

### 429 Too Many Requests
```json
{
  "error": "Rate limit exceeded",
  "details": "Too many requests. Please try again later."
}
```

### 500 Internal Server Error
```json
{
  "error": "Internal server error",
  "details": "An unexpected error occurred"
}
```

## Rate Limiting

- Login endpoint: 5 requests per 15 minutes per IP
- General API: 100 requests per hour per user
- File upload: 10 requests per hour per user

Rate limit info is provided in response headers:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1673798400
```

## Pagination

List endpoints support pagination:

```
GET /applications?page=2&per_page=20
```

Response includes pagination metadata:
```json
{
  "data": [...],
  "pagination": {
    "page": 2,
    "per_page": 20,
    "total": 150,
    "pages": 8
  }
}
```

## Response Format

All responses follow this format:

**Success**:
```json
{
  "data": {...} or [...],
  "meta": {
    "timestamp": "2024-01-15T10:30:00Z",
    "request_id": "req_abc123"
  }
}
```

**Error**:
```json
{
  "error": "error_code",
  "details": "Human readable error message",
  "meta": {
    "timestamp": "2024-01-15T10:30:00Z",
    "request_id": "req_abc123"
  }
}
```
