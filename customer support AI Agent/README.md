# 🤖 Customer Support AI: Intelligent Email Automation with RAG

This project transforms a standard Gmail inbox into an intelligent, self-operating support desk. By combining the power of **Large Language Models (LLMs)** with **Retrieval-Augmented Generation (RAG)**, this n8n workflow identifies customer needs, retrieves technical documentation from a vector database, and drafts high-quality responses—all while keeping your team informed in real-time. 🚀

It is designed to bridge the gap between unstructured customer inquiries and expert-level technical support.

---

## 🛠️ Project Requirements
To successfully deploy and run this workflow, ensure your environment meets the following criteria:

* **n8n Version**: 1.0+ (Self-hosted or Cloud). 🔄
* **Supabase Account**: A database with the `pgvector` extension enabled for storing agency knowledge. 🗄️
* **OpenAI API Key**: Access to `gpt-4o` or `gpt-4-mini` models. 🧠
* **Gmail/Telegram**: Verified accounts with OAuth2 and Bot API credentials respectively. 📧📱

---

## 🏗️ Dependencies
This architecture is built on a **"Hybrid Agentic Stack"** that balances speed with reasoning depth:

1.  **OpenAI GPT-4o**: The primary reasoning engine for classification and email drafting.
2.  **Supabase Vector Store**: Acts as the "long-term memory" containing your agency's service details and FAQs. 💾
3.  **n8n Text Classifier**: A specialized node that filters out promotional noise before the agent begins its work. 🧹

---

## 🏁 Getting Started
Since the workflow schema is already provided, setting up the logic is a matter of configuration rather than manual node placement.

1.  **Import the Canvas**: Copy the provided JSON and paste it into a new n8n workflow. 📋
2.  **Establish Connections**: Update the credentials for **Gmail**, **OpenAI**, **Supabase**, and **Telegram**. 🔑
3.  **Knowledge Injection**: Upload your agency’s PDF or text documentation into your Supabase `rag_example` table. This is the information the AI will use to "study" before answering queries. 📚

---

## ⚡ How to Run the Application
The workflow is designed to be fully autonomous once the **"Active"** toggle is switched on.

* **The Trigger**: The system polls the Gmail inbox every 60 seconds for new messages. 🕒
* **The Gatekeeper**: The **Email Classification** node analyzes the snippet. If it's marketing or spam, the workflow stops. If it's a support inquiry, it proceeds. 🛡️
* **The Reasoning Phase**: The AI Agent queries the **Supabase Vector Store** to find the most relevant answers to the user's specific question. 🔍
* **The Output**: A draft is created in Gmail (for human review before sending), and a Telegram notification is sent to the team. 📩

---

## 💻 Relevant Code Examples

### Email Classification Logic
The classifier ensures the AI doesn't waste resources on newsletters. We define specific categories for the model to distinguish between promotional content and actual service requests:

```json
// Internal configuration for the Text Classifier
categories: [
    {
        category: "promotional",
        description: "Marketing, offers, discounts, or newsletters."
    },
    {
        category: "support_service",
        description: "Customer inquiries, troubleshooting, or help requests."
    }
]
```
### 📝 Agent System Instruction

The agent follows a strict set of rules to ensure accuracy. It is instructed to always check internal data before making a claim to the customer. 🛡️
```
# Rules
- ALWAYS use Supabase Vector Store before answering.
- ALWAYS draft a concise email response.
- Maintain a helpful, professional structure.

# Personalization
- Sign off as: Asad
- Agency: SOMS Media
```

# 🏁 Conclusion

The **Support AI** workflow bridges the gap between passive automation and active intelligence. By utilizing a vector store, the agent provides answers based on your **actual data**, not just general LLM knowledge. 🧠✨

This template is highly extensible—feel free to:
* **Swap Supabase for Pinecone** 🌲
* **Swap OpenAI for Claude** 🎭
* **Modify the logic** to see how different models handle your specific customer base.

If you have questions about the implementation or want to suggest an improvement, please reach out via the **issue tracker**! 🛠️🤝
