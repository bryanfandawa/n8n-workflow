# 📖 n8n Sermon Automation Workflow

This repository showcases my development of an AI-powered automation workflow designed to assist churches in creating content from sermon audio files. The workflow utilizes **n8n** (self-hosted using Docker and a virtual machine) and **ChatGPT** to streamline the process of generating blog posts, transcripts, and social media content.

---

## 🧠 Overview

The core idea is to automate the process of turning sermons into digital content:

1. **Audio Processing**: Sermon audio files are cut into chunks and compressed.
2. **Transcription**: The audio chunks are sent to a transcription service.
3. **AI Integration**: Transcripts are sent to ChatGPT to generate summaries, blog posts, and social media posts.
4. **Content Publishing**: Posts are automatically published to WordPress and other platforms.

---

## 🛠️ Technologies Used

- [n8n](https://n8n.io/) (self-hosted with Docker)
- [ChatGPT](https://platform.openai.com/)
- WordPress (via REST API)
- FFmpeg (for audio processing)
- Whisper (ASR for transcription, optional)

---

## ⚙️ Prerequisites

- Docker & Docker Compose installed on your VM
- OpenAI API Key (for ChatGPT)
- WordPress REST API credentials
- (Optional) Whisper ASR API for transcription
- Properly configured `.env` file with keys

---
