# Autonomous Restaurant Concierge: Multi-Agent AI Orchestration

An enterprise-grade, asynchronous **Hierarchical Multi-Agent System (HMAS)** designed to automate 24/7 restaurant operations, customer service, and kitchen notification workflows. Built inside **n8n**, the system orchestrates a network of specialized LLM instances to interpret natural language requests, dynamically interface with a spreadsheet-based data store, and trigger secure, automated operational alerts.

## 📊 System Architecture

The core architecture transitions away from linear, brittle "If-Then" fallback loops common in legacy bots. Instead, it implements a **Hierarchical Orchestration pattern with Tool Delegation** to optimize the available context window and avoid semantic drift.

```mermaid
graph TD
    User([Chat Trigger Interface]) --> ChatAgent[Chat Agent: Main Orchestrator <br> Gemini 2.5 Flash / Buffer: 10 msg]
    
    %% Specialist Delegation
    ChatAgent -->|Query Menu| MenuAgent[Menu Assistant Agent <br> Memory Buffer: 5 msg]
    ChatAgent -->|Validate Data| OrderAgent[Order Processing Agent <br> Memory Buffer: 5 msg]
    ChatAgent -->|Dispatch Alerts| AlertAgent[Notification Agent <br> Memory Buffer: 5 msg]
    
    %% Tool Layer
    MenuAgent -->|Fetch Schema| Tool1[(Google Sheets API: <br> restaurant-menus)]
    OrderAgent -->|Append JSON Payload| Tool2[(Google Sheets API: <br> Order Log)]
    AlertAgent -->|Asynchronous SMTP| Tool3[Gmail API: <br> Chef Notification Hub]
    
    classDef orchestrator fill:#0E6251,stroke:#117A65,stroke-width:2px,color:#fff;
    classDef agent fill:#1B4F72,stroke:#21618C,stroke-width:2px,color:#fff;
    classDef tool fill:#626567,stroke:#4D5656,stroke-width:2px,color:#fff;
    
    class ChatAgent orchestrator;
    class MenuAgent,OrderAgent,AlertAgent agent;
    class Tool1,Tool2,Tool3 tool;
