# QuestLog - Fitness & Nutrition RPG Tracker

Transform your fitness journey into an epic RPG adventure! QuestLog gamifies your health goals with quests, XP, levels, and premium features.

## 🎮 Features

### Core Features (Free)
- **Quest System**: Create custom fitness and nutrition quests across 4 categories
  - 🏃 Movement (exercise, steps, activities)
  - 🍎 Nutrition (meals, calories, macros)
  - 😴 Recovery (sleep, rest, stretching)
  - 🧘 Mindset (meditation, journaling, habits)
- **XP & Leveling**: Earn experience points and level up as you complete quests
- **Difficulty Tiers**: Easy (50 XP), Normal (150 XP), Epic (500 XP)
- **Drag & Drop Reordering**: Reorder quests with smooth drag and drop
- **Quick Add**: Rapidly create tasks with Enter key shortcut
- **Progress Dashboard**: Visual circular progress and category breakdowns
- **Calorie Tracking**: Track calories consumed and burned
- **Progress Stats**: View total XP, level, quest completion rates, and calorie balance
- **Category Filtering**: Focus on specific quest types
- **Responsive Design**: Works beautifully on mobile and desktop

### Premium Features 🌟
- ✨ **Unlimited Quests** (free tier limited to 10 active quests)
- 📊 **Advanced Analytics** (coming soon)
- 🎨 **Exclusive Themes** (coming soon)
- 🏆 **Legendary Rewards** (coming soon)

## 🚀 Getting Started

### 1. Sign In

Click **START YOUR QUEST** on the landing page to authenticate with Replit Auth. You can use:
- Email & Password
- Google
- GitHub
- Other OAuth providers

### 2. Create Your First Quest

1. Click the **+ NEW QUEST** button
2. Fill in the details:
   - **Title**: What's your quest? (e.g., "Morning Run 5km")
   - **Category**: Choose Movement, Nutrition, Recovery, or Mindset
   - **Difficulty**: Easy, Normal, or Epic
   - **Calories**: Track calories burned (-300) or consumed (+500)
3. Click **ACCEPT QUEST**

### 3. Complete Quests & Level Up

- Click the checkbox to mark quests as complete
- Earn XP based on difficulty
- Watch your progress bar fill up
- Level up every 1000 XP!

### 4. Upgrade to Premium (Optional)

- Click **UPGRADE NOW** on the premium banner
- Complete checkout with Stripe (test mode)
- Unlock unlimited quests and premium features

## 🛠 Tech Stack

### Frontend
- **React** with TypeScript
- **Wouter** for routing
- **TanStack Query** for data fetching
- **Framer Motion** for animations
- **Tailwind CSS** + **shadcn/ui** for styling
- **Chakra Petch** font for cyber-RPG aesthetic

### Backend
- **Express.js** server
- **PostgreSQL** database (Neon-backed)
- **Drizzle ORM** for type-safe database queries
- **Replit Auth** for authentication (OpenID Connect)
- **Stripe** for payment processing

### Infrastructure
- **Replit** hosting and deployment
- Automatic database migrations
- Session management with PostgreSQL store

## 📁 Project Structure

```
.
├── client/
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── hooks/          # Custom React hooks (auth, toast, etc.)
│   │   ├── lib/            # Utilities and types
│   │   ├── pages/          # Page components (Dashboard, Landing, etc.)
│   │   └── App.tsx         # Main app with routing
│   └── index.html          # HTML entry point
├── server/
│   ├── index.ts            # Express server setup
│   ├── routes.ts           # API routes (quests, auth, billing)
│   ├── storage.ts          # Database storage layer
│   └── replitAuth.ts       # Replit Auth middleware
├── shared/
│   └── schema.ts           # Shared types and Drizzle schema
├── STRIPE_SETUP.md         # Stripe configuration guide
└── README.md               # This file
```

## 🔐 Authentication

QuestLog uses **Replit Auth** for user authentication:

- Supports email/password and social logins (Google, GitHub, etc.)
- Secure OpenID Connect implementation
- Automatic session management
- Protected API routes with middleware

