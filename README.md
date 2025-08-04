# 📖 n8n Sermon Automation Workflow

This repository contains a fully automated AI-powered transcription and content creation workflow for church sermon audio files using **n8n**, **Whisper**, **Google Docs**, and **Gemini AI**.

Hosted on a **self-hosted Docker-based virtual machine**, this workflow processes sermon audio from WordPress, transcribes it, cleans it up, and posts it back — all without human intervention unless approval is needed.

---

## 🧠 Overview

The workflow runs in five major phases:

### 🔹 Phase 1: Audio Preparation

- Triggered via a **Webhook** from WordPress.
- Extracts the sermon’s audio URL from the WordPress post.
- Uses `ffmpeg` to cut and compress the audio into chunks.
- Lists the resulting audio files and creates an array of paths.
- Splits the array to allow **chunk-by-chunk processing** in batches.

### 🔹 Phase 2: Transcription

- Iterates through the audio chunks.
- Reads each `.mp3` file as binary.
- Sends it to a local **Whisper API** container for transcription.
- Waits 3 seconds between requests to prevent server overload.
- Aggregates all transcript chunks into a single list.

### 🔹 Phase 3: AI Cleanup + Google Docs

- Combines all transcript parts into a readable string.
- Uses **Gemini AI** via the n8n `AI Agent` node to clean, structure, and format the text.
- Creates a new Google Docs file titled `Sermon_YYYY-MM-DD_HH-mm`.
- Inserts the cleaned transcript into the Google Docs.

### 🔹 Phase 4: Document Formatting + Export

- Fetches the Google Docs file structure using the raw Google Docs API (via HTTP request).
- Reformats the text with:
  - Font: Times New Roman  
  - Font size: 12pt  
  - Paragraph alignment: Justified
- Sends a batchUpdate request to Google Docs.
- Re-fetches the updated document.
- Converts it to **clean HTML** using a Code node to prepare for display/email/blog use.

### 🔹 Phase 5: Human Review & WordPress Publishing

- Sends an email with the HTML transcript asking the user to Approve or Decline.
- If **approved**:
  - Gets the final Google Docs content.
  - Publishes the cleaned HTML back to the original WordPress post.
- If **declined**:
  - Sends a separate editable Google Docs link.
  - Awaits user manual revision.
  - Then republishes to WordPress upon re-approval.
- Cleans up chunk files (`part_*.mp3`) to save VM space.

---

## 🛠️ Technologies Used

- [n8n](https://n8n.io/) (self-hosted via Docker)
- [Google Gemini AI](https://deepmind.google) (via AI Agent integration)
- [Google Docs API](https://developers.google.com/docs/api)
- [FFmpeg](https://ffmpeg.org/) for audio chunking
- [Whisper](https://github.com/openai/whisper) — ASR engine (self-hosted container)
- [WordPress REST API](https://developer.wordpress.org/rest-api/)
- Gmail API (for review/approval emails)

---

## ⚙️ Prerequisites

- ✅ Docker & Docker Compose installed on your server/VM
- ✅ Google Docs and Gmail API credentials (OAuth2)
- ✅ WordPress REST API credentials
- ✅ Self-hosted Whisper model container
- ✅ Gemini API key via Google AI Studio
- ✅ A webhook set up from WordPress to trigger the flow
- ✅ `ffmpeg` installed in your n8n container (manually or via Dockerfile)

---

## 💡 Use Case

Perfect for churches and media teams who want to:

- Automate transcription from sermon audio
- Generate cleaned transcripts for blogs or newsletters
- Keep human-in-the-loop review when needed
- Publish approved content to their WordPress site with minimal effort

---

