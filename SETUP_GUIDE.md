# MindCare - Online Therapy & Counseling Platform

A modern, production-ready mental health platform built with Next.js, React, and Tailwind CSS.

## Project Overview

MindCare is a comprehensive telehealth therapy platform featuring:

- **Home Page** - Hero section with statistics and value propositions
- **Therapist Directory** - Browse and filter licensed professionals
- **Appointment Booking** - Easy scheduling with availability management
- **Video Consultations** - Secure video sessions dashboard
- **Mental Health Resources** - Educational articles, exercises, and crisis support
- **Pricing Plans** - Transparent, flexible pricing tiers
- **User Authentication** - Secure login and signup
- **Payment Integration** - Stripe for secure transactions
- **Responsive Design** - Mobile-first approach, works on all devices

## Tech Stack

- **Frontend**: React 18, Next.js 14
- **Styling**: Tailwind CSS, custom CSS animations
- **State Management**: React Context + Zustand (for scalability)
- **Authentication**: NextAuth.js with JWT
- **Payment**: Stripe.js and Stripe API
- **Database**: PostgreSQL with Prisma ORM
- **Real-time Communication**: Socket.io for video sessions
- **Deployment**: Vercel (recommended) or Docker

## Project Structure

```
mindcare/
├── app/
│   ├── layout.tsx                 # Root layout
│   ├── page.tsx                   # Home page
│   ├── api/
│   │   ├── auth/[...nextauth]/   # NextAuth configuration
│   │   ├── therapists/           # Therapist endpoints
│   │   ├── appointments/         # Booking endpoints
│   │   ├── payments/             # Stripe integration
│   │   └── resources/            # Content endpoints
│   ├── dashboard/
│   │   ├── page.tsx              # User dashboard
│   │   ├── bookings/             # Booking history
│   │   └── sessions/             # Video session page
│   ├── find-therapist/
│   │   └── page.tsx              # Directory page
│   ├── pricing/
│   │   └── page.tsx              # Pricing page
│   ├── resources/
│   │   └── page.tsx              # Resources page
│   └── contact/
│       └── page.tsx              # Contact page
├── components/
│   ├── navigation/
│   │   ├── Navbar.tsx
│   │   └── Footer.tsx
│   ├── therapist/
│   │   ├── TherapistCard.tsx
│   │   └── TherapistGrid.tsx
│   ├── booking/
│   │   ├── BookingForm.tsx
│   │   └── BookingSummary.tsx
│   ├── video/
│   │   ├── VideoRoom.tsx
│   │   └── SessionControls.tsx
│   ├── auth/
│   │   ├── LoginForm.tsx
│   │   └── SignupForm.tsx
│   └── shared/
│       ├── Button.tsx
│       ├── Card.tsx
│       └── Modal.tsx
├── lib/
│   ├── db.ts                     # Database connection
│   ├── prisma.ts                 # Prisma client
│   ├── auth.ts                   # Auth utilities
│   ├── stripe.ts                 # Stripe utilities
│   └── hooks/
│       ├── useAuth.ts
│       ├── useTherapists.ts
│       └── useBookings.ts
├── types/
│   └── index.ts                  # TypeScript types
├── public/
│   ├── images/
│   └── icons/
├── prisma/
│   └── schema.prisma             # Database schema
├── styles/
│   └── globals.css               # Global styles
├── .env.local                    # Environment variables
├── .env.example                  # Environment template
├── next.config.js                # Next.js configuration
├── tsconfig.json                 # TypeScript configuration
├── tailwind.config.js            # Tailwind configuration
└── package.json                  # Dependencies

```

## Installation

### Prerequisites

- Node.js 18+ and npm/yarn
- PostgreSQL database
- Stripe account
- Git

### Step 1: Clone the Repository

```bash
git clone https://github.com/yourusername/mindcare.git
cd mindcare
```

### Step 2: Install Dependencies

```bash
npm install
# or
yarn install
```

### Step 3: Environment Variables

Create a `.env.local` file in the root directory:

```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/mindcare_db"

# NextAuth Configuration
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-secret-key-here-generate-with-openssl-rand-hex-32"

# Stripe
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_your_key"
STRIPE_SECRET_KEY="sk_test_your_key"
STRIPE_WEBHOOK_SECRET="whsec_your_secret"

# Email Service (SendGrid or Resend)
SENDGRID_API_KEY="your_sendgrid_key"
# or
RESEND_API_KEY="your_resend_key"

# AWS S3 (for document storage)
AWS_ACCESS_KEY_ID="your_access_key"
AWS_SECRET_ACCESS_KEY="your_secret_key"
AWS_S3_BUCKET="mindcare-bucket"
AWS_REGION="us-east-1"

# Video Service (Agora or Twilio)
AGORA_APP_ID="your_agora_app_id"
AGORA_APP_CERTIFICATE="your_app_certificate"
# or
TWILIO_ACCOUNT_SID="your_account_sid"
TWILIO_AUTH_TOKEN="your_auth_token"

# API Keys
NEXT_PUBLIC_API_URL="http://localhost:3000"

# Feature Flags
NEXT_PUBLIC_ENABLE_VIDEO_SESSIONS="true"
NEXT_PUBLIC_ENABLE_PRESCRIPTIONS="true"
```