## 💳 Stripe Integration

Premium subscriptions are powered by Stripe. See [STRIPE_SETUP.md](./STRIPE_SETUP.md) for detailed setup instructions.

**Quick Start:**
1. Get Stripe test API keys from [stripe.com/docs/keys](https://stripe.com/docs/keys)
2. Create a product and price in Stripe Dashboard
3. Add secrets in Replit:
   - `STRIPE_SECRET_KEY`
   - `STRIPE_PRICE_ID`
4. Test with card `4242 4242 4242 4242`

## 🗄 Database Schema

### Users Table
```typescript
{
  id: string (UUID, primary key)
  email: string
  isPremium: boolean (default: false)
  stripeCustomerId: string | null
  stripeSubscriptionId: string | null
}
```

### Quests Table
```typescript
{
  id: string (UUID, primary key)
  userId: string (foreign key)
  title: string
  category: string (Movement, Nutrition, Recovery, Mindset, Other)
  difficulty: string (Easy, Normal, Epic)
  calories: number | null (negative = burned, positive = consumed)
  completed: boolean (default: false)
  xp: number
  sortOrder: number (for drag & drop ordering)
  createdAt: timestamp
}
```

## 🎨 Design System

**Theme**: Cyber-Fantasy / Digital RPG
- **Colors**: Neon cyan/blue primary, purple accents, yellow premium tier
- **Font**: Chakra Petch (Google Fonts)
- **Effects**: Glassmorphism cards, subtle animations, glow effects
- **Icons**: Lucide React

**Category Colors**:
- 🏃 Movement: Cyan (`#00D9FF`)
- 🍎 Nutrition: Green (`#00FF88`)
- 😴 Recovery: Purple (`#B794F4`)
- 🧘 Mindset: Pink (`#F687B3`)

## 🧪 Testing

### Test User Authentication
1. Click "START YOUR QUEST" on landing page
2. Sign up with a test email or OAuth provider
3. Verify redirect to dashboard

### Test Quest Flow
1. Create a quest
2. Toggle it complete → verify XP gain
3. Create 11 quests (free users) → verify limit enforcement
4. Delete a quest → verify it's removed

### Test Premium Upgrade
1. Click "UPGRADE NOW"
2. Use test card: `4242 4242 4242 4242`
3. Complete checkout
4. Verify premium banner disappears
5. Create 11+ quests → verify no limit

## 📝 API Reference

### Authentication
- `GET /api/login` - Initiate Replit Auth login
- `GET /api/logout` - End session
- `GET /api/auth/user` - Get current user (protected)

### Quests
- `GET /api/quests` - Get all quests for authenticated user
- `POST /api/quests` - Create a new quest
- `PATCH /api/quests/:id` - Update quest (toggle complete, etc.)
- `DELETE /api/quests/:id` - Delete a quest
- `PUT /api/quests/reorder` - Reorder quests (drag and drop)

### Billing
- `POST /api/billing/create-checkout-session` - Create Stripe checkout
- `POST /api/billing/webhook` - Handle Stripe webhooks

## 🚢 Deployment

QuestLog is deployed on Replit with automatic builds:

1. Push code to your Replit
2. Environment secrets are configured via Replit Secrets
3. Database auto-migrates on startup
4. Publish your Repl to get a live URL

## 🤝 Contributing

This is a solo project, but feedback is welcome! Areas for expansion:
- Advanced analytics dashboard
- Social features (friends, leaderboards)
- Quest templates and achievements
- Mobile app (React Native)
- Integration with fitness APIs (Strava, Apple Health, etc.)

## 📄 License

MIT License - feel free to use this as a template for your own projects!

## 🎯 Roadmap

- [ ] Premium analytics (charts, insights, trends)
- [ ] Achievement system with badges
- [ ] Quest templates library
- [ ] Daily/weekly challenges
- [ ] Fitness API integrations
- [ ] Dark/light theme toggle
- [ ] Export data to CSV
- [ ] Mobile app version

---

Built with ❤️ using Replit, React, Express, PostgreSQL, and Stripe.
