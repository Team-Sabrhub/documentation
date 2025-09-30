# ContextSMS Teams Platform Integration Guide

## What is the ContextSMS Teams Platform API

The ContextSMS Teams Platform API connects Microsoft Teams with external messaging systems. It allows teams to send and receive text messages directly through Microsoft Teams, with full support for multiple customer organizations and CSP white-labeling.

## Who Uses This API

### Business Teams
Companies that want to handle customer text messages directly in Microsoft Teams.

### CSPs (Communication Service Providers)
Companies that provide messaging services with Teams integration to their business customers.

### System Integrators
Developers building unified communication solutions that combine Teams with SMS messaging.

## How It Works

1. A customer sends a text message to your business phone number
2. The ContextSMS platform receives the text message
3. The system routes the message to the correct Microsoft Teams channel
4. Team members see the message in Teams and can respond
5. Their Teams response is sent back as a text message to the customer
6. All conversation history is maintained in Teams

## Before You Start

You need these things:
1. A Sabrhub account with Teams Platform API access
2. Your API credentials (username and password)
3. Microsoft Teams set up for your organization
4. CSP account credentials if you are a service provider
5. Phone numbers that you want to connect to Teams

## Step 1: Authenticate

First, login to get your access token:

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "test@email.com",
    "password": "securePassword123"
  }'
```

### Response

You will receive your access token:

```json
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

Save the `accessToken` for use in all API calls.

## Step 2: Set Up Your CSP Configuration

If you are a CSP, you need to configure your service provider settings first.

### Create CSP Configuration

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/csp" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "cspName": "TechCorp Communications",
    "cspId": "TECHCORP001",
    "contactEmail": "admin@techcorp.com",
    "supportPhone": "+15551234567",
    "billingEnabled": true,
    "whitelabelSettings": {
      "brandName": "TechCorp Messaging",
      "logoUrl": "https://techcorp.com/logo.png",
      "primaryColor": "#0066CC",
      "supportUrl": "https://support.techcorp.com"
    }
  }'
```

### Get Your CSP Information

```bash
curl -X GET "https://teams-dev-frontend.sabrhub.com/api/v2/csp" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Step 3: Create an Enterprise

An enterprise represents one of your business customers in the system.

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/enterprise" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Sunshine Dental Clinic",
    "contactName": "Dr. Maria Rodriguez",
    "contactEmail": "maria@sunshinedental.com",
    "contactPhone": "+15559876543",
    "address": {
      "street": "123 Main Street",
      "city": "San Francisco",
      "state": "CA",
      "zipCode": "94105",
      "country": "US"
    },
    "teamsSettings": {
      "teamId": "teams-channel-id-12345",
      "channelId": "general-channel-id",
      "webhookUrl": "https://hooks.teams.microsoft.com/..."
    }
  }'
```

### What Each Field Means

- `name`: The business customer name
- `contactName`: Primary contact person
- `contactEmail`: Main email for the business
- `contactPhone`: Main phone number
- `address`: Physical business address
- `teamsSettings`: Microsoft Teams integration settings

### Response

You will get back an enterprise ID:

```json
{
  "enterpriseId": "ENT0000789",
  "name": "Sunshine Dental Clinic",
  "contactName": "Dr. Maria Rodriguez",
  "contactEmail": "maria@sunshinedental.com",
  "contactPhone": "+15559876543",
  "status": "active",
  "createdDate": "2024-01-15T10:30:00Z",
  "teamsSettings": {
    "teamId": "teams-channel-id-12345",
    "channelId": "general-channel-id",
    "webhookUrl": "https://hooks.teams.microsoft.com/...",
    "configured": true
  }
}
```

## Step 4: Create Phone Number Mappings

Connect phone numbers to your Teams channels so text messages are routed correctly.

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/mapping" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "phoneNumber": "+15559876543",
    "enterpriseId": "ENT0000789",
    "messagingAppId": "teams-channel-id-12345",
    "messagingAppType": "Teams",
    "routingRules": {
      "businessHours": {
        "start": "09:00",
        "end": "17:00",
        "timezone": "America/Los_Angeles",
        "days": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"]
      },
      "afterHoursMessage": "Thank you for contacting us. We will respond during business hours."
    }
  }'
```

### Response

```json
{
  "mappingId": "MAP0000456",
  "phoneNumber": "+15559876543",
  "enterpriseId": "ENT0000789",
  "messagingAppId": "teams-channel-id-12345",
  "messagingAppType": "Teams",
  "status": "active",
  "createdDate": "2024-01-15T10:35:00Z"
}
```

## Step 5: Test Your Integration

Test that messages flow correctly between SMS and Teams:

1. Send a text message to +15559876543 from your phone
2. The message should appear in the configured Teams channel
3. Reply from Teams
4. Your reply should be sent back as an SMS to the original sender

### Test Message Flow

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/messages/test" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "phoneNumber": "+15559876543",
    "message": "This is a test message from the API",
    "sender": "TestSystem"
  }'
```

## Managing Your Teams Integration

### List All Enterprises

```bash
curl -X GET "https://teams-dev-frontend.sabrhub.com/api/v2/enterprises" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Get Enterprise Details

```bash
curl -X GET "https://teams-dev-frontend.sabrhub.com/api/v2/enterprise/ENT0000789" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Update Enterprise Settings

```bash
curl -X PUT "https://teams-dev-frontend.sabrhub.com/api/v2/enterprise/ENT0000789" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Sunshine Dental Clinic - Downtown",
    "contactPhone": "+15559876544",
    "teamsSettings": {
      "teamId": "new-teams-channel-id-67890",
      "channelId": "updated-channel-id"
    }
  }'
