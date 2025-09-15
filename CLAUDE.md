# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a repository for a single-page professional website for Angel's side business offering landing page services for CrossFit/fitness businesses and professional networking services. The project is currently in planning phase with a comprehensive PRD document.

## Technical Stack

- **Frontend**: Astro (preferred) or similar static site generator
- **CI/CD**: GitHub Actions → GitHub Pages / Netlify / Vercel
- **Forms**: Netlify forms or webhook → Google Sheets / Zapier
- **Integrations**: WhatsApp click-to-chat, Calendly (optional), GA/Meta Pixel
- **Hosting**: Customer-provided domain or agreed provider

## Core Business Model

- **Primary Services**: Landing pages for CrossFit boxes, boutique gyms, personal trainers (Bogotá)
- **Secondary Services**: SMB landing pages; network services for CTOs/IT managers
- **Pricing**: Basic ($200), Standard ($350), Premium ($600)
- **Delivery**: 48-72h demo turnaround

## Content Guidelines

When working on copy or content:
- **Tone**: Concise, professional, minimalist + subtle geeky CLI vibe
- **Language**: Default English; Spanish (Colombian) on request
- **Format**: Markdown preferred
- **Length Limits**: 
  - Hero headline ≤10 words
  - Subhead ≤18 words
  - Service bullets ≤20 words
  - Paragraphs ≤2 sentences

## Required Client Assets

When implementing intake forms or checklists, ensure collection of:
- Logo (SVG/PNG)
- Primary photo (hero)
- Short bio (1-2 lines)
- Service text (bullet points)
- WhatsApp number
- Domain access or DNS instructions

## Deployment Process

Standard delivery process for client projects:
1. Intake form (client provides assets + domain + WhatsApp)
2. Payment & agreement (50% upfront for Standard/Premium)
3. Demo on subdomain in 48-72 hrs
4. 1-2 rounds revisions
5. Final deploy + 7-30 days support (per package)

## Key Integrations

- WhatsApp click-to-chat functionality (primary CTA)
- Calendly booking (Standard/Premium packages)
- Google Analytics / Meta Pixel tracking
- Form handling (Netlify forms or webhook integration)

## Legal Considerations

- Include Habeas Data compliance for Colombian clients
- Privacy statements for WhatsApp usage
- Basic legal templates for Premium package clients

## Development Commands

