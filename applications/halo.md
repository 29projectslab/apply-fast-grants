# 📝 Halo Protocol - Community Finance on Polkadot Asset Hub

## 🌟 Project Overview

**Tagline:** Bringing $4 trillion informal lending circles on-chain through Polkadot Asset Hub

**Brief Description:**
Halo Protocol transforms traditional ROSCAs (Rotating Savings and Credit Associations) into a transparent, automated DeFi primitive on Polkadot Asset Hub. We digitize community-based lending circles used by 1.4 billion people globally, replacing cash-based trust with blockchain transparency while integrating DOT staking and asset management. Members pool monthly contributions, receive lump-sum payouts through rotation, earn yield on idle funds, and build portable on-chain credit scores—all without requiring collateral.

**Integration into Polkadot:**
Halo Protocol leverages Polkadot Asset Hub as the foundation for multi-asset lending circles, enabling:
- **Native DOT Integration**: Users stake DOT as insurance, earn staking rewards, and contribute DOT to circles
- **Asset Hub Multi-Asset Support**: Circles can operate with USDT, USDC, DOT, or other Asset Hub assets
- **XCM Cross-Chain Capabilities**: Future expansion to allow cross-parachain lending circles
- **Nomination Pool Integration**: Automatic DOT staking for insurance pools and idle circle funds
- **Asset Hub as Settlement Layer**: Fast, low-cost transactions for monthly contributions and payouts

**Why This Project:**
After winning multiple hackathons with Halo Protocol on other chains (Solana Superteam, Soonami Venturethon), we recognized Polkadot's unique advantages for community finance:
1. **Asset Hub's multi-asset support** enables diverse circle types (DOT circles, stablecoin circles, hybrid)
2. **Nomination pools** provide sustainable yield without complex DeFi integrations
3. **Low transaction costs** make monthly micro-contributions economically viable
4. **Polkadot's governance culture** aligns with community-driven financial inclusion
5. **Growing Polkadot India community** provides initial user base for testing and adoption

As a Polkadot Fast Grants alumni with 6+ months of Substrate development experience, I'm deeply invested in the Polkadot ecosystem and see this as an opportunity to bring 1.4 billion unbanked users on-chain through familiar financial primitives.

**Video Pitch:** [Will be added upon request - 1-minute demo available]

### 🔍 Project Details

**Technology Stack:**

**Smart Contracts (Polkadot Asset Hub):**
- **Substrate Runtime**: Custom pallets deployed on Asset Hub
- **Ink! Smart Contracts**: For complex circle logic and trust scoring
- **Asset Hub Assets Pallet**: For multi-asset circle support (DOT, USDT, USDC)
- **Nomination Pools**: Integration for automated DOT staking
- **XCM**: For future cross-parachain functionality

**Frontend:**
- **Next.js 14** with TypeScript
- **Polkadot.js API / Polkadot API (PAPI)**: For blockchain interactions
- **SubWallet / Talisman Integration**: For wallet connections
- **TailwindCSS**: For responsive mobile-first design
- **Vercel**: For deployment

**Infrastructure:**
- **Paseo Testnet**: For closed beta testing
- **IPFS**: For metadata and documentation storage
- **The Graph / Subsquid**: For indexing circle data and user analytics

---

**Core Components & Architecture:**

**1. Circle Management System**
```rust
// Pseudo-code for Circle Pallet
pub struct Circle {
    id: CircleId,
    creator: AccountId,
    members: Vec<AccountId>,
    contribution_amount: Balance,
    contribution_asset: AssetId, // DOT, USDT, USDC
    cycle_duration: BlockNumber,
    payout_schedule: PayoutType, // Rotation, Auction, Random
    insurance_required: Balance,
    status: CircleStatus,
    total_collected: Balance,
    current_round: u32,
}

impl Circle {
    fn create_circle(...) -> Result<CircleId>;
    fn join_circle(member: AccountId) -> Result<()>;
    fn contribute(amount: Balance) -> Result<()>;
    fn distribute_payout() -> Result<()>;
    fn stake_insurance(amount: Balance) -> Result<()>;
}
```

**2. Trust Score System**
```rust
pub struct TrustScore {
    user: AccountId,
    score: u32, // 0-1000
    payment_reliability: u32, // 40% weight
    circle_completions: u32, // 30% weight
    staking_history: u32, // 20% weight
    social_verification: u32, // 10% weight
    tier: TrustTier, // Newcomer, Silver, Gold, Platinum
}

pub enum TrustTier {
    Newcomer,  // 0-249: Max 100 DOT circles
    Silver,    // 250-499: Max 500 DOT circles
    Gold,      // 500-749: Max 2000 DOT circles
    Platinum,  // 750-1000: Unlimited, governance rights
}
```

