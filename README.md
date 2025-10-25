# Settel – A New Standard for Payments

https://drive.google.com/file/d/1HR54-MdNI9EnOyF9I0wbjuaiZlc5w52Y/view?usp=sharing

Every once in a while, a product comes along that changes everything. It doesn't just improve what came before. It redefines what's possible.

Today, that product is Settel.

## The Old Way

Online payments are stuck in the past. Businesses pay high fees to middlemen like PayPal. Stripe demands registrations, tax IDs, GST numbers, approvals. Customers are forced into clunky flows, clicking through wallets and popups, exposing themselves to scams and exploits.

It's complicated. It's insecure. It's expensive.

## The New Way

Settel makes payments effortless.

Imagine this: a business wants to accept crypto. Instead of contracts, verifications, SDKs, or compliance hurdles, they drop a single button on their website. That's it.

A customer clicks. They enter their crypto ID. Instantly, they receive a payment request, securely delivered by email. On their trusted device, they approve it. Done.

The merchant gets an invoice. A real-time dashboard. Analytics that make sense. No noise, no friction, no middlemen.

**It just works.**

## What Sets Settel Apart

### The Button
One line of code, and any website can accept crypto.

### Email-First Security
Payment requests flow through the inbox, not injected wallets. It's safer, simpler, human.

### Trusted Devices
Customers pay where they feel secure — their phone, their personal device — not inside an unfamiliar popup.

### Real-Time Accuracy
Powered by live price feeds, every transaction is settled at the right value, every time.

### The Merchant Hub
Invoices, analytics, and payment history in one clean dashboard. Not scattered, not hidden.

## Technical Implementation

### Project Structure

```
src/
├── app/                    # Next.js 15 App Router
│   ├── api/               # Backend API routes
│   │   ├── create-transaction/  # Transaction creation
│   │   ├── prices/            # Real-time crypto pricing
│   │   ├── test-pyth/         # Pyth Network testing
│   │   └── transaction-status/[id]/  # Status polling
│   ├── buttons/           # Button management pages
│   │   ├── page.tsx       # Buttons dashboard
│   │   └── [id]/page.tsx  # Individual button view
│   ├── pay/[id]/          # Payment completion flow
│   ├── history/           # Transaction history
│   ├── settings/          # User profile settings
│   ├── onboard/           # User onboarding
│   ├── sign-in/           # Authentication
│   └── Provider.tsx       # App-wide providers
├── components/            # React components
│   ├── ui/               # Reusable UI components (Shadcn)
│   ├── PaymentButtonPage.tsx  # Embeddable payment button
│   ├── CreateButtonDialog.tsx # Button creation modal
│   ├── NavBar.tsx         # Navigation component
│   ├── MainDashboard.tsx  # Main dashboard
│   ├── LandingPage.tsx    # Landing page
│   └── [other components]
├── models/               # MongoDB schemas
│   ├── buttonModel.ts    # Payment button schema
│   ├── transactionModel.ts  # Transaction schema
│   └── profileModel.ts   # User profile schema
├── actions/              # Server actions
│   ├── buttonActions.ts  # Button CRUD operations
│   ├── transactionActions.ts  # Transaction operations
│   └── userActions.ts    # User profile operations
├── lib/                  # Utility libraries
│   ├── mail.ts          # Email notification system
│   ├── pyth.ts          # Pyth Network integration
│   └── utils.ts         # Common utilities
├── utils/                # Helper functions
│   ├── buttonComponentCode.ts  # Generated component code
│   ├── blockExplorer.ts  # Blockchain explorer utilities
│   └── chain.ts         # Blockchain configuration
└── types/                # TypeScript definitions
    └── button.ts         # Button type definitions
```

### Architecture Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Merchant      │    │   Customer      │    │   Email         │
│   Dashboard     │    │   Website       │    │   Notification  │
│                 │    │                 │    │                 │
│ ┌─────────────┐ │    │ ┌─────────────┐ │    │ ┌─────────────┐ │
│ │Create Button│ │    │ │Payment Btn  │ │    │ │Payment Link │ │
│ │Configure    │ │    │ │Click & ID   │ │    │ │Secure Email │ │
│ │Generate Code│ │────▶│ │Entry        │ │────▶│ │Trusted Flow │ │
│ └─────────────┘ │    │ └─────────────┘ │    │ └─────────────┘ │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   MongoDB       │    │   Pyth Network  │    │   Blockchain    │
│   Database      │    │   Price Feeds   │    │   Networks      │
│                 │    │                 │    │                 │
│ • Buttons       │    │ • ETH/USD       │    │ • Ethereum      │
│ • Transactions  │    │ • POL/USD       │    │ • Polygon       │
│ • Profiles      │    │ • OP/USD        │    │ • Arbitrum      │
│ • Real-time     │    │ • AVAX/USD      │    │ • Optimism      │
│   Updates       │    │ • Live Pricing  │    │ • Base/Avalanche│
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Payment Flow Architecture

