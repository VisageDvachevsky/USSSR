# API Documentation

## Base URL
```
Production: https://your-domain.com/api
Development: http://localhost:5000/api
```

## Authentication

Most endpoints require JWT authentication. Include the token in the Authorization header:
```
Authorization: Bearer <your-jwt-token>
```

## Endpoints

### Leaders

#### GET /leaders/
Get all published leaders

**Response:**
```json
{
  "success": true,
  "count": 7,
  "data": [...]
}
```

#### GET /leaders/:id
Get a specific leader by ID

**Response:**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "name_ru": "Владимир Ильич Ленин",
    "name_en": "Vladimir Ilyich Lenin",
    ...
  }
}
```

#### GET /leaders/:id/facts
Get AI-generated facts about a leader

**Query Parameters:**
- `count` (optional): Number of facts to return (default: 3, max: 10)

**Response:**
```json
{
  "success": true,
  "data": {
    "leader_id": 1,
    "leader_name": "Владимир Ильич Ленин",
    "facts": [...]
  }
}
```

#### GET /leaders/search
Search leaders using semantic search

**Query Parameters:**
- `q` (required): Search query

**Response:**
```json
{
  "success": true,
  "query": "революция",
  "count": 2,
  "data": [...]
}
```

#### GET /leaders/:id/recommendations
Get similar leaders

**Query Parameters:**
- `count` (optional): Number of recommendations (default: 3, max: 5)

**Response:**
```json
{
  "success": true,
  "data": [...]
}
```

#### POST /leaders/
Create a new leader (Admin only)

**Authentication Required:** Yes (Admin)

**Request Body:**
```json
{
  "name_ru": "Имя",
  "name_en": "Name",
  "slug": "name",
  "birth_year": 1900,
  ...
}
```

### Authentication

#### POST /auth/register
Register a new user

**Request Body:**
```json
{
  "username": "user123",
  "email": "user@example.com",
  "password": "securepassword",
  "full_name": "Full Name" // optional
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {...},
    "access_token": "...",
    "refresh_token": "..."
  }
}
```

#### POST /auth/login
Login user

**Request Body:**
```json
{
  "username": "user123",
  "password": "securepassword"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {...},
    "access_token": "...",
    "refresh_token": "..."
  }
}
```

#### GET /auth/me
Get current user information

**Authentication Required:** Yes

**Response:**
```json
{
  "success": true,
  "data": {
    "id": 1,
    "username": "user123",
    "email": "user@example.com",
    ...
  }
}
```

#### POST /auth/refresh
Refresh access token

**Authentication Required:** Yes (Refresh Token)

**Response:**
```json
{
  "success": true,
  "data": {
    "access_token": "..."
  }
}
```

### Analytics

#### GET /analytics/popular
Get most popular leaders by view count

**Query Parameters:**
- `limit` (optional): Number of leaders to return (default: 10, max: 50)

**Response:**
```json
{
  "success": true,
  "data": [...]
}
```

#### GET /analytics/recent-activity
Get recent activity logs (Admin only)

**Authentication Required:** Yes (Admin)

**Query Parameters:**
- `limit` (optional): Number of activities (default: 100, max: 500)

**Response:**
```json
{
  "success": true,
  "count": 100,
  "data": [...]
}
```

## Rate Limiting

Default rate limits:
- General API: 100 requests per hour
- Search: 30 requests per minute
- Auth endpoints: 5-10 requests per hour
- Admin endpoints: 10 requests per hour

## Error Responses

All error responses follow this format:
```json
{
  "success": false,
  "error": "Error type",
  "message": "Detailed error message"
}
```

Common HTTP Status Codes:
- 200: Success
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error