**3. DOT Staking Integration**
```rust
pub struct InsurancePool {
    circle_id: CircleId,
    total_staked: Balance,
    nomination_pool_id: PoolId,
    staking_rewards: Balance,
    members: Vec<(AccountId, Balance)>,
}

impl InsurancePool {
    fn stake_dot(amount: Balance) -> Result<()>;
    fn nominate_to_pool() -> Result<()>;
    fn claim_rewards() -> Result<Balance>;
    fn distribute_rewards_on_completion() -> Result<()>;
}
```

**4. Multi-Asset Support (Asset Hub)**
- Integration with Asset Hub's Assets pallet
- Support for DOT (native), USDT, USDC (Asset Hub assets)
- Asset conversion logic for cross-asset contributions
- Fee handling per asset type

---

**PoC/MVP:**
- ✅ **Halo Protocol deployed on Solana** (https://haloprotocol.fun)
- ✅ **Smart contract architecture** validated on Base testnet
- ✅ **Trust score algorithm** designed and tested
- ✅ **Won Soonami Venturethon EIR Track** - validated product-market fit

---

**Mockups/UI Components:**

**Dashboard:**
- Circle overview cards (active, pending, completed)
- Trust score gauge with tier progression
- Monthly contribution calendar
- Payout schedule timeline
- DOT staking rewards tracker

**Circle Creation Flow:**
1. Choose asset type (DOT, USDT, USDC)
2. Set contribution amount and duration
3. Select payout method (rotation/auction)
4. Invite members (max based on trust tier)
5. Stake insurance (10-20% of contribution)

**Circle Details Page:**
- Member list with trust scores
- Contribution status per member
- Payout history and next recipient
- Yield/staking earnings
- Circle chat/notifications

---

**Data Models:**

**Circle Entity:**
```typescript
interface Circle {
  id: string;
  creator: string;
  members: Member[];
  contributionAmount: number;
  contributionAsset: 'DOT' | 'USDT' | 'USDC';
  cycleDuration: number; // in blocks
  payoutType: 'rotation' | 'auction' | 'random';
  insuranceRequired: number;
  status: 'active' | 'pending' | 'completed' | 'defaulted';
  currentRound: number;
  totalRounds: number;
  createdAt: number;
}
```

**User Entity:**
```typescript
interface User {
  address: string;
  trustScore: number;
  trustTier: 'newcomer' | 'silver' | 'gold' | 'platinum';
  circlesJoined: number;
  circlesCompleted: number;
  totalContributed: number;
  onTimePaymentRate: number;
  insuranceStaked: number;
  stakingRewards: number;
}
```

---

**API Specifications:**

**Core Functions:**
- `createCircle(params: CircleParams): Promise<CircleId>`
- `joinCircle(circleId: string): Promise<void>`
- `contribute(circleId: string, amount: number): Promise<void>`
- `claimPayout(circleId: string): Promise<void>`
- `stakeInsurance(circleId: string, amount: number): Promise<void>`
- `calculateTrustScore(address: string): Promise<TrustScore>`
- `nominateToPool(amount: number): Promise<void>`

---

**What Halo Protocol is NOT:**

❌ **Not a credit card or payment solution**: We facilitate savings/lending circles, not everyday transactions
❌ **Not launching within 3 months on mainnet**: This grant covers testnet MVP; mainnet requires security audit and further funding

**Limitations:**
- **Testnet only** for grant period (Paseo)
- **No mobile app** (PWA only)
- **Limited to Asset Hub assets** (DOT, USDT, USDC - no custom parachains initially)
- **Manual circle matching** (no AI-powered matching in MVP)
- **No fiat on/off ramps** (crypto-only currently)
- **English language only** initially (LATAM/Africa expansion post-grant)

---

### 🧩 Ecosystem Fit

**Where does Halo Protocol fit in the Polkadot ecosystem?**

Halo Protocol fills a critical gap in Polkadot's DeFi landscape: **uncollateralized, community-based lending**. While Polkadot has excellent infrastructure for collateralized DeFi (Acala, Parallel, Hydration), there's no protocol enabling social lending or community finance. We position at the intersection of:

1. **Asset Hub DeFi Layer**: First major DeFi application leveraging Asset Hub's multi-asset capabilities
2. **Financial Inclusion**: Bringing 1.4B unbanked users to Polkadot through familiar primitives (ROSCAs)
3. **DOT Utility**: New use case for DOT beyond staking - insurance staking, circle contributions
4. **User Onboarding**: Gateway for non-crypto natives to enter Polkadot ecosystem with simple UX

**Target Audience:**

**Primary (Launch - Month 3):**
- **Polkadot community members** in India, Philippines, Southeast Asia
- **Crypto-native users** looking for uncollateralized borrowing
- **Existing ROSCA participants** (2-3M active users in target regions)

**Secondary (Post-Grant):**
- **Underbanked populations** in LATAM, Africa, Southeast Asia
- **Web3 newcomers** seeking familiar financial entry points
- **DAOs and communities** for treasury diversification

**Needs Met:**

1. **Access to Capital Without Collateral**: Unlike Acala/Parallel requiring 150%+ collateral, Halo enables lump-sum access through rotating payouts
2. **Transparent ROSCAs**: Replaces opaque cash-based lending circles with on-chain transparency
3. **Yield on Savings**: Idle circle funds earn DOT staking rewards (7-12% APY) vs 0% in traditional ROSCAs
4. **Portable Credit Score**: On-chain trust score transferable across Polkadot ecosystem
5. **DOT Utility**: New productive use case beyond staking - insurance, contributions, governance

**Similar Projects in Polkadot?**

**Direct Competitors: None**

There are NO direct competitors building community finance/ROSCAs on Polkadot. The closest adjacent projects are:

- **Acala/Parallel Finance**: Collateralized lending (requires 150%+ collateral, very different model)
- **Polkadot Bounties**: One-time funding, not recurring lending circles
- **Subsocial/Polkaverse**: Social graphs but no financial primitives

**Why doesn't this exist yet?**

1. **Asset Hub is relatively new**: Multi-asset capabilities recently enabled make this viable
2. **ROSCAs not well-understood** in crypto: Most builders focus on collateralized DeFi or NFTs
3. **Complexity**: Requires combining smart contracts, staking, multi-asset support, and social mechanics
4. **Non-obvious product-market fit**: Requires deep understanding of informal finance and target demographics

**How is Halo Different?**

- **First mover** in community finance on Polkadot
- **Asset Hub native**: Built specifically for Asset Hub's multi-asset infrastructure
- **Financial inclusion focus**: Explicitly targeting underbanked vs crypto-native DeFi power users
- **Trust as collateral**: Pioneering reputation-based lending on Polkadot

---

## 👥 Team

- **Team Name:** Halo Protocol
- **Contact Name:** Kunal
- **Contact Email:** kunal@haloprotocol.fun
- **Website:** https://haloprotocol.fun

### Team members

**Solo Developer:**
- Kunal 

#### LinkedIn Profiles
- https://www.linkedin.com/in/kunaldrall

### Team Code Repos

**Halo Protocol:**
- https://github.com/haloprotocol (organization - will be created for Polkadot version)
- Existing work: https://haloprotocol.fun , https://github.com/kunal-drall/halo(Solana implementation)

**Personal GitHub:**
- https://github.com/kunal-drall

### Team's experience

**Kunal** is a Full Stack Developer with 6+ months of active Substrate and Polkadot development experience.

**Relevant Blockchain Experience:**
- 🏆 **2nd Runner-Up, ISA Solarthon @ UN COP'24** - Blockchain-based Solar Forecasting System (https://isa.int/solarthon)
- 🏆 **Winner, Soonami Venturethon - EIR Track** - Halo Protocol recognized for product-market fit
- 🏆 **Winner, Solana Superteam Track** - Halo Protocol DeFi implementation
- 🏆 **Multiple Web3 Hackathon Bounty Wins** - Across Solana, EVM, Sui ecosystems
- 💻 **2+ years Web3 development** - Full-stack dApp development with DeFi protocol integrations
- 🔗 **Active Polkadot India contributor** - Regular participant in governance and community events

**Technical Skills:**
- **Substrate/Polkadot**: FRAME pallets, Ink! smart contracts, XCM, Nomination pools
- **Frontend**: React, Next.js, TypeScript, Polkadot.js API, SubWallet integration
- **DeFi Protocols**: Lending/borrowing mechanics, AMM integrations, yield strategies
- **Smart Contracts**: Solidity (EVM), Anchor (Solana), Ink! (Polkadot), Move (Aptos)



---

## 📊 Development Status

**Current Status:**

✅ **Halo Protocol v1.0 deployed on Solana/Base Testnet**: Live at https://haloprotocol.fun with documented smart contract architecture

✅ **Product-market fit validated**: 
- Won Soonami Venturethon EIR Track (product validation)
- Won Solana Superteam Track (technical validation)
- User research: 92% want digital ROSCA alternative, 88% willing to pay fees

✅ **Technical architecture designed**: Smart contract specifications, trust score algorithm, yield integration logic all documented

✅ **Landing page live**: https://haloprotocol.fun (currently shows Solana version)

**Research & Preparation:**

📚 **Polkadot Asset Hub research**: Studied Assets pallet, nomination pools, XCM capabilities for circle integration

📚 **Competitive analysis**: Analyzed existing Polkadot DeFi protocols to identify uncollateralized lending gap

📚 **Target market validation**: Interviewed 30+ traditional ROSCA participants in India and Philippines to validate digital migration

📚 **Tokenomics design**: Designed insurance staking mechanism compatible with DOT nomination pools

**Code Repositories:**

While the Polkadot version is not yet started (this grant will fund it), you can review:
- **Architecture principles** from Solana implementation at haloprotocol.fun
- **Trust score algorithm** (blockchain-agnostic logic)
- **Frontend mockups** and UX flow

Upon grant approval, all code will be open-sourced under Apache 2.0 license on GitHub.

---

## 📅 Development Roadmap

### Overview

- **Estimated Duration:** 3 months
- **Full-Time Equivalent (FTE):** 0.8 FTE (solo developer, ~32 hours/week)
- **Total Costs:** $10,000 USD

### Milestone 1: Smart Contracts & Core Logic

**Duration:** 1.5 months  
**FTE:** 0.8  
**Cost:** $5,000 USD

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| 0a. | License | Apache 2.0 |
| 0b. | Documentation | We will provide comprehensive **inline documentation** of all smart contract code (Ink!), plus a **developer tutorial** explaining: (1) How to deploy contracts to Paseo, (2) How circle creation and contribution flow works, (3) How trust score calculation is performed, (4) How insurance staking integrates with nomination pools. Documentation will be hosted on GitHub and included in README. |
| 0c. | Testing and Testing Guide | Core smart contract functions will be fully covered by comprehensive unit tests (>80% coverage) using `ink_e2e` testing framework. We will provide a testing guide explaining: (1) How to run unit tests locally, (2) How to run integration tests on Paseo, (3) Test scenarios covering circle lifecycle (creation, contribution, payout, completion, default). All tests will be runnable via `cargo test`. |
| 0d. | Article | We will publish a **Medium article** (or Polkadot Forum post) titled "Building Community Finance on Polkadot Asset Hub: How Halo Protocol Brings ROSCAs On-Chain" explaining: (1) What ROSCAs are and why they matter ($4T market), (2) Technical architecture on Asset Hub, (3) How DOT staking powers insurance, (4) Vision for financial inclusion on Polkadot. Article will include code snippets and architecture diagrams. |
| 1. | Circle Management Contract | We will create an **Ink! smart contract** (`circle_manager.rs`) deployed on Paseo Asset Hub testnet that handles: (1) Circle creation with configurable parameters (contribution amount, asset type [DOT/USDT/USDC], duration, member limit, payout type), (2) Member joining with trust tier validation, (3) Circle state management (active/pending/completed/defaulted), (4) Access control (only creator can modify settings, only members can contribute). Contract will emit events for all state changes for frontend indexing. |
| 2. | Contribution & Payout Logic | We will implement **contribution collection** and **payout distribution** functions: (1) `contribute()` - accepts DOT/USDT/USDC, validates amount matches circle requirements, updates member contribution status, locks funds in escrow, (2) `distribute_payout()` - executes monthly payout based on rotation/auction schedule, transfers funds to recipient, updates circle round counter, validates all members contributed before payout. Includes fee collection mechanism (0.5% distribution fee). All transactions are atomic to prevent partial executions. |
| 3. | Trust Score System | We will create a **trust scoring contract** (`trust_score.rs`) that: (1) Calculates scores 0-1000 based on weighted factors (40% payment reliability, 30% circle completions, 20% staking history, 10% social verification), (2) Updates scores automatically after each contribution (on-time vs late), (3) Determines trust tiers (Newcomer/Silver/Gold/Platinum), (4) Enforces tier-based limits (Newcomer max 100 DOT circles, Platinum unlimited), (5) Provides score history and improvement recommendations. Score data is queryable via storage mappings. |
| 4. | Insurance & DOT Staking Integration | We will implement **insurance pool management** with DOT staking: (1) `stake_insurance()` - requires members to stake 10-20% of contribution as insurance in DOT, (2) Integration with **Asset Hub Nomination Pools** - automatically nominates pooled insurance to earn staking rewards (~10% APY), (3) `claim_insurance()` - allows claiming insurance funds if member defaults (requires >50% circle vote), (4) `return_insurance()` - returns insurance + proportional staking rewards to members upon successful circle completion. Implements slashing mechanism for defaulters. |
| 5. | Multi-Asset Support (Asset Hub) | We will integrate **Asset Hub Assets pallet** to enable circles with different assets: (1) Support for DOT (native token), (2) Support for USDT (Asset ID on Asset Hub), (3) Support for USDC (Asset ID on Asset Hub), (4) Asset validation logic ensuring contribution matches circle's configured asset, (5) Fee collection per asset type, (6) Asset metadata storage (decimals, symbol). This enables users to create DOT circles, stablecoin circles, or hybrid circles based on preference. |

**Testing Coverage:**
- Unit tests for all contract functions (>80% coverage)
- Integration tests for complete circle lifecycle
- Edge case testing (defaults, late payments, insufficient balance)
- Gas optimization tests to keep transaction costs minimal

**Verification:**
- Deploy contracts to Paseo Asset Hub testnet
- Provide contract addresses and verification scripts
- Run test suite demonstrating all functionality
- Provide Polkadot.js Apps links to interact with contracts

**Budget Breakdown:**
- Smart contract development (Ink!): 120 hours @ $35/hour = $4,200
- Testing & documentation: 20 hours @ $35/hour = $700
- Deployment & debugging: 3 hours @ $35/hour = $105
- **Total: $5,000**

---

### Milestone 2: Frontend & User Experience

**Duration:** 1 month  
**FTE:** 0.8  
**Cost:** $3,500 USD

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| 0a. | License | Apache 2.0 |
| 0b. | Documentation | We will provide: (1) **User guide** with screenshots explaining how to create circles, join circles, contribute, and track trust scores, (2) **Developer documentation** for frontend architecture and component structure, (3) **Wallet integration guide** for SubWallet/Talisman setup on Paseo. Documentation will be hosted on GitHub wiki. |
| 0c. | Testing and Testing Guide | Frontend will include: (1) Unit tests for React components using Jest/React Testing Library, (2) E2E tests for critical user flows (circle creation, contribution, payout claiming) using Playwright, (3) Wallet connection tests with mock providers. Testing guide will explain how to run test suites locally and in CI/CD. |
| 0d. | Article | We will publish a **user-focused article** (Medium/Polkadot Forum) titled "How to Join Your First Lending Circle on Polkadot" featuring: (1) Step-by-step walkthrough with screenshots, (2) Trust score explanation for non-technical users, (3) Benefits of DOT circles vs traditional ROSCAs, (4) FAQ section addressing common questions. Article targets onboarding new users to Polkadot via Halo. |
| 1. | Landing Page & Onboarding | We will create a **mobile-first landing page** (Next.js 14) that: (1) Explains ROSCAs simply with visual animations, (2) Shows $4T market opportunity and 1.4B target users, (3) Highlights benefits (no collateral, earn yield, build credit), (4) Wallet connection flow (SubWallet/Talisman), (5) Paseo testnet faucet instructions, (6) Call-to-action buttons (Create Circle / Join Circle / Join Waitlist). Landing will include trust score visualization and live circle statistics from testnet. Design will be accessible, fast-loading, and conversion-optimized. |
| 2. | Circle Dashboard | We will build a **comprehensive circle dashboard** showing: (1) **Active Circles** - cards displaying circle status, next payment date, next payout recipient, yield earned, (2) **Pending Invites** - circles user can join based on trust tier, (3) **Completed Circles** - history with total contributed/received/yield earned, (4) **Circle Details Modal** - deep dive into member list, contribution timeline, payout schedule, chat/notifications, (5) Real-time updates via WebSocket or polling. Dashboard will be responsive (mobile/desktop) with intuitive navigation. |
| 3. | Circle Creation & Joining Flow | We will implement **user flows** for: (1) **Create Circle** - multi-step form (choose asset [DOT/USDT/USDC], set contribution amount and duration, select payout type [rotation/auction], invite members via shareable link, review and confirm with insurance stake), (2) **Join Circle** - discover circles via invite link or browse public circles, view circle details (members, trust score requirements, commitment), confirm joining with insurance stake, (3) **Validation logic** - checks trust tier limits, insurance requirements, asset balance before transaction. Includes transaction confirmation modals with gas estimates. |
| 4. | Contribution & Payout Interface | We will create **contribution management** features: (1) **Contribution Modal** - displays amount due, deadline, circle details, one-click payment with SubWallet/Talisman signature, (2) **Payment History** - timeline showing past contributions, on-time vs late status, (3) **Payout Claiming** - notification when user is next payout recipient, claim button with automatic distribution, (4) **Yield Tracking** - real-time display of staking rewards earned from insurance pool and idle circle funds, claimable yield interface. All transactions show confirmation status and block explorer links. |
| 5. | Trust Score & Profile Page | We will build a **trust score center** featuring: (1) **Large trust score gauge** (0-1000) with tier badge (Newcomer/Silver/Gold/Platinum), (2) **Score breakdown pie chart** showing 40% payment reliability, 30% completions, 20% staking, 10% social, (3) **Tier benefits comparison** - what unlocks at each level (circle limits, governance rights, fee discounts), (4) **Improvement tips** - actionable suggestions to increase score, (5) **Score history line chart** tracking progress over time, (6) **Profile settings** - link social proofs (Twitter, GitHub) for social verification component. This page gamifies reputation building. |
| 6. | Wallet Integration & Polkadot.js API | We will integrate: (1) **SubWallet SDK** - primary wallet with seamless Paseo connection, (2) **Talisman wallet support** - alternative option for user choice, (3) **Polkadot.js API / PAPI** - robust blockchain interaction layer for reading state, submitting extrinsics, listening to events, (4) **Account management** - display DOT/USDT/USDC balances, show insurance stakes, track pending transactions, (5) **Network handling** - automatic Paseo testnet detection, network switching prompts if on wrong network, testnet faucet links for new users. Connection will be smooth with error handling. |

**Design Specifications:**
- Mobile-first responsive design (mobile, tablet, desktop)
- Accessibility (WCAG 2.1 AA compliance)
- Fast loading (<2s initial load on 3G)
- TailwindCSS for consistent styling
- Dark mode support

**Verification:**
- Deploy frontend to Vercel with custom domain
- Provide live demo link connected to Paseo testnet
- E2E test suite demonstrating all user flows
- Screen recordings showing mobile and desktop UX

**Budget Breakdown:**
- Frontend development (React/Next.js): 85 hours @ $35/hour = $2,975
- Wallet integration & testing: 12 hours @ $35/hour = $420
- UI/UX polish & documentation: 3 hours @ $35/hour = $105
- **Total: $3,500**

---

### Milestone 3: Closed Beta & Waitlist Campaign

**Duration:** 0.5 months (2 weeks)  
**FTE:** 0.8  
**Cost:** $1,500 USD

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| 0a. | License | Apache 2.0 |
| 0b. | Documentation | We will provide: (1) **Beta tester onboarding guide** with Paseo testnet setup instructions, (2) **Feedback collection template** for structured user input, (3) **Bug reporting process** via GitHub issues, (4) **FAQ document** addressing common testnet questions. Documentation will be accessible to non-technical users. |
| 0c. | Testing and Testing Guide | We will: (1) Conduct **closed beta** with 5 selected testers forming 1 complete lending circle on Paseo, (2) Monitor circle through full lifecycle (10 contributions, 10 payouts, completion with insurance return), (3) Collect qualitative feedback on UX, pain points, feature requests, (4) Track quantitative metrics (contribution success rate, transaction times, yield earned, trust score progression). Beta test results will be documented in a report. |
| 0d. | Article | We will publish a **beta results article** (Medium/Polkadot Forum) titled "Halo Protocol Closed Beta Results: Bringing ROSCAs to Polkadot Asset Hub" covering: (1) Beta tester demographics and feedback, (2) Technical learnings and improvements made, (3) Quantitative results (on-time payments, yield earned, trust scores), (4) Roadmap for mainnet launch post-grant, (5) Call-to-action for waitlist signups. Article will build credibility and attract early adopters. |
| 1. | Closed Beta Testing Program | We will execute a **5-tester closed beta** on Paseo: (1) Recruit 5 testers from Polkadot India community (mix of crypto-experienced and newcomers), (2) Onboard testers with testnet DOT from faucet, (3) Guide setup of SubWallet/Talisman on Paseo, (4) Form 1 lending circle with $100 equivalent DOT monthly contributions over 5 months (compressed timeline for testing), (5) Monitor contributions, payouts, and insurance staking throughout cycle, (6) Collect feedback via weekly surveys and exit interview. Beta will validate product-market fit and identify bugs before broader launch. |
| 2. | Waitlist Landing Page & Campaign | We will build a **high-conversion waitlist page** with: (1) Compelling hero section ("Join the future of community finance on Polkadot"), (2) Email signup form with name, email, country, ROSCA experience fields, (3) Social proof (beta tester testimonials, stats from Solana alpha), (4) FAQ section addressing key concerns, (5) Social sharing incentives (referral tracking), (6) Integrated with email service (ConvertKit/Mailchimp) for automated follow-ups. Design will be optimized for mobile and conversion. **Goal: 150+ signups** demonstrating demand. |
| 3. | Community Outreach & Marketing | We will execute **targeted marketing campaign**: (1) Post in Polkadot Forum introducing Halo Protocol, (2) Share beta results in Polkadot India Telegram/Discord, (3) Create Twitter thread explaining ROSCAs and Polkadot benefits, (4) Submit to Polkadot newsletter and ecosystem updates, (5) Engage with potential users in Web3 finance communities (Reddit, Telegram), (6) Leverage existing Halo Protocol network from Solana launch. Campaign will drive waitlist signups and validate market interest in Polkadot community. |
| 4. | Bug Fixes & UX Improvements | Based on closed beta feedback, we will: (1) Fix critical bugs identified during testing, (2) Implement UX improvements suggested by testers (e.g., clearer onboarding, better error messages), (3) Optimize transaction flows for lower gas costs, (4) Improve mobile responsiveness based on real device testing, (5) Enhance trust score transparency with detailed explanations. All improvements will be documented in GitHub changelog. |
| 5. | Final Deliverables & Handoff | We will provide: (1) **Complete open-source codebase** on GitHub (smart contracts + frontend) under Apache 2.0 license, (2) **Comprehensive README** with setup instructions, architecture overview, and contribution guidelines, (3) **Video demo** (5-minute walkthrough of creating and joining circles on Paseo), (4) **Beta test report** with metrics, feedback summary, and learnings, (5) **Waitlist data** (150+ signups with demographics), (6) **Roadmap document** for mainnet launch and post-grant development, (7) **Deployed testnet** (contracts on Paseo + frontend on Vercel) accessible for community testing. |

**Success Metrics:**
- ✅ 5 beta testers successfully complete 1 full lending circle (10 contributions each)
- ✅ 100% on-time contribution rate during beta
- ✅ 150+ waitlist signups from Polkadot community
- ✅ 90%+ positive feedback from beta testers (would recommend to others)
- ✅ Zero critical bugs reported during beta
- ✅ Average trust score increase of 100+ points for beta testers

**Verification:**
- Public GitHub repository with all code
- Live Paseo testnet deployment with contract addresses
- Waitlist landing page with signup counter
- Beta test report with anonymized feedback
- Video demo on YouTube/Twitter
- Article published on Medium/Polkadot Forum

**Budget Breakdown:**
- Beta tester recruitment & support: 10 hours @ $35/hour = $350
- Waitlist page & marketing: 15 hours @ $35/hour = $525
- Bug fixes & improvements: 12 hours @ $35/hour = $420
- Documentation & final deliverables: 6 hours @ $35/hour = $210
- **Total: $1,505** (rounded to $1,500)

---

### 💰 Budget Breakdown Summary

| Milestone | Deliverables | Cost (USD) | Estimated Completion | Hours | Rate |
| --- | --- | --- | --- | --- | --- |
| 1 | Smart Contracts (Ink!), Circle Management, Trust Score, Insurance Staking, Multi-Asset Support | $5,000 | 1.5 months | 143 hours | $35/hour |
| 2 | Frontend (Next.js), Dashboard, Wallet Integration, Trust Score UI, Mobile-First Design | $3,500 | 1 month | 100 hours | $35/hour |
| 3 | Closed Beta (5 testers), Waitlist Campaign (150+ signups), Bug Fixes, Final Deliverables | $1,500 | 2 weeks | 43 hours | $35/hour |
| **Total** | | **$10,000** | **3 months** | **286 hours** | **$35/hour** |

**Funding Justification:**
- **Solo developer rate**: $35/hour is below market rate for senior blockchain developers ($80-150/hour), reflecting grant funding nature and passion for Polkadot ecosystem
- **Time commitment**: 0.8 FTE = 32 hours/week average over 3 months = 384 hours available; 286 billable hours = 74% efficiency (accounts for meetings, admin, unexpected delays)
- **Cost efficiency**: Building on proven architecture from Solana version reduces development time; no team coordination overhead

---

## 🔮 Future Plans

**Post-Fast-Grant Development (Months 4-6):**

1. **Security Audit**: Partner with Polkadot security firms (SRLabs, Quarkslab) for smart contract audit before mainnet launch (estimated $15-25K)

2. **Mainnet Deployment**: Launch on Polkadot Asset Hub mainnet after audit completion

3. **User Acquisition**: Scale from 5 beta testers to 500+ mainnet users through:
   - Polkadot India community partnerships
   - Influencer marketing in target regions (Philippines, Indonesia)
   - Integration with Polkadot wallets (SubWallet, Talisman featured app)

4. **Feature Expansion**:
   - Auction-based payout system (bid for early access)
   - Social verification integration (Twitter, GitHub, on-chain activity)
   - Circle chat/communication features
   - Advanced analytics dashboard

**Funding Strategy:**

1. **Additional Polkadot Grants**:
   - Apply for **Web3 Foundation General Grants** (Tier 2: $30K-100K) for mainnet launch and security audit
   - Seek **Polkadot Treasury Proposal** for user acquisition campaigns (targeting $50K)

2. **VC Funding**:
   - Raise $500K-1M seed round post-mainnet launch targeting:
     - Polkadot-focused VCs (Hypersphere, D1 Ventures)
     - Financial inclusion VCs (Accion Venture Lab)
     - Regional VCs in target markets (LATAM, Southeast Asia)

3. **Revenue Generation**:
   - 0.5% distribution fee + 0.25% yield fee = sustainable revenue model
   - Break-even at $50M TVL / 10K users (realistic 18-24 months post-mainnet)
   - Premium features (analytics, custom circles) for power users

**Ecosystem Growth Vision:**

**6 Months Post-Grant:**
- 1,000+ users on Polkadot Asset Hub
- $1M+ TVL contributing to Asset Hub ecosystem
- 100+ active lending circles with >95% on-time payment rate
- Trust scores accepted by other Polkadot DeFi protocols (Acala, Parallel) for uncollateralized features

**12 Months Post-Grant:**
- 10,000+ users across Polkadot ecosystem
- $10M+ TVL making Asset Hub a DeFi hub
- Cross-parachain lending circles using XCM (circles spanning Acala + Asset Hub)
- Halo trust scores become standard reputation primitive across Polkadot
- Mobile app launch (iOS/Android) with multi-language support (Spanish, Tagalog, Bahasa)

**Long-Term Impact:**
- **Financial Inclusion**: Bring 100K+ unbanked users to Polkadot within 3 years
- **DOT Utility**: Create new productive use case for DOT (insurance staking, contributions) driving demand
- **DeFi Innovation**: Pioneer reputation-as-collateral model for Polkadot DeFi
- **Ecosystem Onboarding**: Serve as gateway for non-crypto natives to discover Polkadot through familiar financial primitives

**Sustainability Model:**
Halo Protocol is designed for long-term sustainability through:
- Fee revenue (target $100K/year at 10K users)
- Premium features ($50K-100K/year)
- B2B partnerships (treasury management for DAOs)
- Community governance via trust score system (future DAO structure)

---

## ℹ️ Additional Information

**Work Already Completed:**


✅ **Market Research**: Surveyed 30+ traditional ROSCA participants in India and Philippines, validating demand for digital alternative:
- 92% want transparent digital ROSCA solution
- 88% willing to pay 0.5% fee for insurance protection
- 78% concerned about defaults in traditional ROSCAs
- 67% interested in earning yield on contributions

✅ **Product Awards & Recognition**:
- 🏆 Winner, Soonami Venturethon EIR Track (product validation)
- 🏆 Winner, Solana Superteam Track (technical excellence)
- 🏆 2nd Runner-Up, ISA Solarthon @ UN COP'24 (blockchain innovation)

✅ **Technical Specifications Designed**: Complete smart contract architecture, trust score algorithm, frontend mockups documented and ready for Polkadot adaptation


**Other Funding:**

- ✅ **No other funding applied for Halo Protocol on Polkadot** (this is the first Polkadot-specific grant)
- ✅ **Previous funding**: Self-funded Solana development (~$8K personal investment in v1.0)
- ✅ **Polkadot Fast Grants Alumni**: Successfully completed previous Fast Grant (different project)
- 🔄 **Future funding**: Will apply for Web3 Foundation General Grants (Tier 2) post-Fast-Grant for mainnet launch

**Why Halo Protocol is a Great Fit for Fast-Grants:**

1. **Fast**: Proven architecture from Solana version accelerates development; can deliver testnet MVP in 3 months
2. **Retroactive**: Leveraging lessons from 50-user alpha; not starting from scratch
3. **Milestone-based**: Clear, measurable deliverables (contracts, frontend, beta testing)
4. **Low-risk**: Solo developer with proven track record (Fast Grants alumni + multiple hackathon wins)
5. **High-growth potential**: Targets $4T market with 1.4B users; can scale significantly post-grant
6. **Ecosystem fit**: Builds on Asset Hub (priority for Fast-Grants) and brings new users to Polkadot

**Community Commitment:**

Upon completion of this grant, I commit to:
- ✅ **Open-sourcing all code** under Apache 2.0 license for ecosystem benefit
- ✅ **Maintaining Paseo testnet deployment** for 6+ months post-grant for community testing
- ✅ **Supporting developers** who want to fork/adapt Halo Protocol for other use cases
- ✅ **Contributing to Polkadot India community** through workshops and documentation
- ✅ **Pursuing mainnet launch** with additional funding to deliver long-term value

**Differentiation from Solana Version:**

While Halo Protocol launched on Solana first, the Polkadot version offers unique advantages:
- **Asset Hub multi-asset support**: DOT + stablecoins in same circles (vs USDC-only on Solana)
- **Nomination pool integration**: Sustainable yield via DOT staking (vs relying on Solend liquidity)
- **Governance alignment**: Polkadot's governance culture matches community-finance ethos better
- **Emerging ecosystem**: Opportunity to become *the* community finance protocol on Polkadot vs competing on saturated Solana

**Long-Term Polkadot Commitment:**

This grant is **not a one-off experiment**. I'm committed to:
- Building Halo Protocol's primary deployment on Polkadot Asset Hub
- Becoming a leading DeFi application in the Polkadot ecosystem
- Onboarding non-crypto users to Polkadot through financial inclusion
- Contributing back to the ecosystem through documentation, tools, and community support

---

**Thank you for considering Halo Protocol for the Polkadot Fast-Grants Programme. We're excited to bring community finance to Polkadot Asset Hub and serve as a gateway for 1.4 billion unbanked individuals to discover the power of decentralized finance.**

**Ready to bring $4T on-chain. Ready to build on Polkadot. 🚀**
