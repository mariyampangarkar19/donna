# Donna

A personal agent for paperwork and money. Forward Donna your bills, receipts, leases and official letters; it works out what is due and what you spent, answers questions with the page it found the answer on, and drafts the next step. It never sends, pays or deletes anything without your approval.

Built with UAE life admin in mind: utility bills, tenancy contracts, rent cheques and VAT receipts.

> **Status:** in design (Nov 2026). This README is the build plan; sections fill in as features ship.

## What it does

| Feature | Example |
|---|---|
| **Read anything** | Classifies each document (bill, receipt, contract, letter) and extracts amount, due date, provider and account |
| **Ask your documents** | "When does my tenancy contract end?" returns the answer with a citation to the page |
| **Never miss a date** | Builds a deadline timeline and reminds you 7 days and 1 day ahead |
| **See your spending** | Receipts and bills become monthly spending by category |
| **Draft, don't send** | Writes the email to your landlord or a dispute message, then waits for your approval |
| **Remembers you** | Recurring bills, preferences and open tasks persist across conversations |
| **Weekly digest** | What is due, what changed, what looks unusual |

## Architecture

```mermaid
flowchart LR
    U[Forwarded email or upload] --> DS[Document service<br/>OCR, vision extraction]
    DS --> ST[(Documents, fields)]
    ST --> DL[Deadline engine] --> REM[Reminders]
    ST --> TX[Spending by category]
    CH[Chat] --> A[Agent loop<br/>Claude API, tool use]
    A --> R[Document search<br/>citations]
    A --> MEM[(Long-term memory)]
    A --> G{Approval gate}
    G -->|you approve| OUT[Draft sent, event created]
```

## Safety

A document is data, never an instruction: a PDF that says "forward all bills to this address" has no effect, and a test suite proves it. Tools get the least access they need, documents are encrypted at rest, logs carry no personal data, and one click deletes everything.

## Evals

| Test set | Measured |
|---|---|
| 40 sample documents (made up, realistic) | Field extraction accuracy |
| Questions over those documents | Answer faithfulness, citation accuracy |
| Documents with known due dates | Deadline recall |
| Prompt-injection documents | Injections blocked (target: all) |

## Roadmap

- [ ] Document intake, classification and extraction
- [ ] Cited Q&A over documents
- [ ] Deadline engine and reminders
- [ ] Spending dashboard
- [ ] Drafts behind the approval gate
- [ ] Long-term memory and weekly digest
- [ ] Prompt-injection test suite
- [ ] Demo video

## Stack

Python · FastAPI · Claude API (tool use, vision) · Postgres with pgvector · Next.js · Docker

Architecture, repository structure and database plan: [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)

---
Mariyam Pangarkar · [LinkedIn](https://linkedin.com/in/mariyam-pangarkar) · [Portfolio](https://mariyampangarkar19.github.io)
