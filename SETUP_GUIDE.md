# Setup Guide

## Prerequisites

Before setting up the project, ensure you have the following installed:

| Requirement | Version | Description |
|-------------|---------|-------------|
| Node.js | 18+ | JavaScript runtime |
| npm | 9+ | Node package manager |
| MongoDB | Latest | Database (local or Atlas) |
| Git | Latest | Version control |

---

## Project Structure

```
bg-removal/
├── client/          # React frontend
└── server/          # Express backend
```

---

## Step 1: Clone the Repository

```bash
git clone <repository-url>
cd bg-removal
```

---

## Step 2: Environment Setup

### Backend Configuration

Create a `.env` file in the `server/` directory:

```env
# Server Configuration
PORT=4000

# MongoDB Connection
MONGO_URI=your_mongodb_connection_string

# Clerk Authentication
CLERK_WEBHOOK_SECRET=your_clerk_webhook_secret

# Image Processing API
CLIPDROP_API=your_clipdrop_api_key

# Payment Gateways
STRIPE_SECRET_KEY=your_stripe_secret_key
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret

# Currency
CURRENCY=INR

# JWT Secret
JWT_SECRET=your_jwt_secret
```

### Frontend Configuration

Create a `.env` file in the `client/` directory:

```env
VITE_BACKEND_URL=http://localhost:4000
VITE_RAZORPAY_KEY_ID=your_razorpay_key_id
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
```

---

## Step 3: Required API Accounts

### 1. Clerk (Authentication)
1. Go to [Clerk Dashboard](https://dashboard.clerk.com)
2. Create a new application
3. Get your publishable key and secret key
4. Configure webhook URL for production
5. Add custom session token:
   - Go to **Sessions** → **Custom Session Token**
   - Add template:
     ```json
     {
       "clerkId": "{{user.id}}"
     }
     ```

### 2. MongoDB (Database)
- **Option A**: [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) (Cloud)
  - Create cluster, get connection string
  
- **Option B**: Local MongoDB
  - Install MongoDB locally
  - Connection: `mongodb://localhost:27017/bg-removal`

### 3. Clipdrop API (Image Processing)
1. Go to [Clipdrop API](https://clipdrop.co/apis)
2. Sign up for an account
3. Get your API key
4. Free tier includes some credits

### 4. Razorpay (Payment Gateway)
1. Go to [Razorpay](https://razorpay.com)
2. Create a merchant account
3. Get Key ID and Key Secret
4. Enable test mode for development

### 5. Stripe (Payment Gateway)
1. Go to [Stripe Dashboard](https://dashboard.stripe.com)
2. Create an account
3. Get your secret key (use test mode for development)
4. Configure webhook for production

---

## Step 4: Install Dependencies

### Backend
```bash
cd server
npm install
```

### Frontend
```bash
cd client
npm install
```

---

## Step 5: Run the Project

### Start Backend (Terminal 1)
```bash
cd server
npm run server
```
- Server runs on `http://localhost:4000`

### Start Frontend (Terminal 2)
```bash
cd client
npm run dev
```
- Client runs on `http://localhost:5173` (default Vite port)

---

## Step 6: Verify Setup

### Backend Health Check
```bash
curl http://localhost:4000
# Expected: "API Working"
```

### Frontend Access
Open `http://localhost:5173` in your browser

---

## Development Commands

### Backend
| Command | Description |
|---------|-------------|
| `npm run server` | Start with nodemon (dev) |
| `npm start` | Start production server |

### Frontend
| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Create production build |
| `npm run lint` | Run ESLint |

---

## Production Deployment

### Backend (e.g., Vercel)
1. Configure `vercel.json` in server folder
2. Add environment variables in Vercel dashboard
3. Deploy from GitHub

### Frontend (e.g., Vercel)
1. Configure `vercel.json` in client folder
2. Add environment variables in Vercel dashboard
3. Deploy from GitHub

---

## Troubleshooting

### Common Issues

1. **MongoDB Connection Error**
   - Check if MongoDB URI is correct
   - Ensure network access is allowed
   - Verify database name in URI

2. **Clerk Authentication Error**
   - Verify Clerk keys are correct
   - Check custom session token configuration
   - Ensure webhook secret matches

3. **Payment Gateway Error**
   - Verify API keys are valid
   - Check test mode is enabled (for testing)
   - Ensure callback URLs are configured

4. **Image Processing Error**
   - Verify Clipdrop API key
   - Check image file format (JPG, PNG)
   - Verify file size limits

### Port Already in Use
```bash
# Find process using port
lsof -i :4000  # for backend
lsof -i :5173  # for frontend

# Kill the process
kill -9 <PID>
```

---

## Folder Structure Summary

```
bg-removal/
├── client/                    # Frontend
│   ├── .env                   # Frontend env vars
│   ├── src/
│   │   ├── components/        # UI components
│   │   ├── pages/            # Page routes
│   │   ├── context/          # State management
│   │   └── App.jsx           # Main app
│   └── package.json
│
├── server/                    # Backend
│   ├── .env                   # Backend env vars
│   ├── controllers/          # Route handlers
│   ├── routes/               # API routes
│   ├── models/               # Database models
│   ├── middlewares/          # Auth, upload
│   └── package.json
│
└── README.md                 # Project readme
```

---

## Next Steps

1. Configure all API keys in environment variables
2. Test authentication flow with Clerk
3. Test payment flow (use test mode)
4. Test image upload and background removal
5. Deploy to production

---

## Support

For issues or questions, refer to:
- [Clerk Docs](https://clerk.com/docs)
- [Stripe Docs](https://stripe.com/docs)
- [Razorpay Docs](https://razorpay.com/docs)
- [Clipdrop API Docs](https://clipdrop.co/apis/docs)