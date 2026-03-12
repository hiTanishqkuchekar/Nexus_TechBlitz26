# AI Sales Intelligence Agent

## Overview

Many businesses receive inquiries from potential customers but often respond late or fail to prioritize important leads.

This project is a simple AI-powered system that captures leads, analyzes them, and helps automate the response process.
The goal is to help businesses identify valuable leads quickly and respond faster.

This project was built as part of a hackathon to demonstrate how AI and workflow automation can improve lead management.

---

## Problem

Businesses often struggle to manage incoming leads effectively.

Common problems include:

* Slow responses to customer inquiries
* No proper way to prioritize leads
* Manual follow-ups
* Losing potential customers due to delayed responses

---

## Solution

This system automates the process of handling leads.

Instead of manually checking each inquiry, the system uses AI to analyze the message and provide useful insights about the lead.

It helps businesses understand which leads are important and what action should be taken next.

---

## How It Works

1. A user submits their information through a simple form.
2. The form sends the lead data to an automation workflow.
3. AI analyzes the message and generates insights such as:

   * Lead Score
   * Customer Intent
   * Suggested Action
4. The lead information is stored in Google Sheets.
5. The system can notify the sales team about new leads.

---

## Features

### Lead Capture

A simple form built using Streamlit collects lead information such as name, company, email, and message.

### AI Lead Analysis

The system analyzes the lead and generates useful insights like lead score and customer intent.

### Workflow Automation

n8n is used to automate the workflow and process incoming leads.

### Simple Lead Storage

Google Sheets is used to store leads and act as a lightweight CRM.

---

## System Architecture

Streamlit (Frontend)
↓
n8n Webhook
↓
AI Lead Analysis
↓
Google Sheets (Lead Storage)
↓
Notifications / Automated Actions

---

## Tech Stack

Frontend
Streamlit

Automation
n8n

AI
LLM API

Database
Google Sheets

---

## Demo Flow

1. User submits a lead through the form
2. The data is sent to the automation workflow
3. AI analyzes the lead and generates insights
4. Lead data is stored in Google Sheets
5. The system helps identify important leads quickly

---

## Author

Tanishq Kuchekar
