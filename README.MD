# Terminal agent + prompts = n8n Automation Workflows


This repo contains **high-precision AI agent prompts** and a library of **ready-to-import n8n workflow JSON files**.

The goal is to make it easy to:
1. Use a terminal AI agent as a **workflow engineer** to build production-ready n8n automations in JSON format.
2. Validate and harden those workflows with dedicated **QA** and **Security** agents.
3. Store and version-control both the **prompts** (brains) and the **workflows** (artifacts).
4. Side note: This is a small personal project aimed to automated automations ( yes you read that right ) improvements will be actively made to the prompts and is not stable yet.

---

## Repository Structure
```
n8n/
├── prompts/
│   ├── n8n_Build_Captain.md
│   ├── n8n_QA_Compliance.md
│   └── n8n_Security_Architect.md
│
├── workflows/
│   ├── workflow-1.json
│   ├── workflow-2.json
│
├── samples/
│   ├── n8n_sample.json
│   └── (future ones samples here)
│
└── README.md
```


---

### `prompts/`
Contains **three specialized Claude Code system prompts**:

1. **n8n_Build_Captain.md**  
   - The *orchestrator* agent.  
   - Builds full n8n workflows from a plain-language spec.  
   - Outputs valid, import-ready JSON + README + sample payloads.

2. **n8n_QA_Compliance.md**  
   - The *quality assurance* agent.  
   - Validates JSON importability, node conventions, error handling, and adherence to n8n best practices.  
   - Returns targeted JSON patches and test plans.

3. **n8n_Security_Architect.md**  
   - The *security review* agent.  
   - Identifies risks in workflow design (auth, PII, DoS, SSRF, etc.).  
   - Suggests and provides JSON fragments for hardening.

---

### `workflows/`
- This is where **all importable `.json` n8n workflows** live.
- Each JSON file should be valid for **direct import** into n8n via UI or CLI.
- Filenames should be descriptive (`verb-object.json`) so they can be understood at a glance.

---

### `samples/`
- Stores **canonical "reference" exports** for n8n workflows — known-good examples that follow best practices and pass QA/Security reviews.
- These samples are not production workflows (skeleton); they exist to **prime AI agents** and serve as schema/style references.
- Example: `n8n_sample.json` contains a minimal but complete pattern (Trigger → HTTP → Transform → Respond + Error handling).
- If you expand to other platforms (Make.com, Zapier, etc.), their samples can also live here, using their respective formats.

---

## How to Use the Agents

1. **Open three Claude Code chats**:
   - **Chat A** → Paste `n8n_Build_Captain.md` (Build Captain)
   - **Chat B** → Paste `n8n_QA_Compliance.md` (QA & Compliance)
   - **Chat C** → Paste `n8n_Security_Architect.md` (Security Architect)

2. **Feed your automation spec** (trigger, data flow, APIs, credentials) into Chat A.  
   The Build Captain will produce:
   - `WORKFLOW.json`
   - `README.md` (workflow-specific)
   - Sample payloads

3. **Send the output** from Chat A to:
   - **Chat B** → for QA & Compliance review
   - **Chat C** → for Security review

4. **Apply patches** from QA & Security, then have Build Captain produce a **final JSON**.

5. **Save final workflow JSON** into `/workflows/` and commit to repo.

---

## Importing into n8n

**UI Method:**
- Go to the n8n Editor UI → **Import from File** → Select the `.json` file.

**CLI Method (self-hosted):**
```bash
n8n import:workflow --input=./workflows/your-workflow.json