1. **Button Creation Flow:**
   Merchant → CreateButtonDialog → MongoDB → Generated Embed Code

2. **Payment Initiation Flow:**
   Customer → PaymentButton → Crypto ID → Email Notification

3. **Payment Completion Flow:**
   Email Link → pay/[id] → Wallet Connect → Transaction → Status Update

4. **Real-time Updates:**
   Transaction Status → Polling → MongoDB → Merchant Dashboard

### Core Components

#### 1. PaymentButtonPage.tsx - The Embeddable Button

```typescript
interface PaymentButtonProps {
  buttonId: string;
  amountUsd: number;
  currency?: string;
  merchantName?: string;
  onTransactionStateChange?: (state, transactionId) => void;
}

// Key Features:
// - Crypto ID input collection
// - Real-time transaction polling
// - Status state management (creating, pending, success, failed)
// - Automatic cleanup and reset functionality
```

#### 2. pay/[id]/page.tsx - Payment Completion Interface

```typescript
// Multi-chain payment processing with:
// - Wagmi hooks for blockchain interaction
// - Real-time price fetching from Pyth Network
// - Chain switching and wallet management
// - Transaction confirmation and status updates

const SUPPORTED_CHAINS = {
  "1": "Ethereum Mainnet",
  "10": "Optimism",
  "42161": "Arbitrum One",
  "137": "Polygon",
  "8453": "Base",
  "43114": "Avalanche",
};
```

#### 3. Database Models & Relationships

**Button Model (buttonModel.ts)**
```typescript
{
  userId: string,           // Creator's Clerk user ID
  name: string,            // Display name for button
  description?: string,    // Optional product description
  amountUsd: number,       // Fixed USD amount
  chainId: string[],       // Supported blockchain networks
  merchantAddress: string, // Merchant's receiving wallet
  transactions: ObjectId[], // Associated transaction refs
  isActive: boolean,       // Button enable/disable status
  timestamps: true         // Auto createdAt/updatedAt
}
```

**Transaction Model (transactionModel.ts)**
```typescript
{
  from: string,            // Customer email address
  to: string,             // Merchant wallet address
  signature?: string,     // Blockchain transaction hash
  chainId: string,        // Blockchain network ID
  time: Date,            // Transaction creation time
  status: "pending" | "success" | "failed",
  buttonId: ObjectId,    // Reference to originating button
  amountUsd: number,     // Original USD amount
}
```

**Profile Model (profileModel.ts)**
```typescript
{
  userId: string,         // Unique Clerk user ID
  email: string,         // User email (unique)
  cryptId: string,       // Custom crypto identifier (unique)
  username: string,      // Display username (unique)
  buttons: ObjectId[]    // Array of created button references
}
```

#### 4. Real-Time Price Integration

**Pyth Network Price Feeds (prices/route.ts)**

```typescript
const PRICE_FEED_IDS = {
  ETH: 'ff61491a931112ddf1bd8147cd1b641375f79f5825126d665480874634fd0ace',
  POL: '5de33a9112c2b700b8d30b8a3402c103578ccfa2765696471cc672bd5cf6ac52',
  AVAX: '93da3352f9f1d105fdfe4971cfa80e9dd777bfc5d0f683ebb6e1294b92137bb7',
  ARB: '3fa4252848f9f0a1480be62745a4629d9eb1322aebab8a791e344b3b9c1adcf5',
  OP: '385f64d993f7b77d8182ed5003d97c60aa3361f3cecfe711544d2d59165e9bdf'
};

// Live price calculation for any USD amount across all chains
GET /api/prices?amount=100&chainId=1
// Returns: { nativeAmount: 0.0234, tokenSymbol: "ETH", price: 4273.45 }
```

#### 5. Email Notification System

**Secure Payment Links (mail.ts)**

```typescript
const payUrl = `${process.env.API_URL}/pay/${transactionId}`;

// HTML email template with:
// - Payment amount and details
// - Secure payment link to pay/[id] page
// - Professional Settel branding
// - Anti-phishing security measures
```

### Transaction Lifecycle

