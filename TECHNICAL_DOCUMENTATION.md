# Technical Documentation

## Project Overview

**Project Name:** BG Removal (Background Removal Web Application)

**Description:** A full-stack web application that removes backgrounds from images using AI. Users can upload images and get them processed with background removal. The application includes a credit-based payment system for using the service.

---

## Architecture

### Tech Stack

| Layer | Technology |
|-------|-------------|
| Frontend | React, Vite, Tailwind CSS, React Router |
| Backend | Node.js, Express.js |
| Database | MongoDB (Mongoose ODM) |
| Authentication | Clerk (OAuth & JWT) |
| Payments | Razorpay, Stripe |
| Image Processing | Clipdrop API |

### Project Structure

```
bg-removal/
├── client/                    # React Frontend
│   ├── src/
│   │   ├── components/       # Reusable UI components
│   │   ├── pages/           # Page components
│   │   ├── context/         # React Context for state management
│   │   ├── assets/          # Images, icons, static data
│   │   ├── App.jsx          # Main app component
│   │   └── main.jsx         # Entry point
│   ├── public/              # Static public assets
│   └── package.json
│
├── server/                   # Express Backend
│   ├── controllers/         # Route handlers
│   ├── routes/              # API route definitions
│   ├── models/              # Mongoose schemas
│   ├── middlewares/         # Auth, upload middleware
│   ├── configs/             # Database config
│   ├── server.js            # Entry point
│   └── package.json
│
└── package.json             # Root package.json (optional)
```

---

## API Endpoints

### User Routes (`/api/user`)

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/webhooks` | No | Clerk webhook for user sync |
| GET | `/credits` | Yes | Get user credit balance |
| POST | `/pay-razor` | Yes | Create Razorpay order |
| POST | `/verify-razor` | No | Verify Razorpay payment |
| POST | `/pay-stripe` | Yes | Create Stripe checkout |
| POST | `/verify-stripe` | Yes | Verify Stripe payment |

### Image Routes (`/api/image`)

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/remove-bg` | Yes | Remove background from image |

---

## Database Models

### User Model (`userModel.js`)

```javascript
{
    clerkId: String,        // Clerk user ID (unique)
    email: String,          // User email (unique)
    photo: String,          // Profile photo URL
    firstName: String,
    lastName: String,
    creditBalance: Number   // Available credits (default: 5)
}
```

### Transaction Model (`transactionModel.js`)

```javascript
{
    clerkId: String,        // User ID
    plan: String,           // Plan name (Basic/Advanced/Business)
    amount: Number,         // Payment amount
    credits: Number,        // Credits purchased
    payment: Boolean,      // Payment status
    date: Number            // Timestamp
}
```

---

## Authentication Flow

1. **Clerk Integration**: Uses Clerk for OAuth authentication
2. **JWT Token**: Tokens contain custom session data with `clerkId`
3. **Webhook Sync**: Clerk webhooks sync user data to MongoDB
4. **Middleware**: `auth.js` middleware extracts clerkId from JWT

### Session Token Format (Clerk)
```json
{
  "clerkId": "{{user.id}}"
}
```

---

## Payment Flow

### Razorpay
1. User selects plan → Backend creates order
2. Frontend loads Razorpay checkout
3. Payment success → Webhook/verification updates credits

### Stripe
1. User selects plan → Backend creates checkout session
2. Redirect to Stripe checkout
3. Success → Redirect to `/verify` page → Update credits

### Credit Plans
| Plan | Price (₹) | Credits |
|------|-----------|---------|
| Basic | 10 | 100 |
| Advanced | 50 | 500 |
| Business | 250 | 5000 |

---

## Image Processing Flow

1. User uploads image via frontend
2. File sent to `/api/image/remove-bg` with JWT
3. Backend validates user credits
4. Image sent to Clipdrop API
5. Response converted to base64
6. User credits deducted
7. Result returned to frontend

---

## Frontend Pages

| Page | Route | Description |
|------|-------|-------------|
| Home | `/` | Landing page with upload |
| Result | `/result` | Shows processed image |
| BuyCredit | `/buy` | Credit purchase page |
| Verify | `/verify` | Payment verification |

---

## Frontend Components

- **Navbar**: Navigation with credit balance
- **Header**: Hero section
- **Steps**: How-it-works guide
- **BgSlider**: Background customization
- **Testimonials**: User reviews
- **Upload**: Image upload with drag-drop
- **Footer**: Links and copyright

---

## Environment Variables

### Client (`.env`)
```
VITE_BACKEND_URL=http://localhost:4000
VITE_RAZORPAY_KEY_ID=your_razorpay_key_id
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_key
```

### Server (`.env`)
```
PORT=4000
MONGO_URI=your_mongodb_connection_string
CLERK_WEBHOOK_SECRET=your_clerk_webhook_secret
CLIPDROP_API=your_clipdrop_api_key
STRIPE_SECRET_KEY=your_stripe_key
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret
CURRENCY=INR
JWT_SECRET=your_jwt_secret
```

---

## Key Features

1. **Background Removal**: AI-powered background removal via Clipdrop API
2. **Credit System**: Pay-as-you-go credit system
3. **Multiple Payment Gateways**: Razorpay and Stripe support
4. **Authentication**: Secure OAuth with Clerk
5. **Real-time Processing**: Instant image processing with loading states
6. **Credit Management**: Automatic credit deduction and balance tracking

---

## Data Flow Diagram

```
┌──────────┐    ┌───────────┐    ┌──────────┐    ┌─────────────┐
│  Client  │───▶│  Backend  │───▶│ Clipdrop │───▶│   Client    │
│  (React) │    │ (Express) │    │   API    │    │  (Display)  │
└──────────┘    └───────────┘    └──────────┘    └─────────────┘
      │              │                                    │
      │              ▼                                    │
      │         ┌─────────┐                               │
      │         │ MongoDB │                               │
      │         └─────────┘                               │
      │              │                                    │
      └──────────────┴──────────▶ Payment Gateway
                   (Razorpay/Stripe)
```

---

## Error Handling

- All API responses follow JSON format: `{ success: boolean, message?: string, data?: any }`
- Frontend uses `react-toastify` for error notifications
- Credit balance validation before image processing
- Payment verification before credit addition