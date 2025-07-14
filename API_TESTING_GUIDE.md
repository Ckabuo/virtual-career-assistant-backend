# API Testing Guide

This guide will help you test all the backend endpoints using Postman.

## 📋 Prerequisites

1. **Backend Server Running**: Ensure the Flask backend is running on `http://localhost:5000`
2. **MongoDB Running**: Ensure MongoDB is running locally
3. **Postman Installed**: Download and install Postman from [postman.com](https://www.postman.com/)

## 🚀 Quick Setup

### 1. Import Postman Collection

1. Open Postman
2. Click "Import" button
3. Select the `postman_collection.json` file from the `backend/` directory
4. The collection will be imported with all endpoints pre-configured

### 2. Configure Environment Variables

The collection uses these variables:
- `base_url`: `http://localhost:5000` (already set)
- `auth_token`: Will be automatically set after login

## 📝 Testing Flow

### Step 1: Authentication

#### 1.1 Register a New User
- **Endpoint**: `POST {{base_url}}/api/auth/register`
- **Body**:
```json
{
  "username": "testuser",
  "email": "test@example.com",
  "password": "password123"
}
```
- **Expected Response**: `201 Created`

#### 1.2 Login User
- **Endpoint**: `POST {{base_url}}/api/auth/login`
- **Body**:
```json
{
  "email": "test@example.com",
  "password": "password123"
}
```
- **Expected Response**: `200 OK`

### Step 2: Submit Responses

#### 2.1 Submit a Career Response
- **Endpoint**: `POST {{base_url}}/api/submit-response`
- **Headers**: `Authorization: Bearer {{auth_token}}`
- **Body**:
```json
{
  "prompt": "What are the best career paths for someone interested in technology?",
  "response": "Based on your interest in technology, here are some excellent career paths to consider:\n\n1. Software Development\n2. Data Science\n3. Cybersecurity\n4. Cloud Computing\n5. AI/ML Engineering\n\nEach of these fields offers strong growth potential and competitive salaries."
}
```

### Step 3: Retrieve Responses

#### 3.1 Get My Responses
- **Endpoint**: `GET {{base_url}}/api/my-responses`
- **Headers**: `Authorization: Bearer {{auth_token}}`

#### 3.2 Get All Responses (Admin Only)
- **Endpoint**: `GET {{base_url}}/api/all-responses`
- **Headers**: `Authorization: Bearer {{auth_token}}`

## 🔧 Manual Testing with curl

### Register User
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "email": "test@example.com",
    "password": "password123"
  }'
```

### Login User
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123"
  }'
```

### Submit Response (replace TOKEN with actual token)
```bash
curl -X POST http://localhost:5000/api/submit-response \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer TOKEN" \
  -d '{
    "prompt": "What are the best career paths for someone interested in technology?",
    "response": "Based on your interest in technology..."
  }'
```

## 🧪 Test Scenarios

### Scenario 1: Complete User Flow
1. Register a new user
2. Login with the user credentials
3. Submit a career response
4. Retrieve the user's responses
5. Verify the response appears in the list

### Scenario 2: Error Handling
1. Try to register with an existing email (should return 409)
2. Try to login with wrong credentials (should return 401)
3. Try to access protected endpoints without token (should return 401)
4. Try to submit response without required fields (should return 400)

## 🔍 Troubleshooting

### Common Issues

1. **Connection Refused**
   - Ensure the Flask backend is running
   - Check if the port 5000 is available

2. **MongoDB Connection Error**
   - Ensure MongoDB is running
   - Check the MONGO_URI in the .env file

3. **401 Unauthorized**
   - Check if the token is valid
   - Ensure the token is being sent in the Authorization header

4. **500 Internal Server Error**
   - Check the Flask server logs for detailed error messages
   - Verify all required environment variables are set 