- `npm run dev` - Start development server (http://localhost:4321)
- `npm run build` - Build for production
- `npm run preview` - Preview production build locally
- `npm run astro` - Run Astro CLI commands

## Project Structure

- `src/pages/` - Page components (file-based routing)
- `src/components/` - Reusable Astro components
- `public/` - Static assets
- `astro.config.mjs` - Astro configuration


# CCNA / CompTIA Network+ / JNCIA-Junos Practice Tool - Static Implementation

## Project Overview

A comprehensive networking certification practice tool designed as a static web application. Serves as both a valuable educational resource and lead generation tool for network consulting services. Demonstrates technical expertise while capturing qualified leads interested in enterprise networking.

## Requirements
- **Static HTML/JS** - No backend, runs on GitHub Pages
- **JSON question banks** - Separate files per topic with comprehensive schemas
- **Session-only tracking** - localStorage for preferences, no persistent user data
- **Original content** - Educational scenarios, no exam dumps (legal compliance)
- **Responsive design** - Mobile-first approach for accessibility
- **Lead integration** - Subtle contact capture for advanced features

## Enhanced JSON Schema

### Topic Bank Structure
```json
{
  "topic": "VLSM",
  "certification": ["CCNA", "Network+"],
  "difficulty": "intermediate",
  "timeEstimate": 30,
  "totalQuestions": 25,
  "passingScore": 80,
  "description": "Variable Length Subnet Masking concepts and calculations",
  "prerequisites": ["Basic Subnetting"],
  "learningObjectives": [
    "Calculate optimal subnet sizes",
    "Minimize IP address waste",
    "Design hierarchical networks"
  ],
  "questions": [
    {
      "id": "vlsm_001",
      "question": "Given network 192.168.1.0/24, design subnets for: 50 hosts, 25 hosts, 10 hosts. What are the subnet masks?",
      "type": "calculation",
      "difficulty": "medium",
      "points": 10,
      "timeLimit": 180,
      "answer": {
        "correct": ["/26", "/27", "/28"],
        "format": "array"
      },
      "explanation": "50 hosts needs 6 host bits (2^6-2=62) = /26, 25 hosts needs 5 host bits (2^5-2=30) = /27, 10 hosts needs 4 host bits (2^4-2=14) = /28",
      "hints": [
        "Use formula 2^n - 2 for usable hosts",
        "Start with largest subnet requirement",
        "Work in order: largest to smallest"
      ],
      "references": [
        "RFC 1918 - Private Address Space",
        "CCNA Study Guide Chapter 8"
      ],
      "tags": ["subnetting", "vlsm", "calculation"]
    }
  ]
}
```

### Question Types

#### 1. Multiple Choice
```json
{
  "type": "multiple_choice",
  "question": "Which routing protocol uses the Bellman-Ford algorithm?",
  "options": ["OSPF", "RIP", "EIGRP", "BGP"],
  "answer": {"correct": "RIP", "index": 1},
  "explanation": "RIP uses the Bellman-Ford algorithm for distance-vector routing."
}
```

#### 2. Multiple Select
```json
{
  "type": "multiple_select",
  "question": "Which protocols operate at Layer 3? (Select all that apply)",
  "options": ["IP", "TCP", "ICMP", "UDP", "ARP"],
  "answer": {"correct": ["IP", "ICMP"], "indices": [0, 2]}
}
```

#### 3. Calculation
```json
{
  "type": "calculation",
  "question": "Calculate the broadcast address for 192.168.10.50/28",
  "answer": {"correct": "192.168.10.63", "format": "ip_address"},
  "validation": "ip_format"
}
```

#### 4. Simulation (CLI Commands)
```json
{
  "type": "simulation",
  "scenario": "Configure OSPF on Cisco router",
  "question": "Enter commands to enable OSPF process 1 and advertise network 192.168.1.0/24 in area 0",
  "answer": {
    "correct": [
      "router ospf 1",
      "network 192.168.1.0 0.0.0.255 area 0"
    ],
    "partial_credit": true
  }
}
```

#### 5. Drag & Drop
```json
{
  "type": "drag_drop",
  "question": "Match OSI layers with their protocols",
  "items": ["HTTP", "TCP", "IP", "Ethernet"],
  "targets": ["Layer 7", "Layer 4", "Layer 3", "Layer 2"],
  "answer": {"correct": [[0,0], [1,1], [2,2], [3,3]]}
}
```

## Core Application Features

### Practice Modes
- **Study Mode**: Untimed practice with immediate feedback
- **Exam Mode**: Timed simulation with end-of-test results
- **Topic Focus**: Practice specific certification topics
- **Weak Areas**: Adaptive practice on missed questions
- **Random Practice**: Mixed questions from all topics

### Progress Tracking (Session-Only)
- Score tracking per session
- Question accuracy by topic
- Time per question analytics
- Weak area identification
- Study time tracking

### Learning Features
- Detailed explanations for all answers
- Progressive hints system
- Reference materials linking
- Formula and command references
- Interactive network diagrams

### User Experience
- Clean, distraction-free interface
- Mobile-responsive design
- Dark/light theme support
- Keyboard shortcuts for power users
- Progress indicators and visual feedback

## Technical Architecture

### Astro Implementation
```
src/
├── pages/
│   ├── index.astro           # Landing page
│   ├── practice/
│   │   ├── index.astro       # Practice tool main
│   │   ├── [topic].astro     # Topic-specific practice
│   │   └── exam.astro        # Exam simulation mode
│   └── reference/
│       └── [topic].astro     # Reference materials
├── components/
│   ├── QuestionRenderer.astro
│   ├── ScoreCard.astro
│   ├── ProgressBar.astro
│   └── TopicSelector.astro
├── data/
│   ├── questions/
│   │   ├── vlsm.json
│   │   ├── subnetting.json
│   │   ├── ospf.json
│   │   └── bgp.json
│   └── schemas/
│       └── question-schema.json
└── utils/
    ├── questionEngine.js
    ├── scoreCalculator.js
    └── progressTracker.js
```

### localStorage Schema
```json
{
  "practiceSession": {
    "startTime": "2024-01-15T10:00:00Z",
    "currentQuestion": 5,
    "answers": [{"questionId": "vlsm_001", "answer": "/26", "correct": true, "timeSpent": 45}],
    "score": {"correct": 8, "total": 10, "percentage": 80},
    "mode": "study",
    "topic": "vlsm"
  },
  "preferences": {
    "theme": "dark",
    "showHints": true,
    "autoAdvance": false,
    "soundEnabled": true
  },
  "analytics": {
    "topicsStudied": ["vlsm", "ospf"],
    "totalQuestions": 150,
    "totalTime": 3600,
    "averageScore": 85
  }
}
```

## Content Strategy

### Topic Roadmap

#### CCNA Focus Areas
1. **Network Fundamentals**
   - OSI Model & TCP/IP Stack
   - Ethernet & Switching Concepts
   - IPv4/IPv6 Addressing

2. **Routing & Switching**
   - VLSM & Subnetting
   - OSPF Configuration
   - EIGRP Implementation
   - Inter-VLAN Routing

3. **Network Security**
   - ACLs Configuration
   - Basic Security Concepts
   - Device Hardening

4. **Network Services**
   - DHCP & DNS
   - NAT/PAT Configuration
   - QoS Basics

#### CompTIA Network+ Areas
1. **Network Troubleshooting**
2. **Network Operations**
3. **Network Security**
4. **Network Architecture**

### Quality Guidelines
- All questions original content
- Realistic scenarios based on enterprise environments
- Progressive difficulty within topics
- Clear, concise explanations
- Industry-standard terminology

## Business Integration Strategy

### Lead Generation
- **Soft Capture**: Email for progress save/resume (optional)
- **Advanced Features**: Contact required for exam simulation mode
- **Study Plans**: Personalized study guides via consultation
- **Practice Labs**: Link to hands-on lab services

### Technical Credibility
- Demonstrates deep networking knowledge
- Showcases educational content creation
- Builds trust through valuable free resource
- Positions as go-to networking expert

### Revenue Opportunities
- **Direct Consultation**: Network design/implementation
- **Training Services**: Custom certification prep
- **Lab Services**: Hands-on practice environments
- **Content Licensing**: Sell question banks to training providers

### Marketing Integration
- SEO-optimized for certification keywords
- Social proof through user testimonials
- Integration with existing landing page services
- Cross-promotion of network and web services

## Implementation Phases

### Phase 1: MVP (2-3 weeks)
- Basic question engine
- VLSM topic with 25 questions
- Simple score tracking
- Mobile-responsive design

### Phase 2: Core Features (3-4 weeks)
- Multiple question types
- 3-4 complete topics
- Progress tracking
- Hint system

### Phase 3: Enhanced Experience (2-3 weeks)
- Exam simulation mode
- Advanced analytics
- Reference materials
- Lead capture integration

### Phase 4: Business Integration (1-2 weeks)
- Contact forms optimization
- Analytics tracking
- SEO optimization
- Cross-service promotion

## Success Metrics
- **User Engagement**: Time spent, questions completed
- **Educational Value**: Score improvements, topic completion
- **Lead Generation**: Contact captures, consultation requests
- **SEO Performance**: Organic traffic, certification keyword rankings
- **Business Impact**: Network service inquiries, consultation bookings