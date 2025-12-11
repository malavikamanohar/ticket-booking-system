# Ticket Booking System - Full Stack Application

A complete full-stack ticket booking system built with Node.js, Express.js, PostgreSQL (backend) and React, TypeScript (frontend). Handles high concurrency scenarios to prevent race conditions and overbooking.

## Project Structure

```
.
├── backend/          # Node.js + Express + PostgreSQL API
│   ├── src/
│   │   ├── db/       # Database configuration and migrations
│   │   ├── models/   # Data models
│   │   ├── controllers/ # Request handlers
│   │   ├── routes/   # API routes
│   │   └── middleware/ # Middleware functions
│   └── package.json
│
├── frontend/         # React + TypeScript application
│   ├── src/
│   │   ├── components/ # React components
│   │   ├── context/    # Context API providers
│   │   ├── pages/      # Page components
│   │   ├── services/   # API service functions
│   │   └── types/       # TypeScript types
│   └── package.json
│
└── README.md
```

## Features

### Backend Features
- ✅ Show/Trip Management (Admin can create shows)
- ✅ User Operations (View shows, book seats)
- ✅ Concurrency Handling (Prevents overbooking using database transactions)
- ✅ Booking Status (PENDING, CONFIRMED, FAILED)
- ✅ Automatic Expiry (PENDING bookings expire after 2 minutes)

### Frontend Features
- ✅ Admin Dashboard (Create and manage shows)
- ✅ User Interface (Browse shows, book seats)
- ✅ Visual Seat Selection (Grid layout with real-time updates)
- ✅ State Management (React Context API)
- ✅ Error Handling (Comprehensive validation and error messages)
- ✅ Responsive Design (Mobile and desktop)

## Quick Start

### Prerequisites

- Node.js (v16 or higher)
- PostgreSQL (v12 or higher)
- npm or yarn

### Backend Setup

1. Navigate to backend directory:
```bash
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables:
```bash
# Create .env file
cp .env.example .env
# Edit .env with your database credentials
```

4. Create PostgreSQL database:
```bash
createdb ticket_booking
```

5. Run migrations:
```bash
npm run migrate
```

6. Start the server:
```bash
npm run dev
```

Backend will run on `http://localhost:3001`

### Frontend Setup

1. Navigate to frontend directory:
```bash
cd frontend
```

2. Install dependencies:
```bash
npm install
```

3. (Optional) Set environment variables:
```bash
# Create .env file
echo "VITE_API_URL=http://localhost:3001/api" > .env
```

4. Start development server:
```bash
npm run dev
```

Frontend will run on `http://localhost:3000`

## API Documentation

### Shows Endpoints

- `POST /api/shows` - Create a new show
- `GET /api/shows` - Get all shows
- `GET /api/shows/:id` - Get show by ID
- `GET /api/shows/:id/seats` - Get available seats for a show

### Bookings Endpoints

- `POST /api/bookings` - Create a new booking
- `PUT /api/bookings/:id/confirm` - Confirm a booking
- `GET /api/bookings/:id` - Get booking by ID
- `GET /api/bookings/user/:userId` - Get user's bookings

## Concurrency Handling

The system uses PostgreSQL transactions with `SELECT FOR UPDATE` to lock rows during booking operations. This ensures:

1. **Atomicity**: Booking operations are atomic
2. **Consistency**: No overbooking can occur
3. **Isolation**: Concurrent requests are properly handled

When multiple users try to book the same seats simultaneously, only one booking succeeds.

## Booking Expiry

PENDING bookings automatically expire after 2 minutes. A background job runs every 30 seconds to check and mark expired bookings as FAILED.

## Database Schema

### Shows Table
- `id` (SERIAL PRIMARY KEY)
- `name` (VARCHAR)
- `start_time` (TIMESTAMP)
- `total_seats` (INTEGER)
- `created_at` (TIMESTAMP)
- `updated_at` (TIMESTAMP)

### Bookings Table
- `id` (SERIAL PRIMARY KEY)
- `show_id` (INTEGER, FOREIGN KEY)
- `user_id` (VARCHAR)
- `seat_numbers` (INTEGER[])
- `status` (VARCHAR) - PENDING, CONFIRMED, or FAILED
- `created_at` (TIMESTAMP)
- `expires_at` (TIMESTAMP)

## Testing

### Backend Testing
```bash
# Create a show
curl -X POST http://localhost:3001/api/shows \
  -H "Content-Type: application/json" \
  -d '{"name":"Test Show","startTime":"2024-12-25T18:00:00Z","totalSeats":40}'

# Get all shows
curl http://localhost:3001/api/shows

# Create a booking
curl -X POST http://localhost:3001/api/bookings \
  -H "Content-Type: application/json" \
  -d '{"showId":1,"userId":"user1","seatNumbers":[1,2]}'
```

### Frontend Testing
1. Start both backend and frontend servers
2. Navigate to `http://localhost:3000`
3. Use Admin dashboard to create shows
4. Book seats from the user interface

## Deployment

### Backend Deployment
The backend can be deployed to:
- Render
- Railway
- Heroku
- AWS
- Any Node.js hosting service

Make sure to:
1. Set environment variables on the hosting platform
2. Configure PostgreSQL database
3. Run migrations after deployment

### Frontend Deployment
The frontend can be deployed to:
- Vercel
- Netlify
- Any static hosting service

Make sure to:
1. Set `VITE_API_URL` environment variable to your deployed backend URL
2. Build the project: `npm run build`
3. Deploy the `dist` folder

## System Design Document

See `SYSTEM_DESIGN.md` for detailed architecture and scalability considerations.

## License

ISC