### Step 4: Database Setup

```bash
# Install PostgreSQL (macOS)
brew install postgresql
brew services start postgresql

# Create database
createdb mindcare_db

# Run migrations
npx prisma migrate dev --name init

# (Optional) Seed database
npx prisma db seed
```

### Step 5: Run Development Server

```bash
npm run dev
# or
yarn dev
```

Visit `http://localhost:3000` in your browser.

## Key Features Implementation

### 1. Authentication

Uses NextAuth.js with email/password and optional OAuth providers:

```typescript
// app/api/auth/[...nextauth]/route.ts
import NextAuth from "next-auth"
import CredentialsProvider from "next-auth/providers/credentials"

const handler = NextAuth({
  providers: [
    CredentialsProvider({
      credentials: {
        email: { label: "Email", type: "email" },
        password: { label: "Password", type: "password" }
      },
      async authorize(credentials) {
        // Verify against database
        const user = await prisma.user.findUnique({
          where: { email: credentials?.email }
        })
        // Compare password and return user
      }
    })
  ],
  callbacks: {
    async jwt({ token, user }) {
      if (user) token.id = user.id
      return token
    },
    async session({ session, token }) {
      session.user.id = token.id
      return session
    }
  }
})
```

### 2. Therapist Management

```typescript
// lib/hooks/useTherapists.ts
import { useEffect, useState } from 'react'

export function useTherapists(filters = {}) {
  const [therapists, setTherapists] = useState([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    const fetchTherapists = async () => {
      const query = new URLSearchParams(filters)
      const response = await fetch(`/api/therapists?${query}`)
      setTherapists(await response.json())
      setLoading(false)
    }
    fetchTherapists()
  }, [filters])

  return { therapists, loading }
}
```

### 3. Payment Integration

```typescript
// app/api/payments/create-session/route.ts
import Stripe from 'stripe'

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!)

export async function POST(req: Request) {
  const { appointmentId, amount } = await req.json()

  const session = await stripe.checkout.sessions.create({
    payment_method_types: ['card'],
    line_items: [{
      price_data: {
        currency: 'usd',
        product_data: { name: 'Therapy Session' },
        unit_amount: amount * 100,
      },
      quantity: 1,
    }],
    mode: 'payment',
    success_url: `${process.env.NEXTAUTH_URL}/success?session_id={CHECKOUT_SESSION_ID}`,
    cancel_url: `${process.env.NEXTAUTH_URL}/cancel`,
  })

  return Response.json({ sessionId: session.id })
}
```

### 4. Video Sessions

```typescript
// components/video/VideoRoom.tsx
import { useState, useEffect } from 'react'
import AgoraRTC from 'agora-rtc-sdk-ng'

export default function VideoRoom({ channelName }: { channelName: string }) {
  const [agoraEngine, setAgoraEngine] = useState<any>(null)

  useEffect(() => {
    const engine = AgoraRTC.createClient({ mode: 'rtc', codec: 'vp8' })
    setAgoraEngine(engine)

    return () => {
      engine.removeAllListeners()
    }
  }, [])

  // Join room, start video, etc.
}
```

### 5. Real-time Notifications

```typescript
// lib/notifications.ts
import nodemailer from 'nodemailer'

const transporter = nodemailer.createTransport({
  service: 'sendgrid',
  auth: {
    user: 'apikey',
    pass: process.env.SENDGRID_API_KEY,
  }
})

export async function sendSessionReminder(email: string, sessionTime: Date) {
  await transporter.sendMail({
    from: 'noreply@mindcare.com',
    to: email,
    subject: 'Upcoming Therapy Session Reminder',
    html: `<p>Your session is scheduled for ${sessionTime.toLocaleString()}</p>`
  })
}
```

## Database Schema (Prisma)

Key models in `prisma/schema.prisma`:

