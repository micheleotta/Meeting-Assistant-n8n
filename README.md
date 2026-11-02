# Meeting Assistant
Automation project developed with n8n to process meeting transcripts using a Large Language Model (LLM), generate structured summaries, identify decisions, tasks, and pending issues, store the results in Google Sheets, and send them via email.

## Objective

Meeting Assistant aims to automate the organization of information discussed during meetings, reducing the manual effort required to create meeting minutes and track action items.

The application receives a transcript file, uses a language model to interpret its content, and organizes the extracted information into a standardized structure.

## Features

- **Transcript upload:** submit `.txt` files through a form.
- **Summary generation:** summarize the main topics discussed.
- **Topic extraction:** identify the subjects covered during the meeting.
- **Decision identification:** record decisions that were actually made.
- **Task extraction:** identify action items, their assignees, and deadlines.
- **Pending issue identification:** record unresolved questions and topics.
- **Data storage:** save the results in Google Sheets.
- **Email delivery:** automatically send the meeting summary to the email address provided in the form.

## Stack

| Technology | Purpose |
|---|---|
| n8n | Workflow orchestration and automation |
| `gemini-3.8-flash` | Transcript processing using an LLM |
| Google Sheets | Data storage |
| Gmail | Email delivery |
| JavaScript | Data manipulation and structuring in Code nodes |

## Workflow Architecture

The workflow consists of the following steps:

1. **Form Trigger:** receives the meeting transcript file and the recipient's email address.
2. **Extract From File:** extracts the text content from the uploaded file.
3. **Edit Fields:** organizes the input data, including the transcript, filename, processing date, and recipient email.
4. **HTTP Request:** sends the transcript to the Google Gemini API for processing.
5. **Code:** parses the model response, converts the JSON content, and organizes the extracted fields.
6. **Google Sheets:** stores the structured information.
7. **Gmail:** sends the meeting summary to the email address provided in the form.

<img width="1358" height="415" alt="image" src="https://github.com/user-attachments/assets/72082351-1ef0-4ebe-98db-d389149bdf79" />


## Data Structure

The language model is expected to return information in JSON format, containing the following fields:

| Field | Description |
|---|---|
| `resumo` | General meeting summary |
| `topicos` | Main topics discussed |
| `decisoes` | Decisions actually made |
| `tarefas` | Identified tasks, including description, assignee, and deadline |
| `pendencias` | Unresolved issues |

In addition to these fields, the workflow maintains the following metadata:

| Field | Description |
|---|---|
| `data_processamento` | Processing date and time |
| `nome_arquivo` | Name of the uploaded file |
| `email_destino` | Email address provided in the form |

### Interpretation Rules

To reduce incorrect interpretations and fabricated information, the processing follows these rules:

- Do not invent information that is not present in the transcript.
- Distinguish suggestions from decisions that have actually been approved.
- Do not assign tasks to participants without evidence in the transcript.
- If the assignee is not identified, use `"Not provided"`.
- If the deadline is not identified, use `"Not provided"`.
- Record unresolved questions as pending issues.
- Preserve contradictions and ambiguities found in the transcript instead of arbitrarily choosing one interpretation.

## Requirements

To run the workflow, the following are required:

- An n8n instance.
- A Google Gemini API key.
- A Google account with access to Google Sheets.
- A Google account configured for sending messages through Gmail.

Configure the following integrations in n8n:
- Google Gemini API
- Google OAuth2 credentials.

## Demonstration
### Mail sent
<img width="1231" height="648" alt="image" src="https://github.com/user-attachments/assets/ef07cc8c-7b9f-4b86-94e5-ea69459e8ea1" />