```

### Get All Mappings

```bash
curl -X GET "https://teams-dev-frontend.sabrhub.com/api/v2/mappings" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Update a Mapping

```bash
curl -X PUT "https://teams-dev-frontend.sabrhub.com/api/v2/mapping/MAP0000456" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "routingRules": {
      "businessHours": {
        "start": "08:00",
        "end": "18:00",
        "timezone": "America/Los_Angeles"
      },
      "autoReply": "Thanks for your message! We will respond shortly."
    }
  }'
```

### Delete a Mapping

```bash
curl -X DELETE "https://teams-dev-frontend.sabrhub.com/api/v2/mapping/MAP0000456" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Real World Example: Dental Practice Chain

Let me show you how a dental practice chain would set up Teams integration:

### Company: SmileBright Dental Group
- They have 5 dental offices
- Each office has its own phone number and Teams channel
- They want all patient messages to go to the correct office Teams channel
- They have one main CSP managing all locations

### Step 1: Set Up CSP

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/csp" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "cspName": "SmileBright Communications",
    "cspId": "SMILEBRIGHT001",
    "contactEmail": "admin@smilebright.com",
    "whitelabelSettings": {
      "brandName": "SmileBright Patient Connect",
      "primaryColor": "#00A86B"
    }
  }'
```

### Step 2: Create Enterprise for First Office

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/enterprise" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "SmileBright Downtown",
    "contactName": "Dr. Sarah Johnson",
    "contactEmail": "sarah@smilebright.com",
    "contactPhone": "+15551001234",
    "teamsSettings": {
      "teamId": "downtown-team-id",
      "channelId": "downtown-general"
    }
  }'
```

### Step 3: Map Phone Number to Downtown Office

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/mapping" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "phoneNumber": "+15551001234",
    "enterpriseId": "ENT0000801",
    "messagingAppId": "downtown-team-id",
    "messagingAppType": "Teams"
  }'
```

### Step 4: Create Enterprise for Second Office

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/enterprise" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "SmileBright Westside",
    "contactName": "Dr. Michael Chen",
    "contactEmail": "michael@smilebright.com",
    "contactPhone": "+15551001235",
    "teamsSettings": {
      "teamId": "westside-team-id",
      "channelId": "westside-general"
    }
  }'
```

### Step 5: Map Phone Number to Westside Office

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/mapping" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "phoneNumber": "+15551001235",
    "enterpriseId": "ENT0000802",
    "messagingAppId": "westside-team-id",
    "messagingAppType": "Teams"
  }'
```

Now each office receives patient text messages directly in their own Teams channel.

## Advanced Features

### Webhook Configuration

Set up webhooks to receive real-time notifications:

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/webhooks" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://your-server.com/webhooks/teams",
    "events": ["message.received", "message.sent", "mapping.created"],
    "secret": "your-webhook-secret-key"
  }'
```

### Message Templates

Create automated response templates:

```bash
curl -X POST "https://teams-dev-frontend.sabrhub.com/api/v2/templates" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "appointment_confirmation",
    "content": "Hi {customerName}, your appointment is confirmed for {appointmentDate} at {appointmentTime}. Reply CONFIRM to verify.",
    "variables": ["customerName", "appointmentDate", "appointmentTime"]
  }'
```

## Error Handling

### Common Errors

#### Error 400: Invalid Teams Configuration
```json
{
  "error": "Teams webhook URL is invalid",
  "code": "INVALID_WEBHOOK_URL"
}
```
Fix: Verify your Teams webhook URL is correctly formatted and accessible.

#### Error 403: Insufficient Permissions
```json
{
  "error": "CSP does not have permission to create enterprises",
  "code": "PERMISSION_DENIED"
}
```
Fix: Contact support to verify your CSP account permissions.

#### Error 409: Phone Number Already Mapped
```json
{
  "error": "Phone number +15551234567 is already mapped to another enterprise",
  "code": "DUPLICATE_MAPPING"
}
```
Fix: Check existing mappings or delete the old mapping first.

## Development vs Production

### Development Environment
- API Base: https://teams-dev-frontend.sabrhub.com/api/v2
- Use for testing integrations and development
- Safe to experiment with test data

### Production Environment  
- API Base: https://context-sms.sabrhub.com/api/v2
- Use only for real customer data
- Higher rate limits and SLA guarantees

## Security Best Practices

1. Always use HTTPS for API calls
2. Store access tokens securely
3. Implement token refresh logic
4. Use webhook secrets to verify incoming data
5. Validate all input data before processing
6. Monitor API usage and set up alerts

## Monitoring and Analytics

### Get Message Statistics

```bash
curl -X GET "https://teams-dev-frontend.sabrhub.com/api/v2/analytics/messages?startDate=2024-01-01&endDate=2024-01-31" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Get Enterprise Usage

```bash
curl -X GET "https://teams-dev-frontend.sabrhub.com/api/v2/analytics/enterprise/ENT0000789" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Getting Help

If you need assistance:
1. Check the error message and code
2. Verify your API endpoint URLs
3. Test your integration in development first
4. Contact support@sabrhub.com with specific error details
5. Include your enterprise ID and mapping IDs when reporting issues
