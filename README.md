[Github Mcp Server](https://apify.com/10emmmm/github-mcp-server?fpr=data)

# GitHub MCP Server 🐙

This Apify Actor implements the **Model Context Protocol (MCP)** for the GitHub API. It creates a secure bridge between your AI agents (like Claude Desktop, Cursor, or custom agents) and your private or public GitHub repositories.

**Why use this?**

- **Give Claude "Sight":** Let your AI read your private code to write bug fixes.
- **Agentic Workflows:** Enable agents to search for repos, read issues, and understand your codebase structure without cloning locally.
- **Zero Ops:** Runs in the cloud via Apify. No need to host your own local MCP server.

## 🚀 Features

- **⚡ Real-time SSE:** Fully supports Server-Sent Events for instant, low-latency communication.
- **🔒 Secure & Private:** Works with your own Fine-grained Personal Access Tokens. Your code stays yours.
- **🔎 Deep Search:** Find repositories precisely using GitHub's advanced query syntax.
- **📂 File Access:** Fetch raw file content up to 100MB.
- **🐞 Issue Tracking:** List open/closed issues to help agents prioritize work.
- **💰 Cost Efficient:** Designed for "Pay-per-Event" (Apify Standby Mode) - pay only when you chat.

## 📦 Tools Provided

| Tool Name | Description | Arguments |
| --- | --- | --- |
| `search_repositories` | Search all of GitHub. | `query` (string), `per_page` (number) |
| `get_file_content` | Get raw content of a file. | `owner` (string), `repo` (string), `path` (string) |
| `list_issues` | List repository issues. | `owner` (string), `repo` (string), `state` (enum: open, closed, all) |

## 🛠️ Usage

### 1. Prerequisites

- An Apify Account.
- A **GitHub Personal Access Token (PAT)** (Classic or Fine-grained) with `repo` scope.

### 2. Configuration (Input)

When starting the Actor, provide the following input:

```
{
    "githubToken": "ghp_your_secret_github_token...",
    "port": 8000
}
```

### 3. Connection URL

Once running, the Actor will provide a **Standby URL** (accessible via the "Standby URL" button in the Apify Console).

Your MCP Connection URL will be:

`https://<your-actor-id>.apify.actor/sse`

## 🔌 How to Connect

### 1. For SSE-Compatible Clients (e.g., Claude Desktop, MCP Inspector)

Most modern MCP clients support SSE directly. Use the **Standby URL** provided in your Apify Console.

**Config Example:**

```
{
  "mcpServers": {
    "github-apify": {
      "url": "https://<your-username>--github-mcp-server.apify.actor/sse?token=<your-token>",
      "transport": "sse"
    }
  }
}
```

### 2. For Stdio-Only Clients (e.g., Cursor, Antigravity)

Some IDEs only support local commands. You can use our **Bridge** to connect these clients to the cloud.

1. Create a file named `bridge.js` (copy it from this repo's `dist/bridge.js`).
2. Add this to your config:

**Config Example:**

```
{
  "mcpServers": {
    "github-apify": {
      "command": "node",
      "args": ["/path/to/bridge.js"]
    }
  }
}
```

---

## 🛡️ Security & Permissions

- **Fine-grained Tokens:** We recommend using GitHub's Fine-grained Tokens and selecting only the repositories you want your AI to access.
- **Environment Variables:** For maximum security, skip the `githubToken` input and set it as an Environment Variable (`GITHUB_TOKEN`) in the Apify Actor settings.

## 💰 Pricing

This Actor is priced **Pay Per Event** to ensure fair usage billing.

## 👨‍💻 Development

Built with:

- [Model Context Protocol SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [Octokit](https://github.com/octokit/octokit.js)
- [Apify SDK](https://docs.apify.com/sdk/js)

## 📄 License

MIT