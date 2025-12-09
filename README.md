# n8n Workflows Repository

A collection of ready-to-use n8n workflows for automation and productivity.

## 📋 Overview

This repository contains various n8n workflows that you can import and use in your n8n instance. Each workflow is self-contained with its own documentation and setup instructions.

## 🚀 Getting Started

1. **Clone this repository**
   ```bash
   git clone <your-repo-url>
   cd n8n-Workflows
   ```

2. **Choose a workflow** from the `workflows/` directory

3. **Import into n8n**
   - Open your n8n instance
   - Go to **Workflows** → **Import from File**
   - Select the workflow JSON file
   - Follow the setup instructions in each workflow's README

## 📁 Workflows

### [AI Email Manager](./workflows/AI-Email-Manager/)
Intelligent Gmail automation that uses AI to automatically categorize emails with custom labels and draft replies for emails requiring responses.

[![Watch the video](https://img.youtube.com/vi/b3oLLiQmjsY/hqdefault.jpg)](https://youtu.be/b3oLLiQmjsY)

**Features:**
- AI-powered email categorization with customizable rules
- Automatic Gmail label application
- Smart draft generation for emails requiring replies
- Built-in evaluation system for testing accuracy
- Filters out emails already labeled by existing Gmail rules

### [AI Thought Logger](./workflows/ai-thought-logger/)
Automatically capture, transcribe, and organize your thoughts from Telegram (voice notes or text) into Google Sheets with AI-powered tagging and summarization.

**Features:**
- Voice note transcription via OpenAI
- AI-powered title, summary, and tag generation
- Automatic Google Sheets logging
- Telegram bot confirmation

## 📝 Adding New Workflows

When adding a new workflow:

1. Create a new folder in `workflows/` with a descriptive name (kebab-case)
2. Add the workflow JSON file (name it `workflow.json`)
3. Create a `README.md` in the workflow folder with:
   - Description of what the workflow does
   - Prerequisites/requirements
   - Setup instructions
   - Configuration steps
   - Usage examples

## 🤝 Contributing

Contributions are welcome! Please ensure:
- Workflows are tested and working
- Documentation is clear and complete
- Workflow files are properly formatted JSON

## 📄 License

This repository is open source. Please check individual workflow documentation for any specific licensing requirements.

## 🔗 Resources

- [n8n Documentation](https://docs.n8n.io/)
- [n8n Community Forum](https://community.n8n.io/)

---

**Made with ❤️ for the n8n community**

