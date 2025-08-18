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