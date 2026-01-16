# ⚡ Flux

> **An adaptive resolution tracker that thinks and evolves with your life.**

Flux isn't just another habit tracker—it's an AI-powered architect that recalibrates your goals in real-time based on your calendar, activity patterns, and life context. Built for 2026, designed to adapt, not dictate.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Next.js](https://img.shields.io/badge/Next.js-15-black)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue)
![Tailwind](https://img.shields.io/badge/Tailwind-3.4-38bdf8)

## 🎯 The Problem

Traditional habit trackers are rigid streak counters. Miss one day? Streak broken. Motivation gone.

**Flux solves this by:**
- 🧠 **Adapting goals automatically** when your calendar shows back-to-back meetings
- 📅 **Injecting habits into your timeline** as calendar blocks
- 🏃 **Syncing with health apps** to auto-mark completions
- 💡 **Learning your patterns** to optimize scheduling

## ✨ Core Features

### 🔄 Adaptive Goal Recalibration
When you miss a "30-minute workout" because your day exploded, Flux's AI suggests a "5-minute wall-sit" to keep the habit loop alive.

```
Original Goal: 30-minute morning workout
Context Detected: Back-to-back meetings starting at 7 AM
AI Adaptation: 5-minute wall-sit during your 11 AM break
Result: Habit chain preserved, momentum maintained
```

### 📆 Unified Timeline Integration
Your resolutions appear as time blocks in Google Calendar. No separate app to check.

- Automatic calendar sync via Nylas/Google Calendar API
- Habits become visible commitments alongside your meetings
- Smart rescheduling when conflicts arise

### 🤖 AI Interview & SMART Generator
Onboarding isn't a form—it's a conversation. Flux asks why you want to change, then generates specific, measurable goals.

**Example Flow:**
```
Flux: "Why do you want to get fit?"
You: "I want more energy for my kids"
Flux: "How much time can you realistically commit daily?"
You: "Maybe 20 minutes"
Flux: ✅ Created "20-min family walk after dinner" 
       (Scheduled: 7:00 PM daily)
```

### 📊 AI-Powered Insights
- Peak performance windows (e.g., "You complete 78% more tasks before 10 AM")
- Adaptation patterns (e.g., "AI has adapted your goals 12 times this week")
- Streak analysis with micro-habit recommendations

### 💪 Health App Sync
Connect Apple Health or Google Fit. When your watch detects a 20-minute walk, Flux auto-marks the resolution complete.

## 🏗️ Tech Stack

### Frontend
- **Next.js 15** - App Router, Server Components, Server Actions
- **React 19** - Concurrent rendering, Suspense
- **TypeScript** - Full type safety
- **Tailwind CSS** - Liquid Glass UI (backdrop-blur, gradients, translucency)

### Backend
- **Supabase** - PostgreSQL, Auth, Realtime subscriptions
- **Anthropic Claude** - Sonnet 4.5 for adaptive intelligence
- **Edge Functions** - Serverless API routes

### Integrations
- **Nylas/Google Calendar API** - Calendar injection
- **Apple HealthKit** - iOS activity sync
- **Google Fit API** - Android activity sync
- **Vercel** - Deployment, Analytics, Edge Network

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- Supabase account
- Anthropic API key

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/flux.git
cd flux

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local
```

### Environment Variables

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key

# Anthropic AI
ANTHROPIC_API_KEY=your_anthropic_api_key

# Calendar (choose one)
GOOGLE_CALENDAR_CLIENT_ID=your_client_id
GOOGLE_CALENDAR_CLIENT_SECRET=your_client_secret
# OR
NYLAS_CLIENT_ID=your_nylas_client_id
NYLAS_CLIENT_SECRET=your_nylas_client_secret
```

### Database Setup

```sql
-- Run in Supabase SQL Editor

-- Users table (handled by Supabase Auth)

-- Goals table
CREATE TABLE goals (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  title TEXT NOT NULL,
  original_text TEXT NOT NULL,
  adapted_text TEXT,
  scheduled_time TIME,
  streak INT DEFAULT 0,
  adaptive_score DECIMAL(3,2) DEFAULT 0.00,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Completions table
CREATE TABLE completions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  goal_id UUID REFERENCES goals(id) ON DELETE CASCADE,
  completed_at TIMESTAMPTZ DEFAULT NOW(),
  was_adapted BOOLEAN DEFAULT FALSE
);

-- Adaptations table
CREATE TABLE adaptations (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  goal_id UUID REFERENCES goals(id) ON DELETE CASCADE,
  reason TEXT NOT NULL,
  suggested_change TEXT NOT NULL,
  accepted BOOLEAN DEFAULT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Row Level Security
ALTER TABLE goals ENABLE ROW LEVEL SECURITY;
ALTER TABLE completions ENABLE ROW LEVEL SECURITY;
ALTER TABLE adaptations ENABLE ROW LEVEL SECURITY;

-- Policies
CREATE POLICY "Users can view own goals" 
  ON goals FOR SELECT 
  USING (auth.uid() = user_id);

CREATE POLICY "Users can insert own goals" 
  ON goals FOR INSERT 
  WITH CHECK (auth.uid() = user_id);
```

### Run Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

## 📁 Project Structure

```
flux/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   └── signup/
│   ├── (dashboard)/
│   │   ├── today/
│   │   ├── calendar/
│   │   └── insights/
│   ├── api/
│   │   ├── adapt-goal/
│   │   ├── sync-calendar/
│   │   └── health-sync/
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── ui/
│   ├── goals/
│   ├── calendar/
│   └── insights/
├── lib/
│   ├── supabase/
│   ├── ai/
│   └── integrations/
├── public/
└── package.json
```

## 🎨 Design Philosophy: Liquid Glass

Flux embraces 2026's signature aesthetic—**Liquid Glass**:

- **Translucent cards** with `backdrop-blur-xl`
- **Layered depth** with `bg-white/10` opacity
- **Gradient orbs** for ambient background animation
- **Glassmorphism borders** using `border-white/20`
- **Smooth animations** with Tailwind transitions

```jsx
// Example component styling
<div className="backdrop-blur-xl bg-white/10 rounded-2xl border border-white/20 shadow-2xl">
  {/* Content */}
</div>
```

## 🤝 Contributing

Flux is currently a solo project, but contributions are welcome!

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 🗓️ Roadmap

### Phase 1: MVP (Weeks 1-4) ✅
- [x] Liquid Glass UI prototype
- [x] Goal creation and tracking
- [x] Basic AI adaptations
- [ ] Supabase integration
- [ ] Authentication

### Phase 2: Intelligence (Weeks 5-8)
- [ ] Claude Sonnet 4.5 integration
- [ ] Real-time goal adaptation
- [ ] Pattern recognition
- [ ] AI interview onboarding

### Phase 3: Integrations (Weeks 9-12)
- [ ] Google Calendar sync
- [ ] Apple Health integration
- [ ] Google Fit integration
- [ ] Automatic completion detection

### Phase 4: Insights (Weeks 13-16)
- [ ] Performance analytics
- [ ] AI-generated insights
- [ ] Streak visualizations
- [ ] Year in Review feature

### Phase 5: Growth (Ongoing)
- [ ] Mobile apps (iOS/Android via Capacitor)
- [ ] Social sharing
- [ ] Community features
- [ ] Premium tier with advanced AI

## 📊 Metrics & Analytics

Track what matters:
- **Completion Rate**: % of goals completed (original + adapted)
- **Adaptation Acceptance**: How often users accept AI suggestions
- **Streak Maintenance**: Average streak length across users
- **Peak Performance**: When users are most productive

## 🔒 Privacy & Security

- **End-to-end encryption** for sensitive health data
- **Row Level Security** in Supabase ensures data isolation
- **No data selling** - your habits are yours alone
- **GDPR compliant** - full data export and deletion

## 📝 License

This project is licensed  under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🌟 Acknowledgments

- **Anthropic Claude** for intelligent adaptations
- **Supabase** for the backend infrastructure
- **Vercel** for seamless deployment
- **The 2026 design community** for Liquid Glass inspiration

## 📧 Contact

**Built by:** [Your Name]  
**Twitter:** [@yourhandle](https://twitter.com/yourhandle)  
**Email:** your.email@example.com

---

<div align="center">

**⚡ Flux adapts to your life, not the other way around.**

[Website](https://flux.app) • [Documentation](https://docs.flux.app) • [Twitter](https://twitter.com/fluxapp)

</div>