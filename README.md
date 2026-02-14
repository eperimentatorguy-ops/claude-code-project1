# N8N Poet Email Agent

An n8n workflow that sends a beautifully formatted email with a poem from a famous author born in the current month, triggered by a form submission.

## Workflow Overview

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

## Setup Instructions

### 1. Import the Workflow

1. Open your n8n instance
2. Go to **Workflows** → **Add workflow** (or use the import option)
3. Click the **⋮** menu → **Import from file**
4. Select `n8n-poet-email-agent.json`

### 2. Configure Credentials

You need to set up two credentials:

#### OpenAI API
1. Go to **Settings** → **Credentials** → **Add Credential**
2. Search for **OpenAI API**
3. Enter your OpenAI API key
4. Save, then select this credential in the **OpenAI Chat Model** node

#### SMTP (Email)
1. Go to **Settings** → **Credentials** → **Add Credential**
2. Search for **SMTP**
3. Configure with your email provider's SMTP settings:
   - **Host**: e.g., `smtp.gmail.com`
   - **Port**: e.g., `465` (SSL) or `587` (TLS)
   - **User**: your email address
   - **Password**: your email password or app-specific password
4. Save, then select this credential in the **Send Poem Email** node

### 3. Activate the Workflow

1. Toggle the workflow to **Active**
2. The form will be available at the URL shown in the Form Trigger node
3. Share the form URL with anyone who wants to receive a poem

## How It Works

1. A user fills out the form with their **name** and **email**
2. The workflow determines the **current month**
3. The AI Agent identifies a **famous poet/author born that month** and selects a **real poem**
4. The response is parsed into structured fields (author, birth date, poem title, poem text, fun fact)
5. A styled HTML email is sent to the user with the poem and author information

## Example Output

For February, you might receive a poem by **Edna St. Vincent Millay** (born February 22, 1892) or **Langston Hughes** (born February 1, 1902).

## Customization

- **Change the LLM**: Swap the OpenAI Chat Model node for Anthropic, Ollama, or any other supported model
- **Modify the email template**: Edit the HTML in the Send Poem Email node
- **Add more form fields**: Add fields like "Preferred language" or "Mood" to the form trigger
- **Change the prompt**: Adjust the AI Agent prompt to request specific types of poems (sonnets, haikus, etc.)
