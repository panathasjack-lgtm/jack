# Booking System

A simple booking system with no built-in pricing. Clients request bookings and receive quote requests.

## Features

- **Create Bookings**: Clients can request bookings with service details
- **Quote Requests**: Generate quote requests for bookings (contact required for pricing)
- **Status Tracking**: Track booking and quote request statuses
- **Service Management**: Support for different service types

## API Endpoints

### Bookings

#### Create Booking
```
POST /api/bookings
Content-Type: application/json

{
  "serviceType": "consultation",
  "clientName": "John Doe",
  "clientEmail": "john@example.com",
  "clientPhone": "+1-555-0123",
  "date": "2026-06-15T10:00:00Z",
  "duration": 60,
  "description": "Initial consultation"
}
```

#### Get All Bookings
```
GET /api/bookings
GET /api/bookings?status=pending
```

#### Get Specific Booking
```
GET /api/bookings/{id}
```

#### Update Booking Status
```
PATCH /api/bookings/{id}/status
Content-Type: application/json

{
  "status": "confirmed"
}
```

Allowed statuses: `pending`, `confirmed`, `completed`, `cancelled`

### Quote Requests

#### Create Quote Request
```
POST /api/bookings/{id}/quote-request
```

#### Get Quote Requests for Booking
```
GET /api/bookings/{id}/quotes
```

#### Get All Quote Requests
```
GET /api/quotes
```

#### Update Quote Request Status
```
PATCH /api/quotes/{id}/status
Content-Type: application/json

{
  "status": "sent"
}
```

Allowed statuses: `pending`, `sent`, `accepted`, `rejected`

## Booking Workflow

1. Client creates a booking request with service details
2. System generates a pending booking
3. Admin can request a quote for the booking
4. Quote is sent to client email
5. Client reviews and can accept/reject the quote
6. Once accepted, booking is confirmed and scheduled

## Data Models

### Booking
- `id`: Unique booking identifier
- `serviceType`: Type of service requested
- `clientName`: Client's full name
- `clientEmail`: Client's email address
- `clientPhone`: Client's phone number
- `date`: Scheduled date and time
- `duration`: Service duration in minutes
- `description`: Service description/details
- `status`: Current booking status
- `notes`: Additional notes
- `createdAt`: Creation timestamp
- `updatedAt`: Last update timestamp

### QuoteRequest
- `id`: Unique quote request identifier
- `bookingId`: Associated booking ID
- `clientEmail`: Client email for quote
- `status`: Quote request status
- `createdAt`: Creation timestamp
- `updatedAt`: Last update timestamp

## Integration

To integrate the booking system into your application:

```typescript
import bookingRouter from './booking/booking.routes';

app.use('/api', bookingRouter);
```

## Notes

- **No pricing in system**: Quotes are handled externally (via email or separate system)
- **Contact required**: Client contact information is mandatory
- **Status-based workflow**: Track booking progression through status updates
- **Quote workflow**: Separate tracking for quote requests and acceptance
