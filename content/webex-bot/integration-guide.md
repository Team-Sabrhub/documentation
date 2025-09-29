# ContextSMS Webex Bot Integration Guide

## What is the ContextSMS Webex Bot API

The ContextSMS Webex Bot API lets you connect phone numbers with Webex messaging. When someone sends a text message to a phone number, it can show up in Webex. When someone sends a message in Webex, it can be sent as a text message.

## Who Uses This API

### Customers
Companies that want their Webex teams to receive and send text messages from customers.

### CSPs (Communication Service Providers)
Companies that provide messaging services to their business customers.

## How It Works

1. A customer sends a text message to a phone number
2. The ContextSMS system receives the text message
3. The system finds the right Webex bot to send the message to
4. The message appears in the Webex team chat
5. Team members can reply through Webex
6. Their reply is sent back as a text message to the customer

## Before You Start

You need these things:
1. A Sabrhub account with Webex API access
2. Your API credentials (username and password)
3. Access to a Webex bot or the ability to create one
4. Phone numbers that you want to connect to Webex

## Step 1: Authenticate

First, login to get your access token:

```bash
curl -X POST "https://usermgmt.sabrhub.com/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "your-email@company.com",
    "password": "your-password"
  }'
```

Save the `accessToken` from the response. You will use it in all future API calls.

## Step 2: Create an Enterprise

An enterprise represents a customer company in the system. Each customer company gets their own enterprise.

```bash
curl -X POST "https://context-webex.sabrhub.com/api/v1/enterprise/CSP001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "ABC Marketing Company",
    "contact": "Sarah Johnson",
    "number": "+15551234567",
    "email": "sarah@abcmarketing.com",
    "defaultMmsSelection": true
  }'
```

### What Each Field Means

- `name`: The customer company name
- `contact`: The main contact person at the company
- `number`: The company phone number
- `email`: The main email address for the company
- `defaultMmsSelection`: Whether to enable MMS (picture messages) by default

### Response

You will get back an enterprise ID:

```json
{
  "enterpriseId": "E0000123",
  "name": "ABC Marketing Company",
  "contact": "Sarah Johnson",
  "number": "+15551234567",
  "email": "sarah@abcmarketing.com",
  "deleted": false,
  "createdDate": 1698446777442,
  "updateDate": 1698446777443,
  "webexSettings": null,
  "numberToMessageAppMaps": null,
  "defaultMmsSelection": true
}
```

Save the `enterpriseId`. You will need it for the next step.

## Step 3: Create Phone Number Mappings

Now you need to connect phone numbers to Webex bots. This tells the system which Webex bot should receive messages for each phone number.

```bash
curl -X POST "https://context-webex.sabrhub.com/api/v1/mapping/add/E0000123" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "messagingAppId": "webex-bot-12345",
    "phoneNumber": "+15551234567",
    "messagingAppType": "Webex"
  }'
```

### What Each Field Means

- `messagingAppId`: The ID of your Webex bot
- `phoneNumber`: The phone number that will receive text messages
- `messagingAppType`: Always set this to "Webex" for Webex bots

### Response

You will get back a mapping ID:

```json
{
  "numberToMessageAppMapId": "NTMAP0000456",
  "messagingAppId": "webex-bot-12345",
  "phoneNumber": "+15551234567",
  "messagingAppType": "Webex"
}
```

## Step 4: Test Your Integration

Now test that everything is working by sending a text message to the phone number you configured.

1. Send a text message to +15551234567 from your personal phone
2. The message should appear in your Webex bot chat
3. Reply through Webex
4. Your reply should be sent back as a text message

## Managing Your Integration

### Get Enterprise Information

To see details about an enterprise:

```bash
curl -X GET "https://context-webex.sabrhub.com/api/v1/enterprise/getenterprise/E0000123" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### List All Enterprises for Your CSP

To see all enterprises under your CSP account:

```bash
curl -X GET "https://context-webex.sabrhub.com/api/v1/csp/enterprises/CSP001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Get All Mappings for an Enterprise

To see all phone number mappings for an enterprise:

```bash
curl -X GET "https://context-webex.sabrhub.com/api/v1/mappings/enterprise/E0000123" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Delete a Phone Number Mapping

To remove a phone number mapping:

```bash
curl -X DELETE "https://context-webex.sabrhub.com/api/v1/mapping/NTMAP0000456" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Delete an Enterprise

To remove an enterprise completely:

```bash
curl -X DELETE "https://context-webex.sabrhub.com/api/v1/enterprise/deleteenterprise/E0000123" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Real World Example

Let me show you how a marketing company would set up the integration:

### Company: BrightStar Marketing
- They have 3 phone numbers for different campaigns
- They want all text messages to go to their main Webex team
- They have one Webex bot for the whole company

### Step 1: Create the Enterprise

```bash
curl -X POST "https://context-webex.sabrhub.com/api/v1/enterprise/CSP001" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "BrightStar Marketing",
    "contact": "Mike Chen",
    "number": "+15559871234",
    "email": "mike@brightstarmarketing.com",
    "defaultMmsSelection": true
  }'
```

### Step 2: Map First Phone Number (Main Line)

```bash
curl -X POST "https://context-webex.sabrhub.com/api/v1/mapping/add/E0000124" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "messagingAppId": "brightstar-webex-bot",
    "phoneNumber": "+15559871234",
    "messagingAppType": "Webex"
  }'
```

### Step 3: Map Second Phone Number (Campaign A)

```bash
curl -X POST "https://context-webex.sabrhub.com/api/v1/mapping/add/E0000124" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "messagingAppId": "brightstar-webex-bot",
    "phoneNumber": "+15559871235",
    "messagingAppType": "Webex"
  }'
```

### Step 4: Map Third Phone Number (Campaign B)

```bash
curl -X POST "https://context-webex.sabrhub.com/api/v1/mapping/add/E0000124" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "messagingAppId": "brightstar-webex-bot",
    "phoneNumber": "+15559871236",
    "messagingAppType": "Webex"
  }'
```

Now BrightStar Marketing can receive text messages from all three phone numbers in their single Webex team chat.

## Error Handling

### Common Errors

#### Error 400: Bad Request
This happens when your request data is wrong.

Example error:
```json
{
  "error": "Invalid phone number format"
}
```

Fix: Check that your phone number includes the country code like +15551234567

#### Error 401: Unauthorized
This happens when your access token is wrong or expired.

Example error:
```json
{
  "error": "Invalid or expired token"
}
```

Fix: Login again to get a new access token

#### Error 404: Not Found
This happens when you try to access something that does not exist.

Example error:
```json
{
  "error": "Enterprise not found"
}
```

Fix: Check that you are using the correct enterprise ID

## Development vs Production

### Development Environment
Use these URLs for testing:
- Authentication: https://usermgmt-dev.sabrhub.com/v1
- API Base: https://webex-dev-frontend.sabrhub.com/api/v1

### Production Environment
Use these URLs for real customers:
- Authentication: https://usermgmt.sabrhub.com/v1
- API Base: https://context-webex.sabrhub.com/api/v1

Always test your integration in development first before using production.

## Next Steps

After you have a working integration:

1. Set up webhook handling to receive real-time message notifications
2. Implement error handling and retry logic in your code
3. Monitor your integration for any issues
4. Scale to handle multiple customers and enterprises

## Getting Help

If you need help:
1. Check this documentation first
2. Test your API calls with curl or Postman
3. Contact support@sabrhub.com with your specific error messages
4. Include your enterprise ID and mapping IDs when asking for help