```prisma
model User {
  id            String    @id @default(cuid())
  email         String    @unique
  name          String?
  password      String
  phone         String?
  avatar        String?
  createdAt     DateTime  @default(now())
  
  appointments  Appointment[]
  sessions      Session[]
  reviews       Review[]
}

model Therapist {
  id              String    @id @default(cuid())
  userId          String    @unique
  user            User      @relation(fields: [userId], references: [id])
  licenses        String[]
  specialties     String[]
  bio             String
  hourlyRate      Float
  availability    Json      // Weekly schedule
  ratings         Float     @default(5.0)
  
  appointments    Appointment[]
  sessions        Session[]
  reviews         Review[]
}

model Appointment {
  id            String    @id @default(cuid())
  userId        String
  user          User      @relation(fields: [userId], references: [id])
  therapistId   String
  therapist     Therapist @relation(fields: [therapistId], references: [id])
  
  startTime     DateTime
  duration      Int       // minutes
  status        String    // scheduled, completed, cancelled
  notes         String?
  
  payment       Payment?
  session       Session?
  
  createdAt     DateTime  @default(now())
}

model Session {
  id              String    @id @default(cuid())
  appointmentId   String    @unique
  appointment     Appointment @relation(fields: [appointmentId], references: [id])
  
  recordingUrl    String?
  notes           String?
  nextSessionDate DateTime?
  
  createdAt       DateTime  @default(now())
}

model Payment {
  id              String    @id @default(cuid())
  appointmentId   String    @unique
  appointment     Appointment @relation(fields: [appointmentId], references: [id])
  
  amount          Float
  currency        String    @default("usd")
  status          String    // pending, completed, failed
  stripeSessionId String?
  
  createdAt       DateTime  @default(now())
}

model Review {
  id            String    @id @default(cuid())
  userId        String
  user          User      @relation(fields: [userId], references: [id])
  therapistId   String
  therapist     Therapist @relation(fields: [therapistId], references: [id])
  
  rating        Int       // 1-5
  comment       String?
  
  createdAt     DateTime  @default(now())
}
```

## Deployment

### Option 1: Deploy to Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Login and deploy
vercel
# Follow prompts to connect your GitHub repo
```

**Vercel Environment Variables**: Set in dashboard:
- DATABASE_URL (PostgreSQL on Railway/Heroku)
- NEXTAUTH_SECRET
- NEXTAUTH_URL
- Stripe keys
- etc.

### Option 2: Deploy with Docker

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm ci

# Build application
COPY . .
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

```bash
docker build -t mindcare .
docker run -p 3000:3000 mindcare
```

### Option 3: Deploy to AWS

```bash
# Using Amplify
amplify init
amplify add hosting
amplify push

# Or using EC2/ECS with load balancing
# Create RDS PostgreSQL database
# Deploy with CodeDeploy
```

### Environment Setup for Production

1. **Database**: Use AWS RDS or Heroku PostgreSQL
   ```bash
   # Example connection string
   postgresql://user:pass@db.provider.com:5432/mindcare_prod
   ```

2. **Email Service**: 
   - SendGrid (recommended)
   - AWS SES
   - Resend

3. **Video/Calling**:
   - Agora.io (recommended, $0.99 per 1000 minutes)
   - Twilio ($0.10 per minute)

4. **Storage**:
   - AWS S3 for session recordings
   - Cloudinary for user avatars

5. **Monitoring**:
   - Sentry for error tracking
   - DataDog or New Relic for monitoring

## Running Tests

```bash
# Unit tests
npm run test

# E2E tests
npm run test:e2e

# Coverage
npm run test:coverage
```

## Performance Optimization

- Image optimization with `next/image`
- Code splitting and lazy loading
- CDN for static assets
- Database query optimization with Prisma
- Redis caching for therapist listings
- Service workers for offline capability

## Security Best Practices

✅ **Implemented**:
- HTTPS enforced
- CSRF protection
- Password hashing with bcrypt
- Rate limiting on auth endpoints
- SQL injection prevention (Prisma)
- XSS protection
- HIPAA compliance for health data
- Secure session management

## API Documentation

See `/docs/api.md` for complete endpoint documentation.

Key endpoints:

- `POST /api/auth/signin` - User login
- `POST /api/auth/signup` - New user registration
- `GET /api/therapists` - List therapists with filters
- `POST /api/appointments` - Create appointment
- `GET /api/appointments` - User's appointments
- `POST /api/payments/create-session` - Stripe checkout
- `GET /api/sessions` - User's video sessions
- `POST /api/resources` - Get mental health content

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see LICENSE file for details.

## Support

- Documentation: `/docs`
- Issues: GitHub Issues
- Email: support@mindcare.com
- Status: https://status.mindcare.com

## Roadmap

- [ ] Insurance integration
- [ ] Prescription management
- [ ] Group therapy sessions
- [ ] Mobile apps (iOS/Android)
- [ ] AI-powered therapist matching
- [ ] Wearable integration
- [ ] Multi-language support

---

**Built with ❤️ for mental health**
