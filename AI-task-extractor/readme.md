# Email Task Intelligence: Automated Extraction

Managing a busy inbox often feels like a full-time job. This project introduces the **AI Task Extractor**, an automated n8n workflow that monitors your incoming emails, uses Large Language Models (LLMs) to identify action items, and organizes them into a structured Google Sheet.

It is designed to turn unstructured natural language into actionable data, ensuring no deadline or request slips through the cracks.

---

## Project Requirements
To deploy this workflow effectively, you will need:

* **n8n Instance**: Either self-hosted (Docker/npm) or an n8n Cloud account.
* **Google Cloud Console Access**: Required to enable the Gmail and Google Sheets APIs and generate OAuth2 credentials.
* **Google Gemini API Key**: To power the "brain" of the extraction process.
* **Active Google Sheet**: A spreadsheet pre-formatted with headers for *Task*, *Duration*, *Priority*, and *End Date*.

---

## Dependencies
The workflow utilizes specific nodes and frameworks to handle the automation lifecycle:

1.  **Gmail Trigger Node**: Periodically polls your inbox for new messages.
2.  **Google Gemini (PaLM) Node**: The specific LLM provider used to interpret and categorize tasks.
3.  **n8n Code Node**: A JavaScript-based processor that cleans AI-generated strings and prepares them for database entry.
4.  **Google Sheets Node**: The final destination for the structured data.

---

## Getting Started
To get this workflow running, follow these configuration steps:

1.  **Import the JSON**: Copy the provided JSON schema and paste it directly into your n8n workflow canvas.
2.  **Credential Setup**:
    * Link your **Gmail account** to the Trigger node.
    * Link your **Google Gemini API Key** to the Chat Model node.
    * Authorize your **Google Sheets account** for the Storage node.
3.  **Target Spreadsheet**: Open the "Store Tasks in Sheet" node and select your specific "To-Do List" spreadsheet from the dropdown menu.

---

## How to Run the Application
The workflow is designed to be "set and forget."

* **Test Run**: Click "Execute Workflow" and send yourself a test email containing a sentence like: *"Please finish the project report by Friday and set the priority to High."*
* **Verify**: Check your Google Sheet to see the task appear as a new row with the extracted fields.
* **Activate**: Once tested, click the **Publish** button in the top right of the n8n UI. The system will now check for new emails every minute.

---

## Relevant Code Examples

### The Extraction Logic
Inside the `AI: Extract Tasks` node, we use a specific prompt to ensure the AI returns data in a predictable format.
### Prompt
``` 
Extract all tasks mentioned in this email.
For each task, return a JSON object with:
- task
- duration
- priority
- end_date
```

### Data Sanitization (JavaScript)

The AI sometimes wraps its JSON response in Markdown code blocks (e.g., \`\`\`json ... \`\`\`). The **Parse AI Output** node uses the following logic to clean that text so n8n can read it as a native object:

```javascript
// Remove markdown wrappers and trim whitespace
const aiOutput = item.json.output.replace(/```json|```/g, '').trim();

// Convert the string back into a usable JavaScript array
try {
    const tasks = JSON.parse(aiOutput);
    return tasks.map(t => ({ json: t }));
} catch (error) {
    console.log("Parsing error:", error);
}
```
This ensures that even if the AI is "chatty," the final data sent to your spreadsheet remains clean and error-free.

---

# Conclusion

The **AI Task Extractor** represents a significant step toward a truly autonomous personal assistant. By moving task management out of your brain and into an automated system, you can focus on execution rather than organization.
