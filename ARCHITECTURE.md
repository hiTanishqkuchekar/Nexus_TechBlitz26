# AI Sales Intelligence Agent - System Architecture

## Overview
A full-stack AI-powered sales automation system.

## Stack
- Frontend: Next.js 14 + TailwindCSS
- Backend/API: Next.js API Routes
- AI: OpenAI GPT-4 / Gemini Pro
- Automation: n8n
- Database: Supabase (PostgreSQL)
- Notifications: Telegram Bot

## Data Flow
1. User submits lead form on frontend
2. Frontend POSTs to n8n webhook
3. n8n sends data to AI analysis endpoint
4. AI returns structured lead intelligence
5. n8n stores in Supabase
6. n8n notifies sales team via Telegram
7. n8n sends automated email reply to lead
8. Frontend polls/fetches from Supabase for display
