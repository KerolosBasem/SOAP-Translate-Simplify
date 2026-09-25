# 🏥 AI Healthcare Documentation & Translation Automation

An AI-powered healthcare automation workflow built with **n8n** that processes medical information received through Telegram and intelligently routes it to the appropriate AI pipeline.

The system supports three main healthcare workflows:

* 🩺 **SOAP Note Generation**
* 📝 **Medical Document Simplification**
* 🌍 **Medical Document Translation & Validation**

The workflow uses AI models and automation services to transform raw patient information, voice recordings, and medical documents into structured, professional, and patient-friendly outputs.

## ✨ Features

### 🩺 1. AI SOAP Note Generation

Healthcare professionals can send a **voice message or audio recording** containing a patient consultation through Telegram.

The workflow:

1. Receives the Telegram message or audio.
2. Determines the input type.
3. Downloads the audio file.
4. Transcribes the consultation using **Google Gemini** or **Groq Whisper**.
5. Checks the transcript length.
6. Uses an AI agent to generate a structured SOAP note.
7. Formats the result into:

   * **S – Subjective**
   * **O – Objective**
   * **A – Assessment**
   * **P – Plan**
8. Sends the generated SOAP draft to a doctor for review and approval.

The workflow explicitly treats the generated SOAP note as an AI draft requiring physician review.

### 🌍 2. Medical Document Translation

Medical documents can be submitted through Telegram and processed automatically.

The translation pipeline:

1. Receives the document through Telegram.
2. Downloads the file.
3. Extracts the document text.
4. Identifies the source and target languages.
5. Generates an initial translation.
6. Refines the translation for accuracy, terminology, grammar, and natural language.
7. Performs an independent **back-translation**.
8. Compares the original text with the back-translated version.
9. Detects potential meaning changes, omissions, additions, altered numbers, dates, dosages, or safety-critical discrepancies.

The translation pipeline is designed to preserve the original meaning and use professional medical terminology.

### 📝 3. Medical Text Simplification

The system can route requests asking for medical documents or reports to be **explained in patient-friendly language**.

The intent-classification layer specifically recognizes requests such as:

* Explain this medical report
* Make this document patient-friendly
* Simplify this medical information
* تبسيط
* شرح للمريض

The goal is to make complex medical information easier for patients to understand while preserving the important information.

### 🧠 AI Intent Router

An AI routing agent analyzes incoming Telegram messages and determines which workflow should process the request.

It supports four routes:

```text
SOAP
SIMPLIFIED
TRANSLATE
UNKNOWN
```

This allows a single Telegram interface to act as the entry point for multiple healthcare automation workflows.

### 🔐 Access Control

The workflow includes an authorization step that checks the Telegram user's identity before allowing access to the system. Unauthorized users receive an **"Access denied!"** response.

### 📩 Doctor Review & Approval

SOAP notes are not automatically treated as final clinical documentation.

The generated draft is sent to the assigned doctor through email with approval options. The doctor can:

* ✅ Approve the SOAP note
* ✏️ Request an edit

If an edit is requested, the workflow sends the doctor the original draft and a reference token, while also notifying the relevant Telegram user.

### 🤖 AI Technologies

The workflow integrates multiple AI services, including:

* **Google Gemini** for transcription, classification, and language-model tasks
* **Groq Whisper** for audio transcription
* **DeepL** for the initial translation stage
* **n8n AI Agents** for orchestration and processing

For example, the workflow uses the `whisper-large-v3` model through the Groq transcription API and Gemini models for AI processing.

## 🔄 High-Level Architecture

```text
                 ┌─────────────────────┐
                 │   Telegram User     │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │  Telegram Trigger  │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   Access Control    │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │    AI Intent Router │
                 └──────────┬──────────┘
                            │
            ┌───────────────┼────────────────┐
            ▼               ▼                ▼
       ┌─────────┐    ┌────────────┐   ┌────────────┐
       │  SOAP   │    │ Simplify   │   │ Translate  │
       └────┬────┘    └──────┬─────┘   └──────┬─────┘
            │                │                │
            ▼                ▼                ▼
       Transcription    AI Processing    Translation
            │                │                │
            ▼                │                ▼
       SOAP Generator        │         Back Translation
            │                │                │
            ▼                │                ▼
       Doctor Review         │         Validation
            │                │                │
            └────────────────┴────────────────┘
                            │
                            ▼
                    Telegram / Email
```

## 🛠️ Main n8n Components

The workflow uses several n8n components, including:

* Telegram Trigger
* Telegram file download
* Switch / IF nodes
* n8n AI Agents
* Google Gemini Chat Models
* Structured Output Parsers
* HTTP Request
* Code nodes
* PDF text extraction
* Gmail integration
* Telegram notifications

## 🔒 Important Security & Clinical Considerations

This project is intended as an **automation and documentation assistant**, not as an autonomous medical decision-making system.

AI-generated SOAP notes should be reviewed by a qualified healthcare professional before being used as clinical documentation. The workflow itself includes a doctor approval stage before the note is considered filed.

When deploying this project with real patient information, appropriate security, privacy, access-control, data-retention, and healthcare compliance requirements should be implemented for the deployment environment.

## 🚀 Use Cases

This workflow can be adapted for:

* Clinical documentation automation
* Medical transcription
* Patient-friendly medical explanations
* Medical report translation
* Multilingual healthcare communication
* Doctor documentation assistance
* Healthcare Telegram bots
* AI-assisted administrative workflows

## 📦 Installation

1. Install and configure **n8n**.
2. Import the provided n8n workflow JSON file.
3. Configure the required credentials:

   * Telegram Bot API
   * Google Gemini
   * Groq API
   * DeepL
   * Gmail
4. Update the authorized Telegram user configuration.
5. Configure the source and target languages for translation.
6. Activate the workflow.
7. Send a supported message or document through Telegram.

## ⚙️ Configuration

Before running the workflow, review the credential and configuration nodes and replace any environment-specific values.

In particular, configure:

```text
Telegram credentials
Google Gemini API
Groq API
DeepL API
Gmail OAuth2
Authorized Telegram users
Source language
Target language
Doctor email
```

## 📌 Project Status

**Status:** Active Development

This project demonstrates how **n8n + AI agents + Telegram + healthcare-oriented automation** can be combined into a unified workflow for clinical documentation, medical communication, and translation.
