# Quick Start Guide

## Your First API Call in 30 Minutes

This guide will help you make your first successful API call to Sabrhub in under 30 minutes.

## What You Need

1. Your Sabrhub account username and password
2. A tool to make API calls like curl, Postman, or your programming language
3. 30 minutes of your time

## Step 1: Choose Your API

Sabrhub has three main APIs:

### Webex Bot API
Use this if you want to:
- Connect Webex with phone numbers
- Send messages through Webex
- Manage customer organizations

### Teams Platform API
Use this if you want to:
- Connect Microsoft Teams with messaging
- Manage multiple customer accounts
- Route messages between systems

### ContextRegister API
Use this if you want to:
- Register brands for SMS compliance
- Manage 10DLC campaigns
- Handle brand verification

## Step 2: Login and Get Your Token

First, login to get your access token. Replace the username and password with your real ones:

```bash
curl -X POST "https://usermgmt.sabrhub.com/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "your-email@company.com",
    "password": "your-password"
  }'
```

You will get a response like this:

```json
{
  "username": "your-email@company.com",
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expirationTimeInUTC": "2024-02-29T15:09:14.242875589"
}
```

Copy the `accessToken` value. You will need it for the next step.

## Step 3: Make Your First API Call

Now use your access token to make an API call. Here are examples for each API:

### Example 1: Get Webex Enterprises

```bash
curl -X GET "https://context-webex.sabrhub.com/api/v1/csp/enterprises/CSP001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Example 2: Get Teams CSP Information

```bash
curl -X GET "https://teams-dev-frontend.sabrhub.com/api/v2/csp" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Example 3: Get Brand List from ContextRegister

```bash
curl -X GET "https://contextregister.sabrhub.com/api/v1/api/brands?page=1&size=10" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Step 4: Understand the Response

If your API call works, you will get data back in JSON format. If something goes wrong, you will get an error message.

### Success Response
```json
{
  "data": "your data here",
  "status": "success"
}
```

### Error Response
```json
{
  "error": "Invalid credentials",
  "status": "error"
}
```

## Common First-Time Mistakes

### Mistake 1: Wrong Authorization Header
Wrong way:
```
Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Right way:
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Mistake 2: Using Wrong Environment
For testing, always use development URLs first:
- Development: https://usermgmt-dev.sabrhub.com/v1
- Production: https://usermgmt.sabrhub.com/v1

### Mistake 3: Forgetting Content-Type
Always include this header for POST requests:
```
Content-Type: application/json
```

## Next Steps

After you make your first successful API call:

1. Read the detailed guides for your chosen API
2. Try creating or updating data through the API
3. Build error handling into your code
4. Test with real data in the development environment
5. Switch to production when ready

## Getting Help

If you get stuck:
1. Check your access token is not expired
2. Verify you are using the correct API URL
3. Make sure your JSON is properly formatted
4. Contact support@sabrhub.com with your error message

## Sample Complete Flow

Here is a complete example using the Webex API:

### Step 1: Login
```bash
curl -X POST "https://usermgmt-dev.sabrhub.com/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "test@email.com",
    "password": "yourpassword"
  }'
```

### Step 2: Create an Enterprise
```bash
curl -X POST "https://webex-dev-frontend.sabrhub.com/api/v1/enterprise/CSP001" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Test Company",
    "contact": "John Smith",
    "number": "+12223334444",
    "email": "john@testcompany.com"
  }'
```

### Step 3: Get the Enterprise You Just Created
```bash
curl -X GET "https://webex-dev-frontend.sabrhub.com/api/v1/enterprise/getenterprise/E0000090" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

Congratulations! You have successfully integrated with Sabrhub APIs.
