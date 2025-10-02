# Health Saarthi

<p align="center">
  <img src="assets/banner-animated.svg" alt="Health Saarthi - animated AI + health banner" width="100%" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/license-MIT-2ecc71.svg" alt="license" />
  <img src="https://img.shields.io/github/actions/workflow/status/OWNER/REPO/ci.yml?branch=main" alt="build status" />
  <img src="https://img.shields.io/github/contributors/OWNER/REPO" alt="contributors" />
  <img src="https://img.shields.io/github/issues/OWNER/REPO" alt="open issues" />
  <img src="https://img.shields.io/codecov/c/github/OWNER/REPO" alt="coverage" />
  <img src="https://img.shields.io/github/last-commit/OWNER/REPO" alt="last commit" />
</p>

Tagline: An AI-driven, multilingual healthcare companion that combines preventive care, diagnostics, emergency response, and wearable integration into a single accessible platform.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Features](#features)
- [Architecture](#architecture)
  - [Mermaid diagrams (rendered on GitHub)](#mermaid-diagrams-rendered-on-github)
  - [ASCII / PNG fallback](#ascii--png-fallback)
- [Tech Stack](#tech-stack)
- [Installation & Setup](#installation--setup)
  - [Quick start (local)](#quick-start-local)
  - [Docker (recommended)](#docker-recommended)
  - [Example `.env` template](#example-env-template)
- [Usage & Examples](#usage--examples)
  - [Dev commands](#dev-commands)
  - [Sample API requests](#sample-api-requests)
- [Screenshots & Demo](#screenshots--demo)
- [Testing & CI/CD](#testing--cicd)
- [Roadmap & Future Scope](#roadmap--future-scope)
- [Contribution Guide](#contribution-guide)
- [License & Contact](#license--contact)
- [Optional Enhancements](#optional-enhancements)
- [Acknowledgements & Sources](#acknowledgements--sources)

---

## Project Overview

Health Saarthi is a full-stack, production-ready TypeScript application that combines AI-powered disease detection, multilingual voice/text chatbot, wearable integration, ambulance tracking, vaccination scheduling, and administrative dashboards into one platform. It reduces late diagnoses, encourages preventive care adoption, and improves access for low-literacy or multilingual users.

Technical materials (architecture, workflows, diagrams) used to craft this README come from the project PPT and technical specification supplied with the project. :contentReference[oaicite:0]{index=0}

Why this project matters
- Early triage and diagnostics reduce burden on tertiary care.
- Voice and multilingual interfaces increase usability in low-literacy contexts.
- Wearable connectivity and push reminders improve preventive adherence.

---

## Objectives

- Provide a unified health companion covering triage, diagnosis, reminders, and emergency workflows.
- Maintain end-to-end type safety using TypeScript (shared contracts).
- Prioritize HTTPS for Web Bluetooth and geolocation features.
- Support both serverful (Node) and serverless (Netlify/Vercel) deployment models.
- Keep the code modular, testable, and CI-driven.

---

## Features

- 🩺 ML-powered disease detection  
  Upload skin/oral/X-ray images; PyTorch models (server or ML microservice) return label, confidence, and recommended next steps.  
  Asset placeholder: `assets/gifs/disease-detection.gif` (alt: "disease detection demo GIF")

- 🗣️ Voice & text Vapi chatbot  
  Multilingual voice assistant powered by a Vapi proxy for STT/TTS and intent handling. Useful for triage and FAQs.  
  Asset placeholder: `assets/gifs/vapi-chat.gif` (alt: "voice chatbot demo GIF")

- 💉 Vaccination tracker & reminders  
  Family-centric schedules, local health centre sync, push and SMS reminders, calendar integration.  
  Asset placeholder: `assets/svgs/vaccine.svg` (alt: "vaccination tracker illustration")

- 🚑 Live ambulance tracking & SOS  
  One-tap SOS, real-time ambulance location on a map, booking and estimated arrival.  
  Asset placeholder: `assets/gifs/ambulance-tracking.gif` (alt: "ambulance tracking GIF")

- 🧠 Mental health screening & companion  
  PHQ-9 and GAD-7 assessments, risk scoring, and referral workflows to counselors or teleconsultation.  
  Asset placeholder: `assets/svgs/mental-health.svg` (alt: "mental health illustration")

- 🏋️ YOLOv8 pose & exercise guidance  
  Real-time pose estimation and feedback with YOLOv8/pose models for exercise guidance and rep counting.  
  Asset placeholder: `assets/gifs/yolov8-pose.gif` (alt: "pose correction GIF")

- 👥 Admin dashboard & doctor discovery  
  Map visualizations for doctor availability, community dashboards for NGOs, and automated reporting after consultations.  
  Asset placeholder: `assets/svgs/dashboard.svg` (alt: "admin dashboard preview")

---

## Architecture

The project uses a unified Vite dev server for local development, an Express backend for APIs, and PyTorch/YOLOv8 services for ML/vision tasks. Shared TypeScript contracts live in `shared/api.ts` to keep client and server synchronized.

Mermaid diagrams below describe the topology, image-diagnosis sequence, and reminder data flow.

### Mermaid diagrams (rendered on GitHub)

System diagram
```mermaid
flowchart LR
  subgraph Client
    A[React SPA<br/>Vite + Tailwind] 
    B[MiBand Adapter<br/>Web Bluetooth]
  end

  subgraph Backend
    C[Express API<br/>/api/*]
    D[Vapi Proxy<br/>(STT/TTS)]
    E[ML Services<br/>PyTorch / YOLOv8]
    F[Auth & DB<br/>Postgres / Redis]
  end

  subgraph Infra
    G[Storage<br/>S3 / Firebase Storage]
    H[CI/CD<br/>GitHub Actions]
  end

  A -->|HTTP / WebSocket| C
  B -->|Bluetooth| A
  C --> D
  C --> E
  C --> F
  E --> G
  C --> G
  H -. deploy .-> A
  H -. deploy .-> C
