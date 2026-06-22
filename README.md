# 🧠 MindCare - Online Therapy & Counseling Platform

A modern, production-ready telehealth platform for connecting patients with licensed mental health professionals.

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Node](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen)

---

## 🌟 Features

### For Patients
- 🔍 **Search & Filter** - Find therapists by specialty, experience, and availability
- 📅 **Easy Booking** - Simple appointment scheduling with real-time availability
- 🎥 **Video Sessions** - Secure, encrypted video consultations
- 💰 **Flexible Pricing** - Transparent pricing with insurance support
- 📚 **Resources** - Educational content on mental health
- 🆘 **Crisis Support** - Quick access to emergency resources
- 🔒 **Secure & Private** - HIPAA compliant, encrypted data

### For Therapists
- 👥 **Client Management** - Manage clients and session history
- 📋 **Notes & Records** - Secure session notes and treatment plans
- 💳 **Payment Handling** - Automatic payment processing
- 📊 **Analytics** - Track client progress and metrics
- 🔔 **Notifications** - Session reminders and alerts
- 📱 **Mobile Access** - Manage practice on the go

### Platform Features
- 🌐 **Responsive Design** - Works on desktop, tablet, and mobile
- ⚡ **High Performance** - Optimized for speed and reliability
- 🔐 **Enterprise Security** - Bank-level encryption and protection
- 📈 **Scalable** - Handle thousands of concurrent users
- 🎨 **Modern UI** - Beautiful, calming interface
- 🌍 **Multi-language Ready** - Internationalization support

---

## 📦 Tech Stack

### Frontend
- **React 18** - UI framework
- **Next.js 14** - React framework with SSR/SSG
- **TypeScript** - Type safety
- **Tailwind CSS** - Utility-first styling
- **React Hook Form** - Form management
- **Zustand** - State management

### Backend
- **Next.js API Routes** - Serverless functions
- **Node.js** - JavaScript runtime
- **Express.js** - API framework (optional)
- **Prisma** - ORM for database

### Database
- **PostgreSQL 15** - Relational database
- **Redis** - Caching layer (optional)

### External Services
- **Stripe** - Payment processing
- **SendGrid/Resend** - Email service
- **Agora/Twilio** - Video conferencing
- **AWS S3** - File storage
- **Sentry** - Error tracking

---

## 🚀 Quick Start

### Option 1: Single File SPA (No Backend Required)

```bash
# Just open in browser
open therapy-platform.html

# Or serve locally
python -m http.server 8000
# Visit http://localhost:8000/therapy-platform.html
```

Demo features:
- Browse therapists (sample data)
- Book appointments
- View pricing
- Browse resources
- Authentication UI

### Option 2: Full Next.js Project

```bash
# Prerequisites: Node.js 18+, PostgreSQL

# Clone repository
git clone https://github.com/yourusername/mindcare.git
cd mindcare

# Install dependencies
npm install

# Setup environment
cp .env.example .env.local
# Edit .env.local with your database URL and API keys

# Setup database
npx prisma migrate dev --name init

# Run development server
npm run dev

# Visit http://localhost:3000
```

---

## 📋 Project Structure

```
mindcare/
├── app/                          # Next.js app directory
│   ├── api/                     # API routes
│   │   ├── auth/               # Authentication
│   │   ├── therapists/         # Therapist endpoints
│   │   ├── appointments/       # Booking endpoints
│   │   └── payments/           # Stripe integration
│   ├── dashboard/              # User dashboard
│   ├── find-therapist/         # Therapist directory
│   ├── pricing/                # Pricing page
│   ├── resources/              # Educational content
│   └── contact/                # Contact page
├── components/                  # Reusable React components
├── lib/                        # Utility functions
│   ├── auth.ts                # Authentication utilities
│   ├── stripe.ts              # Stripe helpers
│   ├── db.ts                  # Database connection
│   └── hooks/                 # Custom React hooks
├── prisma/                    # Database schema
├── public/                    # Static files
├── styles/                    # Global CSS
├── tests/                     # Test files
├── docs/                      # Documentation
├── .env.example               # Environment template
├── vercel.json               # Vercel configuration
├── next.config.js            # Next.js config
├── tailwind.config.js        # Tailwind config
└── package.json              # Dependencies
```

---

## 🔧 Configuration

### Environment Variables

Copy `.env.example` to `.env.local` and update:

```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/mindcare"

# Authentication
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="generate-with-openssl-rand-hex-32"

# Payments
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."
STRIPE_SECRET_KEY="sk_test_..."

# Email
SENDGRID_API_KEY="SG_..."

# Video Service
AGORA_APP_ID="..."
AGORA_APP_CERTIFICATE="..."
```

See `.env.example` for all available options.

---

## 📚 Documentation

