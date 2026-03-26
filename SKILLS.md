# Agent Skills — Master Index

This file is the **single source of truth** for all available skills in this repository.
AI agents should read this file first to understand what skills exist, what triggers them,
and which folder to load the full SKILL.md from.

> **How to use this index:**
> 1. Scan the Trigger Keywords column for matches to the current task
> 2. If multiple skills match, pick the most specific one
> 3. Load `skills/<folder>/SKILL.md` for the full instructions
> 4. Never load a skill you don't need — one skill per task

---

## 🎨 Design & UI/UX

| Skill | Folder | Trigger Keywords |
|---|---|---|
| Frontend Design | `frontend-design` | build UI, web component, landing page, HTML/CSS, styling, beautify |
| Taste Skill | `taste-skill` | design quality, anti-slop, AI slop, generic UI, design variance, visual density |
| UI Constraints | `ui-skills` | UI rules, opinionated UI, interface constraints, design constraints |
| Web Design Guidelines | `web-design-guidelines` | web standards, accessibility, audit UI, check design, review UX |
| Platform Design | `platform-design-skills` | Apple HIG, Material Design, WCAG, cross-platform, iOS design rules, Android design rules |
| Apple HIG | `apple-hig-skills` | Apple HIG, iOS guidelines, macOS guidelines, visionOS, SwiftUI design |
| Canvas Design | `canvas-design` | poster, art, PNG, PDF design, visual art, static design |
| Algorithmic Art | `algorithmic-art` | generative art, p5.js, flow fields, particle system, seeded randomness |
| Brand Guidelines | `brand-guidelines` | brand colors, Anthropic brand, brand typography, company style |
| Theme Factory | `theme-factory` | apply theme, styled artifact, themed document, color theme, arctic frost, midnight galaxy |
| Color Expert | `color-expert` | color theory, color palette, OKLCH, color naming, color accessibility, pigment, contrast |
| Creative Director | `creative-director` | creative direction, campaign concept, creative brief, SIT, TRIZ, SCAMPER |
| Beautiful Prose | `beautiful-prose` | brand voice, rewrite copy, no AI tics, timeless prose, forceful writing |

---

## ✨ Animation & Motion

| Skill | Folder | Trigger Keywords |
|---|---|---|
| GSAP Core | `gsap-core` | GSAP, animation library, gsap.to, gsap.from, tween, easing, stagger |
| GSAP Timeline | `gsap-timeline` | animation sequence, GSAP timeline, choreograph animations |
| GSAP ScrollTrigger | `gsap-scrolltrigger` | scroll animation, scroll-linked, parallax, pinning, scrolltrigger |
| GSAP Plugins | `gsap-plugins` | Flip animation, Draggable, SplitText, ScrambleText, SVG animation, ScrollToPlugin |
| GSAP React | `gsap-react` | GSAP in React, useGSAP, Next.js animation, animation cleanup |
| GSAP Performance | `gsap-performance` | animation jank, 60fps, animation performance, will-change, reduce motion |

---

## 🖥️ Next.js & React

| Skill | Folder | Trigger Keywords |
|---|---|---|
| Next.js Best Practices | `next-best-practices` | Next.js, RSC, server components, data fetching, App Router, metadata, route handlers |
| Next.js Cache | `next-cache-components` | PPR, use cache, cacheLife, cacheTag, Next.js caching |
| Next.js Upgrade | `next-upgrade` | upgrade Next.js, migrate Next.js, codemod |
| React Best Practices | `react-best-practices` | React performance, re-renders, bundle size, React optimization, TTI |
| Composition Patterns | `composition-patterns` | compound components, render props, component reuse, React 19, flexible components |
| shadcn/ui | `shadcn-ui` | shadcn, shadcn/ui, radix, component library, install shadcn |
| React Components | `react-components` | Stitch to React, convert design to React, component from design |
| Design MD | `design-md` | DESIGN.md, design system file, design tokens file |
| Enhance Prompt | `enhance-prompt` | improve prompt, better UI prompt, Stitch prompt |
| Web Artifacts Builder | `web-artifacts-builder` | complex artifact, React artifact, claude.ai artifact |

---

## 📱 Mobile (React Native / Expo)

| Skill | Folder | Trigger Keywords |
|---|---|---|
| React Native Best Practices | `react-native-best-practices` | React Native, RN performance, FPS, Hermes, FlashList, bridge |
| Upgrading React Native | `upgrading-react-native` | upgrade React Native, RN 0.x to 0.y, rn-diff-purge |
| Expo App Design | `expo-app-design` | Expo app, Expo Router, native UI, NativeWind |
| Expo Deployment | `expo-deployment` | deploy Expo, EAS Build, EAS Submit, TestFlight, Expo production |
| Upgrading Expo | `upgrading-expo` | upgrade Expo, Expo SDK upgrade, Expo 50, Expo 51 |

---

## 🖌️ Figma & Design Systems

