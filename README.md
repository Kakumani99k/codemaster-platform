---
noteId: "01872cf05b7611f0845a57f33a2b25cc"
tags: []

---

# LeetCode Clone Backend

A fully functional backend for a LeetCode-style coding platform with user authentication, problem management, and code execution capabilities.

## Features

- User authentication (register, login, profile management)
- Problem CRUD operations with difficulty levels and tags
- Real-time code execution with Docker containers
- Submission tracking and testing
- Rate limiting and security measures
- Comprehensive test coverage

## Prerequisites

- Node.js 18+
- Docker installed and running
- Firebase project with Admin SDK credentials

## Installation

1. Install dependencies:
```bash
npm install
```

2. Create `.env` file based on `.env.example`:
```bash
cp .env.example .env
```

3. Update `.env` with your configuration:
```
PORT=3000
NODE_ENV=development
JWT_SECRET=your-secure-secret-key
JWT_EXPIRE=7d
RATE_LIMIT_MAX=100
RATE_LIMIT_WINDOW_MS=900000
DOCKER_TIMEOUT=10000
MAX_CONCURRENT_EXECUTIONS=5
```

4. Ensure Firebase credentials are in place at `../leetcode_credentials.json`

## Running the Server

Development mode:
```bash
npm run dev
```

Production mode:
```bash
npm start
```

## Testing

Run all tests:
```bash
npm test
```

Run tests with coverage:
```bash
npm test -- --coverage
```

Watch mode:
```bash
npm run test:watch
```

## API Documentation

See [API_DOCUMENTATION.md](./API_DOCUMENTATION.md) for detailed endpoint documentation.

## Docker Setup

The code execution service requires Docker. Pull the required images:

```bash
docker pull node:18-alpine
docker pull python:3.11-alpine
docker pull openjdk:17-alpine
docker pull gcc:latest
```

## Project Structure

```
backend/
├── src/
│   ├── config/         # Configuration files
│   ├── controllers/    # Request handlers
│   ├── middleware/     # Express middleware
│   ├── models/         # Data models
│   ├── routes/         # API routes
│   ├── services/       # Business logic
│   ├── utils/          # Utility functions
│   └── server.js       # Main server file
├── tests/              # Test files
├── temp/               # Temporary files for code execution
└── package.json
```

## Security Features

- JWT-based authentication
- Rate limiting on all endpoints
- Input validation and sanitization
- Secure Docker container execution
- Request logging and monitoring

## Database Schema

### Users Collection
- uid (string): Unique user ID
- email (string): User email
- displayName (string): Display name
- role (string): User role (user/admin)
- solvedProblems (array): List of solved problem IDs
- stats (object): User statistics

### Problems Collection
- id (string): Unique problem ID
- title (string): Problem title
- description (string): Problem description
- difficulty (string): easy/medium/hard
- tags (array): Problem tags
- testCases (array): Visible test cases
- hiddenTestCases (array): Hidden test cases
- starterCode (object): Language-specific starter code
- submissions (number): Total submissions
- acceptedSubmissions (number): Accepted submissions
- acceptanceRate (number): Acceptance percentage

### Submissions Collection
- id (string): Unique submission ID
- userId (string): User ID
- problemId (string): Problem ID
- code (string): Submitted code
- language (string): Programming language
- status (string): Submission status
- results (array): Test case results
- runtime (number): Execution time
- createdAt (timestamp): Submission time

## License

ISC