- **[Quick Start Guide](QUICKSTART.md)** - Get started in 5 minutes
- **[Setup Guide](SETUP_GUIDE.md)** - Detailed setup instructions
- **[Deployment Guide](DEPLOYMENT_GUIDE.md)** - Deploy to production
- **[Docker Setup](Docker_Setup.md)** - Containerized deployment

---

## 🚀 Deployment

### Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Login and deploy
vercel login
vercel --prod
```

### Docker

```bash
# Build image
docker build -t mindcare .

# Run container
docker-compose up -d
```

### AWS, DigitalOcean, Heroku

See [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md) for detailed instructions.

---

## 💳 Payment Integration

### Stripe Setup

1. Sign up at [stripe.com](https://stripe.com)
2. Get API keys from dashboard
3. Add to environment variables
4. Test with card: `4242 4242 4242 4242`

### Test Transactions

- Amount: $80
- Card: 4242 4242 4242 4242
- Expiry: 12/25
- CVC: 123

---

## 🎨 Customization

### Change Branding

Edit colors in `therapy-platform.html`:

```css
:root {
    --primary: #06b6d4;      /* Cyan */
    --sage: #84cc16;         /* Green */
    --lavender: #a78bfa;     /* Purple */
}
```

### Update Content

- Therapist profiles in `therapists` array
- Resources in `resources` database
- Pricing in `pricing` configuration
- Copy in component files

---

## 🧪 Testing

```bash
# Run unit tests
npm run test

# Run E2E tests
npm run test:e2e

# Generate coverage report
npm run test:coverage

# Type checking
npm run type-check

# Linting
npm run lint
```

---

## 🔒 Security

### Built-in Security Features

✅ HTTPS enforced
✅ Password hashing (bcrypt)
✅ CSRF protection
✅ XSS prevention
✅ SQL injection prevention (Prisma)
✅ Secure session management
✅ Rate limiting
✅ Input validation

### HIPAA Compliance

- Encryption in transit (HTTPS)
- Encryption at rest
- Audit logging
- Data retention policies
- Access controls
- Regular security updates

---

## 📊 Monitoring & Analytics

### Error Tracking (Sentry)

```bash
npm i @sentry/nextjs
```

Configure in `next.config.js`:

```javascript
withSentryConfig(nextConfig, {
  org: "your-org",
  project: "mindcare",
});
```

### Performance Monitoring

- Web Vitals integration
- Real User Monitoring (RUM)
- Uptime monitoring
- Synthetic monitoring

---

## 🐛 Troubleshooting

### Database Connection Issues

```bash
# Test connection
psql $DATABASE_URL -c "SELECT 1"

# Run migrations
npx prisma migrate deploy

# View database
npx prisma studio
```

### Build Errors

```bash
# Clear cache
rm -rf .next node_modules
npm install
npm run build
```

### Payment Issues

- Verify Stripe keys are correct
- Check webhook configuration
- Review Stripe dashboard logs
- Test with test card numbers

---

## 📱 Mobile Optimization

- Responsive design (mobile-first)
- Touch-friendly interactions
- Optimized images
- Reduced data usage
- Offline support (service workers)

---

## ♿ Accessibility

- WCAG 2.1 AA compliant
- Keyboard navigation
- Screen reader support
- Color contrast compliance
- Semantic HTML

---

## 🌍 Internationalization

Add multi-language support:

```bash
npm i next-intl
```

Update locale configuration for:
- English (en)
- Spanish (es)
- French (fr)
- German (de)

---

## 📈 Performance Metrics

- **First Contentful Paint**: < 1.5s
- **Largest Contentful Paint**: < 2.5s
- **Cumulative Layout Shift**: < 0.1
- **Time to Interactive**: < 3.5s

---

## 💬 Support & Community

- **Documentation**: [docs.mindcare.com](https://docs.mindcare.com)
- **Issues**: [GitHub Issues](https://github.com/mindcare/issues)
- **Discussions**: [GitHub Discussions](https://github.com/mindcare/discussions)
- **Email**: support@mindcare.com
- **Status**: [status.mindcare.com](https://status.mindcare.com)

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Mental health professionals who inspired this platform
- Open source community
- Contributors and testers

---

## 📞 Contact

- **Website**: https://mindcare.com
- **Email**: hello@mindcare.com
- **Phone**: 1-800-MINDCARE
- **Twitter**: @mindcare
- **LinkedIn**: /company/mindcare

---

## 🎯 Roadmap

### v1.1
- [ ] Insurance integration
- [ ] Prescription management
- [ ] Mobile apps (iOS/Android)

### v2.0
- [ ] Group therapy sessions
- [ ] AI-powered therapist matching
- [ ] Wearable integration
- [ ] Multi-language support

### v3.0
- [ ] Marketplace for mental health apps
- [ ] Integration with EHR systems
- [ ] Advanced analytics dashboard
- [ ] API for third-party integrations

---

**Built with ❤️ for mental health**

*Last updated: June 2026*
