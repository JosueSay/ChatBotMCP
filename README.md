# Chatbot (CLI) · MCP Client

Console-based chatbot that connects to **Anthropic Claude** and can call tools exposed by **Model Context Protocol (MCP)** servers (e.g., a local FEL server).  
Maintains conversational context, can auto/manuelly call tools, and logs sessions as structured JSONL.

## 🔗 Related repositories

- [MCP FEL (Local)](https://github.com/JosueSay/MCPLocalFEL) — MCP server used by this client (validate/render/batch FEL).
- [Reference: OpenAI Chat API Example](https://github.com/JosueSay/Selectivo_IA/blob/main/docs_assistant/README.md) — reference only (API connectivity & instruction patterns).

## ✨ Features

- Anthropic Claude API client (streaming, context carry-over).
- **Tool calling** (manual `/tools` & automatic) via MCP.
- Session logs in **JSONL**.
- Works with local MCP via **STDIO** (WSL) or remote MCP via URL.

## ⚙️ Requirements

- **Python 3.12**
- **WSL Ubuntu 22.04** (or Linux)
- Virtualenv recommended

## 🔧 Installation

```bash
git clone <REPO_URL>
cd <REPO_DIR>

python3.12 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
````

Create your env file:

```bash
cp .env.example .env
```

Use **placeholders** and **absolute WSL paths** for MCP:

```env
# ---- Antrhopic ----
ANTHROPIC_API_KEY=your_key
ANTHROPIC_MODEL=claude-sonnet-4-20250514 # optional other model https://docs.anthropic.com/en/api/models-list

# ---- MCPS ----
MCP_FEL_CMD="/ABSOLUTE/PATH/venv/bin/python /ABSOLUTE/PATH/servers/fel_mcp_server/server_stdio.py"
MCP_FEL_URL=url_mcp # pending
ROUTER_DEBUG=0 # 1: active 0:deactive -> debug chat reasoning

# ---- Chat Logs ----
LOG_DIR=/ABSOLUTE/PATH/TO/REPO/data/logs/sessions
```

## 🚀 Usage

```bash
python apps/cli/chat.py
```

You’ll see:

```bash
──────────── Chatbot (CLI) + MCP ────────────
commands: /exit | /clear | /tools
You ›
```

### Discover tools (from connected MCP servers)

```bash
You › /tools
```

### Example prompts (FEL server)

**Validate XML:**

```bash
Call the MCP tool FEL:fel_validate with EXACTLY this JSON:
{"xml_path":"/ABSOLUTE/PATH/TO/REPO/data/xml/factura.xml"}
```

**Render PDF (no logo):**

```bash
Call the MCP tool FEL:fel_render with EXACTLY this JSON:
{"xml_path":"/ABSOLUTE/PATH/TO/REPO/data/xml/factura.xml","out_path":"/ABSOLUTE/PATH/TO/REPO/data/out/testing.pdf"}
```

**Batch:**

```bash
Call the MCP tool FEL:fel_batch with EXACTLY this JSON:
{"dir_xml":"/ABSOLUTE/PATH/TO/REPO/data/xml","out_dir":"/ABSOLUTE/PATH/TO/REPO/data/out/batch"}
```

> Always use **absolute WSL paths** (`/mnt/...`) when running on Windows via WSL.

## 📝 Logs

- Stored in `LOG_DIR` as **JSONL** (one object per event).
- Useful to trace **tool usage**, **model decisions**, and **errors**.

## 📚 References

- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Anthropic API](https://docs.anthropic.com/en/api)
- [JSON-RPC 2.0](https://www.jsonrpc.org/)
