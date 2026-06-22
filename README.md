[Github Mcp Server](https://apify.com/zerobudget/github-mcp-server?fpr=data)

# GitHub MCP Server — Intelligent Developer Operations for AI Agents

The most comprehensive GitHub MCP server available. Not just another API wrapper — this is an **intelligence layer** that gives AI agents superpowers for understanding, managing, and improving codebases.

**75+ tools** spanning full repository lifecycle (create/update/delete repos, fork, star, transfer), **batch file commits** (`github_create_tree_and_commit`), file CRUD, branches & protection, PR reviews & merge, global search (issues, users, topics), organizations & collaborators, Actions secrets (libsodium-sealed), Dependabot alerts, webhooks, deploy keys, notifications, watching, gists, optional **GraphQL** (Projects V2, Discussions when `enableGraphQL` is true), GitHub Actions workflows, releases, plus the **intelligence layer** below.

## Why This Is Different

Every GitHub MCP server out there wraps basic CRUD: create an issue, read a PR, list repos. That's table stakes.

This server adds an **intelligence layer** on top:

- **`github_repo_health_check`** scores your repository 0–100 across documentation, maintenance, community, CI/CD, and security — then tells you exactly what to fix.
- **`github_pr_risk_analysis`** evaluates pull requests for risk factors: size, critical files touched, missing tests, breaking changes, reviewer coverage.
- **`github_contributor_insights`** reveals your bus factor, review load distribution, and expertise mapping so you know where knowledge concentration risk lives.
- **`github_issue_triage`** auto-categorizes and prioritizes open issues, flags stale tickets, detects duplicates, and identifies good-first-issue candidates.
- **`github_codebase_overview`** lets an AI agent "understand" any repository in 60 seconds: architecture pattern, tech stack, entry points, test coverage.
- **`github_dependency_check`** analyzes your dependency tree for outdated packages and known vulnerabilities.

Plus all the core operations you'd expect — done well, with smart rate limiting, response caching, and error handling that doesn't just crash.

---

## Quick Start

### 1. Get a GitHub Token

Go to [github.com/settings/tokens](https://github.com/settings/tokens) and create a Personal Access Token with these scopes:

- `repo` — Full control of private repositories
- `read:org` — Read org membership
- `read:user` — Read user profile data

### 2. Deploy on Apify

```
$apify push
```

Or use the Apify Console to create an Actor from this repository.

### 3. Configure Input

```
{
  "githubToken": "ghp_your_token_here",
  "defaultRepo": "owner/repo",
  "enableInsights": true,
  "enableGraphQL": false,
  "rateLimitBuffer": 500,
  "readOnly": false
}
```

### 4. Connect to Your AI Agent

The server runs via stdio transport, compatible with Claude Desktop, Cursor, and any MCP-compatible client.

**Claude Desktop** (`claude_desktop_config.json`):

```
{
  "mcpServers": {
    "github": {
      "command": "node",
      "args": ["path/to/dist/main.js"],
      "env": {
        "GITHUB_TOKEN": "ghp_your_token_here"
      }
    }
  }
}
```

**Cursor** (`.cursor/mcp.json`):

```
{
  "github-mcp-server": {
    "command": "node",
    "args": ["path/to/dist/main.js"],
    "env": {
      "GITHUB_TOKEN": "ghp_your_token_here"
    }
  }
}
```

---

## Tool Reference

### Tier 1: Core Operations

| Tool | Description |
| --- | --- |
| `github_search_repositories` | Search repos by query, language, stars, topics. Includes momentum score. |
| `github_get_repository` | Full repo details with health indicators (README, LICENSE, CI, CONTRIBUTING). |
| `github_list_branches` | Branches with protection status and ahead/behind counts vs default branch. |
| `github_get_file_contents` | Read any file from any branch. Auto-decodes base64, detects binary files. |
| `github_list_issues` | Filter issues by state, labels, assignee, milestone, date range. Response time stats. |
| `github_create_issue` | Create issue with auto-suggested labels based on title/body keyword matching. |
| `github_get_issue` | Full issue with comments, timeline, linked PRs, and conversation summary. |
| `github_update_issue` | Update title, body, state, labels, assignees, milestone. |
| `github_list_pull_requests` | List PRs with merge conflict status and review bottleneck info. |
| `github_get_pull_request` | Full PR with diff stats, reviews, checks, and review readiness assessment. |
| `github_create_pull_request` | Create PR with title, body, head/base, draft status, reviewers. |
| `github_search_code` | Search code across repos with language/path/filename filters. |
| `github_compare_branches` | Compare branches: commits ahead/behind, files changed, conflict potential. |
| `github_list_commits` | Commits with filters and stats (additions, deletions, files changed). |

### Tier 2: Intelligence Layer

| Tool | Description |
| --- | --- |
| `github_repo_health_check` | Comprehensive health report with score 0–100 across 5 categories. |
| `github_pr_risk_analysis` | Risk assessment for PRs: size, critical files, tests, breaking changes. |
| `github_contributor_insights` | Bus factor, expertise mapping, review load, active/dormant contributors. |
| `github_issue_triage` | Auto-categorize, prioritize, detect duplicates, find good-first-issues. |
| `github_codebase_overview` | Architecture detection, tech stack, entry points, test coverage indicator. |
| `github_dependency_check` | Dependency health: outdated packages, vulnerabilities, health score. |

### Tier 3: Workflow & Automation

| Tool | Description |
| --- | --- |
| `github_list_workflows` | GitHub Actions workflows with success rate, avg duration, last run status. |
| `github_trigger_workflow` | Dispatch a workflow with custom inputs. Returns run ID. |
| `github_get_workflow_run` | Run status, timing, failed step identification. |
| `github_release_notes_generator` | Generate structured release notes from conventional commits between two refs. |
| `github_manage_labels` | List, create, update, delete labels. Suggest standard label taxonomy. |

### Tier 4: Full lifecycle (repos, code, reviews, search, org, security)

| Tool | Description |
| --- | --- |
| `github_get_directory` | List directory entries (files/subdirs) at a path. |
| `github_create_or_update_file` | Create/update a single file via Contents API (base64-encoded). |
| `github_delete_file` | Delete a file with commit. |
| `github_create_tree_and_commit` | **Batch push**: many files in one commit (Git Data API). |
| `github_create_repository` / `github_update_repository` / `github_delete_repository` | Repo CRUD (`delete` requires `confirm: "DELETE"`). |
| `github_fork_repository` | Fork to user or org. |
| `github_star_repository` / `github_list_starred` | Star/unstar; list starred repos. |
| `github_transfer_repository` | Transfer ownership. |
| `github_create_branch` / `github_delete_branch` / `github_merge_branches` | Branch operations. |
| `github_update_branch_protection` | Branch protection or `remove_protection: true` to clear. |
| `github_list_review_comments` | Inline PR review comments. |
| `github_submit_review` / `github_merge_pull_request` / `github_update_pull_request` | Review and merge. |
| `github_add_comment` / `github_add_reaction` | Issue/PR comments and reactions. |
| `github_list_tags` / `github_list_releases` | Tags and releases. |
| `github_create_tag` / `github_create_release` / `github_delete_release` | Tag, release, delete release. |
| `github_search_issues_prs` / `github_search_users` / `github_search_topics` | Global search. |
| `github_get_user` | Public user profile. |
| `github_list_orgs` / `github_get_org` / `github_list_org_members` / `github_list_org_repos` | Organizations. |
| `github_manage_collaborators` | List/add/remove collaborators. |
| `github_list_notifications` / `github_mark_notifications_read` / `github_manage_watching` | Notifications & watching. |
| `github_list_gists` / `github_get_gist` / `github_create_gist` / `github_update_gist` | Gists. |
| `github_manage_secrets` | List/set/delete **Actions** secrets (values sealed with repo public key). |
| `github_manage_deploy_keys` | List/add/remove deploy keys. |
| `github_list_vulnerability_alerts` | Dependabot alerts. |
| `github_manage_webhooks` | List/create/update/delete repo webhooks. |

### Tier 5: GraphQL (optional, `enableGraphQL: true`)

| Tool | Description |
| --- | --- |
| `github_list_projects` / `github_get_project` | Projects V2 for a repo or org. |
| `github_list_discussions` / `github_create_discussion` | Repository discussions. |

**Read-only mode** (`readOnly: true`): registers read/search/list tools; skips file writes, repo mutations, branch mutations (entire branch module), PR mutations (except `github_list_review_comments`), tag/release/tag mutations, starred list is kept; notifications list only; gist list/get only; security list paths only inside combined tools.

**End-to-end example:** create repo → `github_create_tree_and_commit` → branch → `github_create_pull_request` → `github_submit_review` → `github_merge_pull_request` → `github_create_tag` → `github_create_release`.

---

## Intelligence Layer Deep Dive

### Repo Health Check

The killer feature. Given any repository, returns a comprehensive health report:

```
{
  "overall_score": 78,
  "breakdown": {
    "documentation": {
      "score": 85,
      "has_readme": true,
      "readme_length": "comprehensive",
      "has_contributing": true,
      "has_license": true,
      "has_code_of_conduct": false
    },
    "maintenance": {
      "score": 72,
      "last_commit_days_ago": 3,
      "avg_issue_response_time_hours": 18,
      "stale_issues_count": 12
    },
    "community": {
      "score": 80,
      "bus_factor": 3,
      "star_trend": "growing"
    },
    "ci_cd": {
      "score": 75,
      "has_github_actions": true,
      "branch_protection_enabled": true
    },
    "security": {
      "score": 70,
      "has_security_policy": false,
      "open_vulnerability_alerts": 2
    }
  },
  "recommendations": [
    "Add a CODE_OF_CONDUCT.md to improve community trust",
    "Add a SECURITY.md policy",
    "3 stale PRs should be reviewed or closed"
  ]
}
```

### PR Risk Analysis

Every engineering manager wants this. Given a PR number:

```
{
  "risk_level": "high",
  "size_category": "L",
  "lines_changed": 847,
  "risk_factors": [
    "Large PR (847 lines) — consider breaking into smaller PRs",
    "Touches CI configuration (.github/workflows/deploy.yml)",
    "No test files modified despite 12 source files changed",
    "Only 1 reviewer for a large PR (recommend 2+)"
  ],
  "has_tests": false,
  "has_breaking_changes": true,
  "recommendations": [
    "Add test coverage for changed files",
    "Request at least one additional reviewer",
    "Consider splitting CI changes into a separate PR"
  ]
}
```

### Contributor Insights

Know your team's knowledge distribution:

```
{
  "bus_factor": 2,
  "total_contributors": 15,
  "active_contributors": 8,
  "review_load_distribution": [
    { "login": "alice", "reviews": 45, "percentage": 52 },
    { "login": "bob", "reviews": 28, "percentage": 32 }
  ],
  "expertise_map": {
    "src/api/": ["alice", "charlie"],
    "src/components/": ["bob", "diana"],
    "infrastructure/": ["alice"]
  }
}
```

---

## Configuration

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `githubToken` | string | *required* | GitHub PAT with `repo`, `read:org`, `read:user` scopes |
| `defaultOrg` | string | — | Default organization to scope queries to |
| `defaultRepo` | string | — | Default repo (`owner/repo`). Can be overridden per tool call. |
| `enableInsights` | boolean | `true` | Enable intelligence layer tools. Disable to reduce API usage. |
| `rateLimitBuffer` | integer | `500` | Reserve this many API calls from the 5000/hr limit |
| `readOnly` | boolean | `false` | Disable all mutation tools (create, update, delete) |

---

## Rate Limiting

The server implements smart rate limiting that other MCP servers lack:

**How it works:**

1. After every API call, the server reads `X-RateLimit-Remaining` from response headers.
2. When remaining calls approach the `rateLimitBuffer` threshold, the server automatically pauses and waits for the rate limit window to reset.
3. If the wait would exceed 5 minutes, the server throws a clear error instead of hanging.
4. On 429 (rate limited), 502, and 503 responses, the server retries with exponential backoff (up to 3 attempts).

**Response caching** further reduces API usage:

- GET responses are cached for 60 seconds
- Cache is automatically invalidated on any mutation (POST/PUT/PATCH/DELETE)
- ETags are stored for potential conditional request support

**Rate limit status** is logged periodically and included in the final shutdown log so you always know your consumption.

---

## Security

### Token Scopes

The GitHub token needs these scopes:

- **`repo`** — Required for accessing private repositories, issues, PRs, and repository contents
- **`read:org`** — Required for organization membership and team data
- **`read:user`** — Required for user profile information

For public repositories only, a token with just `public_repo` scope is sufficient.

### Security Measures

- The GitHub token is **never logged**, **never included in error messages**, and **never written to datasets**
- All user input is validated via Zod schemas before reaching the GitHub API
- The `readOnly` mode disables all mutation tools for safe exploration
- Error messages are informative but never expose internal implementation details

---

## Use Cases

### Onboard to a New Codebase in 60 Seconds

```
Agent: Use github_codebase_overview for "facebook/react"
→ Language breakdown, architecture (monorepo), entry points, test coverage

Agent: Use github_repo_health_check for "facebook/react"
→ Health score 92/100, strong across all categories
```

### Automated PR Review Assistant

```
Agent: Use github_pr_risk_analysis for PR #4521
→ Risk: medium, size L, missing tests for 3 changed files

Agent: Use github_get_pull_request for PR #4521
→ Full diff stats, 2 approved reviews, all checks passing
```

### Sprint Planning Helper

```
Agent: Use github_issue_triage for "myorg/backend"
→ 47 open issues categorized: 12 bugs, 18 features, 8 questions, 9 stale

Agent: Use github_contributor_insights for "myorg/backend"
→ Bus factor: 2 (risk!), Alice handling 60% of reviews
```

### Release Automation

```
Agent: Use github_release_notes_generator from "v2.3.0" to "main"
→ Structured notes: 5 features, 3 fixes, 1 breaking change, formatted Markdown

Agent: Use github_compare_branches base "release/2.4" head "main"
→ 23 commits ahead, 0 behind, no conflicts
```

### Repo Audit

```
Agent: Use github_repo_health_check for "startup/api"
→ Score 45/100 — missing license, no CI, 30 stale issues

Agent: Use github_dependency_check for "startup/api"
→ 8 outdated packages, 2 known vulnerabilities (high severity)
```

---

## Output Format

Every tool call is logged to the Apify dataset with this structure:

```
{
  "toolCallId": "uuid",
  "toolName": "github_repo_health_check",
  "timestamp": "2025-01-15T10:30:00Z",
  "duration_ms": 2450,
  "success": true,
  "rateLimitRemaining": 4823
}
```

---

## Troubleshooting

### "Authentication failed"

Your token is invalid or expired. Generate a new one at [github.com/settings/tokens](https://github.com/settings/tokens). Make sure it has `repo`, `read:org`, and `read:user` scopes.

### "Rate limit exceeded"

You've consumed too many API calls. The server respects a buffer (default 500 calls) and will pause automatically. If you're hitting limits frequently, increase `rateLimitBuffer` or disable `enableInsights` (insight tools use more API calls per invocation).

### "Resource not found"

The repository, issue, or PR doesn't exist — or your token doesn't have access. Private repos require the `repo` scope. Check the owner/repo format is correct (e.g., `facebook/react`, not `react`).

### "Conflict: repository is empty"

Some operations (like listing branches or reading files) fail on empty repositories. Push at least one commit first.

### Intelligence tools return incomplete data

The insight tools aggregate data from multiple API calls. If the repository is very large or has thousands of issues, some metrics may be estimated rather than exact. The tools are designed to stay within rate limits by sampling recent data rather than exhaustively scanning all history.

### Build errors

```
npm install
npm run build
```

Requires Node.js 20+. The build compiles TypeScript from `src/` into `dist/`.

---

## Development

```
# Install dependencies
npm install

# Build
npm run build

# Run locally (requires GITHUB_TOKEN env var)
GITHUB_TOKEN=ghp_xxx npm start

# Development with auto-reload
GITHUB_TOKEN=ghp_xxx npm run dev
```

---

## Architecture

```
src/
├── main.ts              # Apify Actor entry point
├── mcp-server.ts        # MCP server setup & tool registration
├── github-client.ts     # API client with rate limiting & caching
├── types.ts             # TypeScript interfaces
└── tools/
    ├── repo-tools.ts    # Repository operations (4 tools)
    ├── issue-tools.ts   # Issue operations (4 tools)
    ├── pr-tools.ts      # Pull request operations (3 tools)
    ├── code-tools.ts    # Code search & analysis (3 tools)
    ├── insight-tools.ts # Intelligence layer (6 tools)
    ├── workflow-tools.ts # GitHub Actions (3 tools)
    └── release-tools.ts # Releases & labels (2 tools)
```

---

## License

MIT