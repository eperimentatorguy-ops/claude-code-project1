# N8N Agent Workflows

A collection of n8n workflows for AI-powered agent orchestration.

---

## 1. Orchestrator Agent (Project Manager)

An AI-powered orchestrator that acts as a project manager, coordinating Research and Strategy sub-agents to deliver comprehensive results from a single goal.

### Workflow Overview

```
Chat Trigger
    │
    ▼
Orchestrator Agent (Claude) ◄── Window Buffer Memory
    │
    ├──► Research Agent Tool  (calls sub-workflow)
    └──► Strategy Agent Tool  (calls sub-workflow)
```

### Nodes

| Node | Purpose |
|------|---------|
| **Chat Trigger** | Receives the user's goal or task via n8n chat interface |
| **Orchestrator Agent** | Claude-powered project manager that decomposes goals, delegates to sub-agents, and synthesizes final output |
| **Anthropic Chat Model** | Claude Sonnet powering the orchestrator's reasoning |
| **Research Agent Tool** | Calls your Research Agent sub-workflow for data gathering and investigation |
| **Strategy Agent Tool** | Calls your Strategy Agent sub-workflow for planning and recommendations |
| **Window Buffer Memory** | Maintains conversation context across multiple chat turns (20-message window) |

### How It Works

1. You send a **goal or task** via the chat interface (e.g., "Analyze the competitive landscape for AI writing tools and recommend a go-to-market strategy")
2. The orchestrator **breaks the goal into sub-tasks** and determines execution order
3. It calls the **Research Agent** first to gather data, facts, and context
4. It then calls the **Strategy Agent** with the research findings to produce an informed plan
5. The orchestrator **synthesizes everything** into a structured deliverable with:
   - Executive summary
   - Research findings
   - Strategic recommendations
   - Action items

### Setup Instructions

#### 1. Import the Workflow

1. Open your n8n instance
2. Go to **Workflows** → **Add workflow** → **⋮** menu → **Import from file**
3. Select `n8n-orchestrator-agent.json`

#### 2. Configure the Anthropic Credential

1. Go to **Settings** → **Credentials** → **Add Credential**
2. Search for **Anthropic API**
3. Enter your Anthropic API key
4. Save, then select this credential in the **Anthropic Chat Model** node

#### 3. Link Your Sub-Agent Workflows

This is the key step — you need to point the tool nodes to your existing agent workflows:

1. Open the **Research Agent Tool** node → set **Workflow** to your Research Agent workflow
2. Open the **Strategy Agent Tool** node → set **Workflow** to your Strategy Agent workflow

Each sub-workflow should accept input parameters and return results. The tool nodes are configured to pass these fields:

**Research Agent Tool inputs:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `task` | string | Yes | The research question or investigation topic |
| `context` | string | No | Additional context or constraints |
| `depth` | string | No | `quick`, `standard`, or `deep` |

**Strategy Agent Tool inputs:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `objective` | string | Yes | The strategic objective to address |
| `research_findings` | string | No | Research data to inform the strategy |
| `constraints` | string | No | Budget, timeline, or other constraints |

#### 4. Activate & Use

1. Toggle the workflow to **Active**
2. Open the chat interface (click **Chat** in the workflow canvas)
3. Type your goal and the orchestrator will coordinate the agents

### Customization

- **Change the LLM**: Swap the Anthropic node for OpenAI, Ollama, or any other supported model
- **Add more sub-agents**: Duplicate a Tool Workflow node, rename it, point it to another workflow, and update the orchestrator system prompt to describe the new agent
- **Adjust the system prompt**: Edit the Orchestrator Agent's system message to change coordination behavior, output format, or tone
- **Memory window**: Adjust `contextWindowLength` in the Window Buffer Memory node (default: 20 messages)

---

## 2. Poet Email Agent

An n8n workflow that sends a beautifully formatted email with a poem from a famous author born in the current month, triggered by a form submission.

### Workflow Overview

```
Form Trigger → Get Current Month → AI Agent (find poet & poem) → Parse Response → Send Email
```

### Nodes

| Node | Purpose |
|------|---------|
| **Form Trigger** | Collects recipient's name and email via a web form |
| **Get Current Month** | Determines the current month and passes form data along |
| **AI Agent** | Uses an LLM to find a famous poet born this month and select one of their poems |
| **OpenAI Chat Model** | Language model powering the AI Agent |
| **Parse AI Response** | Extracts author, poem title, poem text, and fun fact from the AI output |
| **Send Poem Email** | Sends a styled HTML email with the poem to the recipient |

### Setup Instructions

#### 1. Import the Workflow

1. Open your n8n instance
2. Go to **Workflows** → **Add workflow** → **⋮** menu → **Import from file**
3. Select `n8n-poet-email-agent.json`

#### 2. Configure Credentials

**OpenAI API:**
1. Go to **Settings** → **Credentials** → **Add Credential**
2. Search for **OpenAI API** → enter your API key → Save
3. Select this credential in the **OpenAI Chat Model** node

**SMTP (Email):**
1. Go to **Settings** → **Credentials** → **Add Credential**
2. Search for **SMTP** → configure with your email provider's settings (host, port, user, password)
3. Select this credential in the **Send Poem Email** node

#### 3. Activate the Workflow

1. Toggle the workflow to **Active**
2. The form will be available at the URL shown in the Form Trigger node
3. Share the form URL with anyone who wants to receive a poem

### How It Works

1. A user fills out the form with their **name** and **email**
2. The workflow determines the **current month**
3. The AI Agent identifies a **famous poet/author born that month** and selects a **real poem**
4. The response is parsed into structured fields (author, birth date, poem title, poem text, fun fact)
5. A styled HTML email is sent to the user with the poem and author information

### Customization

- **Change the LLM**: Swap the OpenAI Chat Model node for Anthropic, Ollama, or any other supported model
- **Modify the email template**: Edit the HTML in the Send Poem Email node
- **Add more form fields**: Add fields like "Preferred language" or "Mood" to the form trigger
- **Change the prompt**: Adjust the AI Agent prompt to request specific types of poems (sonnets, haikus, etc.)