| Skill | Folder | Trigger Keywords |
|---|---|---|
| Figma Generate Design | `figma-generate-design` | write to Figma, create in Figma, push page to Figma, build screen in Figma |
| Figma Implement Design | `figma-implement-design` | implement Figma design, code from Figma, Figma URL to code |
| Figma Design System Rules | `figma-create-design-system-rules` | design system rules, Figma rules, Figma code connect |
| Gstack Design Consultation | `gstack-design-consultation` | build design system, create DESIGN.md, design from scratch, aesthetic |
| Gstack Design Review | `gstack-design-review` | visual QA, design audit, design polish, check if it looks good |
| Gstack Plan Design Review | `gstack-plan-design-review` | review design plan, rate design, design critique, plan design |

---

## 🛒 E-Commerce & Marketing

| Skill | Folder | Trigger Keywords |
|---|---|---|
| Stripe Best Practices | `stripe-best-practices` | Stripe, payment, checkout, subscription, Connect, billing, PaymentIntent |
| Copywriting | `copywriting` | write marketing copy, landing page copy, hero copy, headline, CTA, value proposition |
| Page CRO | `page-cro` | conversion rate, CRO, this page isn't converting, improve conversions, bounce rate |
| Content Strategy | `content-strategy` | what should I write about, content plan, blog topics, editorial calendar |
| Social Content | `social-content` | LinkedIn post, Twitter thread, Instagram, social media, tweet, content calendar |
| Launch Strategy | `launch-strategy` | product launch, Product Hunt, go-to-market, GTM plan, feature announcement |
| Ad Creative | `ad-creative` | ad copy, Facebook ad, Google ad, headlines, RSA headlines, ad variations |

---

## 🗄️ Backend & Database

| Skill | Folder | Trigger Keywords |
|---|---|---|
| Supabase Postgres | `supabase-postgres-best-practices` | Supabase, Postgres, SQL, database, query optimization, schema |
| MCP Builder | `mcp-builder` | MCP server, Model Context Protocol, integrate API, build MCP |
| Better Auth Setup | `better-auth-best-practices` | Better Auth, auth.ts, authentication setup, session, auth configuration |
| Better Auth Create | `better-auth-create-auth` | scaffold auth, add login, add sign-up, implement authentication |
| Better Auth Email | `better-auth-emailAndPassword` | email verification, password reset, login flow, sign-in, credential |
| Better Auth 2FA | `better-auth-twoFactor` | two-factor authentication, TOTP, 2FA, MFA, authenticator app, OTP |

---

## 🔁 Dev Workflow & Quality

| Skill | Folder | Trigger Keywords |
|---|---|---|
| Dev Workflow Skills | `dev-workflow-skills` | PRD writing, TDD, codebase architecture, git guardrails, issue triage, refactoring |
| Startup Skills | `startup-skills` | startup, SaaS, new project, bootstrap, spec-driven, security-first |
| GitHub Patterns | `github` | pull request, stacked PRs, branching, code review, GitHub workflow |
| Webapp Testing | `webapp-testing` | test web app, Playwright, browser test, UI test, visual test |
| Skill Creator | `skill-creator` | create skill, new skill, modify skill, skill performance, skill eval |

---

## 📄 Documents & Media

| Skill | Folder | Trigger Keywords |
|---|---|---|
| Word Documents | `docx` | Word document, .docx, word doc, DOCX |
| PowerPoint | `pptx` | PowerPoint, .pptx, presentation, deck, slides |
| Excel | `xlsx` | Excel, .xlsx, spreadsheet, CSV, tabular data |
| PDF | `pdf` | PDF, extract PDF, create PDF, form filling |
| Remotion | `remotion` | Remotion, video in React, programmatic video |
| Internal Comms | `internal-comms` | status report, newsletter, leadership update, incident report |
| Doc Coauthoring | `doc-coauthoring` | write documentation, proposal, technical spec, decision doc |

---

## 🤖 Trading Bot

| Skill | Folder | Trigger Keywords |
|---|---|---|
| Trading Bot | `trading-bot` | trading bot, MT5, XAUUSD, forex, gold trading, confluence filter, FTMO, vectorbt, TradingAgents |

---

## 🧭 Skill Selection Rules

1. **Be specific** — if the task is "scroll animation in React", use `gsap-react` + `gsap-scrolltrigger`, not `frontend-design`
2. **One skill at a time** unless two are explicitly needed (e.g., `gsap-scrolltrigger` + `gsap-react`)
3. **Design tasks**: check `taste-skill` first for anti-slop guidance before building any UI
4. **New projects**: `startup-skills` → `gstack-design-consultation` → `next-best-practices`
5. **Clothing brand / e-commerce**: `taste-skill` + `gsap-scrolltrigger` + `copywriting` + `stripe-best-practices`
6. **Auth**: always `better-auth-best-practices` first, then specific plugin skill
7. **When in doubt**: read the description at the top of the relevant SKILL.md before proceeding