1. **BUTTON_CREATION**
   Merchant → CreateButtonDialog → buttonActions.createButton() → MongoDB

2. **PAYMENT_INITIATION**
   Customer → PaymentButton → Crypto ID → create-transaction API

3. **EMAIL_NOTIFICATION**
   Transaction Created → sendTransactionMail() → Customer Email

4. **PAYMENT_COMPLETION**
   Email Link → pay/[id] → Wallet Connect → Blockchain Transaction

5. **STATUS_TRACKING**
   Real-time Polling → transaction-status/[id] → Status Updates

6. **CONFIRMATION**
   Success → updateTransactionStatus() → Merchant Notification

### API Endpoints

#### Transaction Management

**POST /api/create-transaction**
```typescript
// Creates transaction record and sends email notification
{
  buttonId: string,
  cryptoId: string,
  amountUsd: number,
  currency?: string
}
```

**GET /api/transaction-status/[id]**
```typescript
// Real-time transaction status polling
// Returns: { status: "pending" | "success" | "failed" }
```

#### Price Management

**GET /api/prices?amount={usd}&chainId={chain}**
```typescript
// Multi-chain price calculation using Pyth feeds
// Returns current crypto equivalent for USD amount
```

**GET /api/test-pyth**
```typescript
// Pyth Network connectivity testing endpoint
```

### Security Features

- **Email-First Architecture** - No wallet injections or popups
- **Clerk Authentication** - Secure user session management
- **Unique Crypto IDs** - Custom identifiers linked to emails
- **Real-Time Price Protection** - Pyth Network prevents manipulation
- **Multi-Chain Validation** - Transaction verification across networks
- **Trusted Device Approval** - Payments on user's preferred device

### Technology Stack

#### Core Framework
- **Next.js 15.5.4** (App Router)
- **React 19.1.0**
- **TypeScript 5**
- **Tailwind CSS 4**

#### Blockchain
- **Wagmi 2.16.9** (React hooks)
- **Viem 2.37.4** (Ethereum interface)
- **RainbowKit 2.2.8** (Wallet UI)

#### Database & Auth
- **MongoDB 6.19.0**
- **Mongoose 8.18.1**
- **Clerk** (Authentication)

#### External APIs
- **Pyth Network** (Price feeds)
- **Nodemailer** (Email delivery)

#### UI Components
- **Shadcn UI** (Component library)
- **Radix UI** (Primitive components)
- **Lucide React** (Icons)

## Why This Matters

Business owners want to focus on running their business, not on chasing invoices or wrestling with gateways. They want payments that are direct, reliable, transparent.

Settel gives them exactly that. No GST numbers. No endless approvals. No 3% fees. Just a button, a request, and a payment.

For customers, it means trust. No wallet popups. No phishing risk. No confusion. Just a request they can see, approve, and move on.

It's the simplicity of PayPal, without the cost. The reach of Stripe, without the red tape. And the power of crypto, without the chaos.

## Getting Started

### Prerequisites

- Node.js 18+
- MongoDB database
- Email service credentials (Gmail SMTP)
- Clerk authentication setup
- Pyth Network API access

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/YadlaMani/settel.git
   cd settel
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure environment variables**
   ```bash
   # Create .env.local file with required variables:
   MONGODB_URI=your_mongodb_connection_string
   CLERK_SECRET_KEY=your_clerk_secret_key
   CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
   MAIL_PASS=your_email_password
   API_URL=http://localhost:3000
   ```

4. **Run development server**
   ```bash
   npm dev
   ```

5. **Open in browser**
   ```
   http://localhost:3000
   ```

### Quick Start Guide

1. **Sign up** - Create your Settel account
2. **Connect wallet** - Link your receiving wallet address
3. **Create button** - Set amount, description, and supported chains
4. **Get embed code** - Copy the generated button code
5. **Add to website** - Paste the code wherever you want payments
6. **Start receiving** - Customers can now pay with crypto via email

### Deployment

#### Environment Setup
```bash
# Production build
npm run build

# Start production server
npm start
```

#### Platform Support
- **Vercel** - Optimized for Next.js deployment
- **AWS/Digital Ocean** - Full Node.js server support
- **Docker** - Containerized deployment ready

## The Vision

One button that works everywhere. One dashboard that tells you everything. One place to pay any kind of request.

This is not just another tool. It's a new standard. A standard for how payments should be: simple, transparent, and human.

With Settel, we're not just building a product. We're reimagining commerce itself.

---

**Built with ❤️ for the future of payments**
