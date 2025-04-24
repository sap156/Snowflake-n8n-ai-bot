# 🧠 Snowflake ChatBot: AI Agent via Slack + OpenAI + n8n

![Snowflake ChatBot Architecture]
<img width="746" alt="Screenshot 2025-04-23 at 10 06 19 PM" src="https://github.com/user-attachments/assets/f18a4766-8d1e-4ff9-86cd-2147f63df2d8" />


This is a demo project where I built an **AI-powered Slack bot** that connects to **Snowflake**, receives messages through **Slack**, generates SQL queries via **OpenAI**, and returns insights on **data quality checks (DQC)** — all inside `n8n`.

---

## 🚀 Features

- 🤖 AI Agent built using OpenAI (`gpt-4o`)
- 🔗 Connected to **Snowflake** to run live SQL queries
- 💬 Integrated with **Slack** using Slack triggers and message posts
- 🔍 Queries a **DQC_RESULTS** table to answer:
  - “What data quality rules failed today?”
  - “How many errors were detected?”
  - “Show me passed checks on the CUSTOMERS table”
- ⚙️ All powered by [n8n](https://n8n.io), a low-code automation platform

---

## 📦 Architecture

1. **Slack Trigger (`Get Message`)**
   - Listens for Slack mentions (e.g. `@SnowBot show DQ failures`)
2. **n8n Agent Node**
   - Uses an OpenAI-powered agent with memory and tool capabilities
   - Understands natural language and decides what SQL to run
3. **Tool: `query_snowflake`**
   - Executes the generated SQL on the Snowflake database
   - Uses the `DQC_RESULTS` table
4. **Slack Post (`Send Message`)**
   - Returns the summarized insights back into Slack

---

## 📄 DQC Table Structure

```sql
CREATE OR REPLACE TABLE BANKING_DEMO.BANKING.DQC_RESULTS (
  CHECK_ID NUMBER,
  TABLE_NAME VARCHAR,
  RULE_NAME VARCHAR,
  RULE_DESCRIPTION VARCHAR,
  STATUS VARCHAR, -- PASS or FAIL
  ERROR_COUNT NUMBER,
  EXECUTED_AT TIMESTAMP_NTZ
);
```

---

## 🧠 Example Prompt (System Message in Agent)

> You are a Data Quality (DQC) assistant.  
> You receive natural language questions and respond using real-time data from a Snowflake database.  
> You can call a tool named `query_snowflake(sql: string)` to run SQL queries...  
> Always summarize results in plain English.

---

## 🛠️ How to Use

1. Clone the repo and import the JSON into your n8n instance
2. Configure the following credentials:
   - ✅ Slack API
   - ✅ OpenAI API Key
   - ✅ Snowflake account
3. Add the bot to a Slack channel
4. Trigger it with `@YourBot what DQ rules failed today?`

---

## 🔮 Future Enhancements

- 📈 Return charts or formatted Slack blocks
- 📅 Add scheduling (e.g., “send daily summary at 9AM”)
- 🧠 Enable tool memory and conversation chaining

