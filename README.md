# n8n Personal AI Agent

A self-hosted AI agent workflow powered by n8n, Ollama, and LangChain. This project demonstrates how to build an intelligent personal assistant that can search Wikipedia and respond to chat messages.

## 🎯 Features

- **Local AI Processing**: Uses Ollama with Llama3 model for offline language processing
- **Tool Integration**: Integrates Wikipedia search tool for knowledge retrieval
- **Chat Interface**: Web-based chat interface for interacting with the agent
- **Memory Management**: Maintains conversation history with Window Buffer Memory
- **LangChain Integration**: Leverages LangChain's Tools Agent for sophisticated tool calling

## 📋 Prerequisites

- Docker & Docker Compose
- Ollama installed and running locally
- Llama3 model downloaded in Ollama: `ollama pull llama3`
- Node.js and npm (if modifying the workflow)

## 🚀 Quick Start

### 1. Clone the repository
```bash
git clone https://github.com/dronamg-ai/n8nagent.git
cd n8nagent
```

### 2. Start n8n with Docker
```bash
docker compose up -d
```

The n8n instance will be available at `http://localhost:5678`

### 3. Verify Ollama is running
```bash
ollama serve
```

In another terminal, verify the model is available:
```bash
ollama list
```

### 4. Import the workflow
1. Open n8n at `http://localhost:5678`
2. Go to **Workflows** → **Import from file**
3. Select `personal_agent_workflow.json`
4. Click "Activate" to start the workflow

## 📁 Project Structure

```
.
├── docker-compose.yml              # n8n Docker container configuration
├── personal_agent_workflow.json     # The AI agent workflow definition
├── package.json                     # Project metadata
├── .gitignore                       # Git ignore rules
└── README.md                        # This file
```

## 🔧 Configuration

### Docker Compose Settings
- **Port**: 5678 (accessible at `http://localhost:5678`)
- **Data Volume**: `n8n_data` (persistent storage)
- **Environment**: Production mode with UTC timezone

### Workflow Nodes

1. **When chat message received** - Chat trigger node
2. **AI Agent** - Tools Agent using Ollama + Wikipedia tool
3. **Ollama Chat Model** - Llama3 language model
4. **Window Buffer Memory** - Maintains conversation history
5. **Wikipedia Tool** - Search Wikipedia for information

## 💬 Usage

1. Activate the workflow in n8n
2. Open the chat interface
3. Type a message to interact with the agent
4. The agent will use Wikipedia tool as needed to answer questions

Example prompts:
- "Who is Batman?"
- "Tell me about artificial intelligence"
- "What is n8n?"

## ⚠️ Troubleshooting

### Tool Input Schema Error
**Error**: `"Expected string, received object"`

**Solution**: This occurs when the agent tries to pass tool parameters as JSON objects instead of strings. The workflow is configured to handle this, but if you still encounter it:

1. Ensure `returnDirect: true` is set on the Wikipedia tool
2. Check that the system message instructs the agent to pass plain strings
3. Verify Ollama is running and Llama3 model is downloaded
4. Try reducing `maxIterations` to 5 in the AI Agent configuration

### Container Not Running
```bash
# Check container status
docker compose ps

# View logs
docker compose logs -f n8n

# Restart container
docker compose restart
```

### Ollama Connection Issues
- Ensure Ollama service is running on `http://localhost:11434`
- Update the Ollama Chat Model node if using a different Ollama address
- Verify the `llama3` model is available: `ollama list`

## 📝 Workflow Logic

The workflow follows this execution flow:

1. **Chat Trigger** receives a user message
2. **AI Agent** processes the message using Ollama's Llama3 model
3. If needed, agent calls **Wikipedia Tool** to search for information
4. **Window Buffer Memory** maintains context across conversations
5. Response is sent back to the chat interface

## 🛠️ Development

To modify the workflow:

1. Open `personal_agent_workflow.json` in a text editor
2. Make changes to node parameters or connections
3. Export the updated workflow from n8n
4. Commit and push changes:
   ```bash
   git add personal_agent_workflow.json
   git commit -m "Update workflow: [description]"
   git push
   ```

## 📦 Dependencies

- **n8n**: Workflow automation platform
- **Ollama**: Local LLM runtime
- **LangChain**: AI agent orchestration framework
- **Wikipedia API**: Knowledge retrieval tool

## 📄 License

This project is provided as-is for educational and personal use.

## 🤝 Contributing

Contributions are welcome! Please feel free to:
- Report issues
- Submit pull requests
- Suggest improvements

---

**Last Updated**: April 26, 2026  
**Status**: Active Development
