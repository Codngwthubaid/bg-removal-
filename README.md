# BG Removal

A full-stack AI-powered web application that removes backgrounds from images. Upload any image and get instant background removal with support for custom background colors.

## Features

- **AI Background Removal** - Powered by Clipdrop API
- **Credit System** - Pay-as-you-go credit-based pricing
- **Multiple Payment Gateways** - Razorpay and Stripe support
- **Secure Authentication** - Clerk OAuth
- **Custom Backgrounds** - Choose background colors

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React, Vite, Tailwind CSS |
| Backend | Node.js, Express |
| Database | MongoDB |
| Authentication | Clerk |
| Payments | Razorpay, Stripe |
| Image Processing | Clipdrop API |

## Quick Start

```bash
# Clone the repository
git clone <repository-url>
cd bg-removal

# Install dependencies
cd server && npm install
cd ../client && npm install

# Setup environment variables
# See SETUP_GUIDE.md for details

# Run development servers
# Terminal 1: cd server && npm run dev
# Terminal 2: cd client && npm run dev
```

## Project Structure

```
bg-removal/
├── client/                 # React frontend
│   ├── src/
│   │   ├── components/     # UI components
│   │   ├── pages/          # Page routes
│   │   └── context/        # State management
│   └── package.json
│
└── server/                 # Express backend
    ├── controllers/        # Route handlers
    ├── routes/            # API routes
    ├── models/            # Database models
    ├── middlewares/       # Auth & upload
    └── package.json
```

## Credit Plans

| Plan | Price | Credits |
|------|-------|---------|
| Basic | ₹10 | 100 |
| Advanced | ₹50 | 500 |
| Business | ₹250 | 5000 |

## API Endpoints

- `POST /api/image/remove-bg` - Remove image background
- `GET /api/user/credits` - Get user credits
- `POST /api/user/pay-razor` - Razorpay payment
- `POST /api/user/pay-stripe` - Stripe payment

## Documentation

- [Setup Guide](./SETUP_GUIDE.md)
- [Technical Documentation](./TECHNICAL_DOCUMENTATION.md)

## License

ISC