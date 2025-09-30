# Authentication

## What is Authentication

Authentication is how you prove who you are to the Sabrhub API system. It is like showing your ID card to enter a building.

## How Sabrhub Authentication Works

All Sabrhub APIs use Bearer token authentication. A Bearer token is like a special password that proves you have permission to use the API.

## Step 1: Get Your API Token

Contact Sabrhub Support to get your API token:
1. Send an email to support@sabrhub.com
2. Tell them what you want to build with the API
3. They will send you a secure API token
4. Save this token safely

## Step 2: Login to Get Access Token

Before using any API, you need to login first.

### Login Request

Send a POST request to the login endpoint:

```
POST https://usermgmt.sabrhub.com/v1/auth/login
Content-Type: application/json

{
  "username": "test@email.com",
  "password": "yourpassword"
}
```

### Login Response

When login is successful, you get back:

```
{
  "username": "test@email.com",
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expirationTimeInUTC": "2024-02-29T15:09:14.242875589",
  "roles": ["CSP_BUSINESS"],
  "tenantId": "Sabrhub",
  "services": ["Teams"]
}
```

The most important part is the `accessToken`. This is what you will use in all your API calls.

## Step 3: Use Your Access Token

For every API call you make, include your access token in the Authorization header:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Example API Call with Token

```
GET https://context-webex.sabrhub.com/api/v1/enterprise/getenterprise/E0000090
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## Step 4: Refresh Your Token

Access tokens expire after some time. When your token expires, use the refresh token to get a new one.

### Refresh Token Request

```
POST https://usermgmt.sabrhub.com/v1/auth/getaccesstoken
Content-Type: application/json

{
  "username": "test@email.com",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

## Important Security Rules

1. Never share your API token with anyone
2. Never put your token in websites or client-side code
3. Store your token in a secure place like environment variables
4. If your token is stolen, contact support immediately

## Environment URLs

### Development Environment
Use these URLs when testing your integration:
- Authentication: https://usermgmt-dev.sabrhub.com/v1
- Webex API: https://webex-dev-frontend.sabrhub.com/api/v1
- Teams API: https://teams-dev-frontend.sabrhub.com/api/v2
- ContextRegister API: https://cr-dev.sabrhub.com/api/v1

### Production Environment
Use these URLs when your integration is ready for real customers:
- Authentication: https://usermgmt.sabrhub.com/v1
- Webex API: https://context-webex.sabrhub.com/api/v1
- Teams API: https://context-sms.sabrhub.com/api/v2
- ContextRegister API: https://contextregister.sabrhub.com/api/v1
