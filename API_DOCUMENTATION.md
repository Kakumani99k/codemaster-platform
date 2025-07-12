---
noteId: "018705e05b7611f0845a57f33a2b25cc"
tags: []

---

# LeetCode Clone API Documentation

## Base URL
```
http://localhost:3000/api/v1
```

## Authentication
Most endpoints require authentication via JWT token. Include the token in the Authorization header:
```
Authorization: Bearer YOUR_JWT_TOKEN
```

## Endpoints

### Authentication

#### Register
- **POST** `/auth/register`
- **Body:**
  ```json
  {
    "email": "user@example.com",
    "password": "password123",
    "displayName": "John Doe" // optional
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "user": { ...userObject },
      "token": "jwt_token"
    }
  }
  ```

#### Login
- **POST** `/auth/login`
- **Body:**
  ```json
  {
    "email": "user@example.com",
    "password": "password123"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "user": { ...userObject },
      "token": "jwt_token"
    }
  }
  ```

#### Get Profile
- **GET** `/auth/profile`
- **Headers:** Authorization required
- **Response:**
  ```json
  {
    "success": true,
    "data": { ...userObject }
  }
  ```

#### Update Profile
- **PUT** `/auth/profile`
- **Headers:** Authorization required
- **Body:**
  ```json
  {
    "displayName": "New Name",
    "bio": "About me",
    "website": "https://example.com",
    "location": "New York"
  }
  ```

### Problems

#### List Problems
- **GET** `/problems`
- **Query Parameters:**
  - `page` (default: 1)
  - `limit` (default: 20, max: 100)
  - `difficulty` (easy/medium/hard)
  - `tag` (string)
  - `search` (string)
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "problems": [...],
      "pagination": {
        "page": 1,
        "limit": 20,
        "total": 100,
        "totalPages": 5
      }
    }
  }
  ```

#### Get Problem
- **GET** `/problems/:id`
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "id": "problem_id",
      "title": "Two Sum",
      "description": "...",
      "difficulty": "easy",
      "tags": ["array", "hash-table"],
      "testCases": [...],
      "starterCode": {...},
      "constraints": [...],
      "examples": [...]
    }
  }
  ```

#### Create Problem
- **POST** `/problems`
- **Headers:** Authorization required
- **Body:**
  ```json
  {
    "title": "Problem Title",
    "description": "Problem description",
    "difficulty": "easy|medium|hard",
    "tags": ["array", "math"],
    "testCases": [
      { "input": "2 3", "output": "5" }
    ],
    "hiddenTestCases": [
      { "input": "10 20", "output": "30" }
    ],
    "starterCode": {
      "javascript": "function solve() {}",
      "python": "def solve():"
    },
    "constraints": ["1 <= n <= 100"],
    "examples": [
      {
        "input": "nums = [2,7,11,15], target = 9",
        "output": "[0,1]",
        "explanation": "..."
      }
    ]
  }
  ```

#### Update Problem
- **PUT** `/problems/:id`
- **Headers:** Authorization required
- **Body:** Same as create (all fields optional)

#### Delete Problem
- **DELETE** `/problems/:id`
- **Headers:** Authorization required

#### Get User Solved Problems
- **GET** `/problems/user/:userId/solved`
- **Headers:** Authorization required

### Submissions

#### Run Code
- **POST** `/submissions/run`
- **Headers:** Authorization required
- **Body:**
  ```json
  {
    "problemId": "problem_id",
    "code": "// your solution code",
    "language": "javascript|python|java|cpp|c"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "results": [
        {
          "testCase": 1,
          "passed": true,
          "expected": "5",
          "actual": "5",
          "executionTime": 100
        }
      ],
      "allPassed": true
    }
  }
  ```

#### Submit Solution
- **POST** `/submissions`
- **Headers:** Authorization required
- **Body:**
  ```json
  {
    "problemId": "problem_id",
    "code": "// your solution code",
    "language": "javascript|python|java|cpp|c"
  }
  ```
- **Response:**
  ```json
  {
    "success": true,
    "data": {
      "id": "submission_id",
      "status": "pending",
      "createdAt": "2024-01-01T00:00:00Z"
    }
  }
  ```

#### Get Submission
- **GET** `/submissions/:id`
- **Headers:** Authorization required

#### List User Submissions
- **GET** `/submissions`
- **Headers:** Authorization required
- **Query Parameters:**
  - `page` (default: 1)
  - `limit` (default: 20)
  - `problemId` (optional)
  - `status` (pending|running|accepted|wrong_answer|time_limit_exceeded|runtime_error|compile_error)

#### List Problem Submissions
- **GET** `/submissions/problem/:problemId`
- **Headers:** Optional authorization

## Error Responses

All errors follow this format:
```json
{
  "error": "Error message",
  "details": "Additional details (optional)"
}
```

Common HTTP status codes:
- 200: Success
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 409: Conflict
- 429: Too Many Requests
- 500: Internal Server Error

## Rate Limiting

- Authentication endpoints: 5 requests per 15 minutes
- Submission endpoints: 10 requests per minute
- Other endpoints: 100 requests per 15 minutes

## Supported Languages

- JavaScript (Node.js 18)
- Python (3.11)
- Java (OpenJDK 17)
- C++ (GCC latest)
- C (GCC latest)