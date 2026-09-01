<div align="center">

<img src="assets/logo.png" width="120" alt="ToolSDK MCP Registry" />

# Awesome ToolSDK MCP Registry with stars

**The Enterprise MCP Registry & Gateway.** A unified infrastructure to discover, secure, and execute Model Context Protocol (MCP) tools. Exposes local processes (STDIO) and remote servers (StreamableHTTP) via a unified HTTP API with built-in Sandbox and OAuth 2.1 support.

<a href="https://www.npmjs.com/package/@toolsdk.ai/registry">
  <img src="https://img.shields.io/npm/v/@toolsdk.ai/registry.svg?style=flat-square" alt="npm version" />
</a>
<a href="https://github.com/toolsdk-ai/toolsdk-mcp-registry/actions/workflows/test.yaml">
  <img src="https://github.com/toolsdk-ai/toolsdk-mcp-registry/actions/workflows/test.yaml/badge.svg" alt="Build Status" />
</a>
<img src="https://img.shields.io/badge/MCP_Servers-4547-blue?style=flat-square" alt="MCP Servers Count" />
<img src="https://img.shields.io/badge/LICENSE-MIT-ff69b4?style=flat-square" alt="License" />
<br />
<a href="https://www.producthunt.com/products/toolsdk-ai">
  <img src="https://api.producthunt.com/widgets/embed-image/v1/top-post-badge.svg?post_id=997428&theme=light&period=daily" alt="Product Hunt" height="40" />
</a>

<a href="#mcp-servers">🔍 <b>Browse 4547+ Tools</b></a>
  •   <a href="#quick-start">🐳 <b>Self-hosted</b></a>
  •   <a href="#install-via-package-manager">📦 <b>Use as SDK</b></a>
  •   <a href="./docs/CONTRIBUTING.md">➕ <b>Add Server</b></a>
  •   <a href="https://www.youtube.com/watch?v=J_oaDtCoVVo" target="_blank">🎥 <b>Video Tutorial</b></a>

<a href="https://toolsdk.ai" target="_blank">
  <img src="assets/hero.png" alt="ToolSDK.ai - MCP Servers Hosting" />
</a>

***

</div>

## Start Here

* 🔍 I want to **find an MCP Server** → [Browse Directory](#mcp-servers)
* 🔌 I want to **integrate MCP tools** into my AI app → [Integration Guide](https://toolsdk.ai/docs/tutorials/getting-started#-quick-start)
* 🚀 I want to **deploy an MCP Gateway** → [Deployment Guide](#deploy-enterprise-gateway-recommended)
* ➕ I want to **submit my MCP Server** → [Contribution Guide](./docs/CONTRIBUTING.md)

> \[!IMPORTANT]
> **Pro Tip**: If a server is marked as `validated: true`, you can use it instantly with **Vercel AI SDK**:
>
> ```ts
> const tool = await toolSDK.package('<packageName>', { ...env }).getAISDKTool('<toolKey>');
> ```
>
> **Want validation?** Ask AI: *"Analyze the `make build` target in the Makefile and the scripts it invokes, and determine how an MCP server gets marked as `validated: true`."*

## Getting Started

<a id="docker-self-hosting"></a>

### Deploy Enterprise Gateway (Recommended)

Deploy your own **private MCP Gateway & Registry** in minutes. This provides the full feature set: Federated Search, Remote Execution, Sandbox, and OAuth.

#### ⚡ Quick Deploy (One-Liner)

Start the registry immediately with default settings:

```bash
docker compose up -d
```

*Did this save you time? Give us a [**Star on GitHub**](https://github.com/toolsdk-ai/toolsdk-mcp-registry) ⭐ 186 | 🐛 32 | 🌐 TypeScript | 📅 2026-08-27 — it helps others discover this registry!*

**Configuration:**

* Set `MCP_SANDBOX_PROVIDER=LOCAL` in `.env` file if you want to disable the sandbox (not recommended for production).
* *See [Configuration Guide](./docs/DEVELOPMENT.md) for full details.*

> \[!TIP]
> **Tip for Private Deployment**: This registry contains 4547+ public MCP servers. If you only need a specific subset for your private environment, you can prune the `packages/` directory.
> 📖 See [Package Management Guide](./docs/DEVELOPMENT.md#5--package-management-for-private-deployment) for details.

That's it! Your self-hosted MCP registry is now running with:

* 🌐 **HTTP API** with OpenAPI documentation
* 🛡️ **Secure Sandbox execution** for AI agent tools
* 🔍 **Full-text search** (Meilisearch)

#### 🎉 Access Your Private MCP Registry

* 🌐 **Local Web Interface**: <http://localhost:3003>
* 📚 **Swagger API Docs**: <http://localhost:3003/swagger>
* 🔍 **Search & Execute** 4547+ MCP Servers remotely
* 🤖 **Integrate** with your AI agents, chatbots, and LLM applications

#### 🌐 Remote Tool Execution Example

Execute any MCP tool via HTTP API - perfect for AI automation, chatbot integrations, and serverless deployments:

```bash
curl -X POST http://localhost:3003/api/v1/packages/run \
  -H "Content-Type: application/json" \
  -d '{
    "packageName": "@modelcontextprotocol/server-everything",
    "toolKey": "echo",
    "inputData": {
      "message": "Hello from ToolSDK MCP Registry!"
    },
    "envs": {}
  }'
```

#### 🔌 MCP Gateway (Streamable HTTP Proxy)

The registry also acts as an **MCP Gateway** — any registered package can be accessed as a standard [Streamable HTTP](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#streamable-http) endpoint, even if the original server is STDIO-only.

**Endpoint:** `POST /mcp/<packageName>`

Pass environment variables via `x-mcp-env-*` headers:

```bash
curl -X POST http://localhost:3003/mcp/@modelcontextprotocol/server-github \
  -H "Content-Type: application/json" \
  -H "x-mcp-env-GITHUB_PERSONAL_ACCESS_TOKEN: ghp_your_token" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

The server returns a `mcp-session-id` header — include it in subsequent requests to reuse the session (sessions expire after 30 min).

This is useful for:

* **Protocol Bridging** — Expose local STDIO servers as remote HTTP endpoints
* **Centralized Access** — Give AI agents a single HTTP gateway to all MCP tools
* **Client Compatibility** — Connect from any MCP client that supports Streamable HTTP

<details>
<summary><strong>Alternative: Use as Registry SDK (Data Only)</strong></summary>

<a id="use-as-sdk"></a>

### Alternative: Use as Registry SDK (Data Only)

If you only need to access the **list of MCP servers** programmatically (without execution or gateway features), you can use the NPM package.

```bash
npm install @toolsdk.ai/registry
```

#### Usage

Perfect for building your own directory or analysis tools:

```ts
import mcpServerLists from '@toolsdk.ai/registry/indexes/packages-list.json';
```

#### Access via Public API (No Installation Required)

Fetch the complete MCP server registry programmatically:

```bash
curl https://toolsdk-ai.github.io/toolsdk-mcp-registry/indexes/packages-list.json
```

```ts
// JavaScript/TypeScript - Fetch API
const mcpServers = await (
  await fetch('https://toolsdk-ai.github.io/toolsdk-mcp-registry/indexes/packages-list.json')
).json();

// Use for AI agent tool discovery, LLM integrations, etc.
console.log(mcpServers);
```

```python
# Python - For AI/ML projects
import requests

mcp_servers = requests.get(
    'https://toolsdk-ai.github.io/toolsdk-mcp-registry/indexes/packages-list.json'
).json()

# Perfect for LangChain, CrewAI, AutoGen integrations
```

</details>

## Why ToolSDK MCP Registry?

**ToolSDK MCP Registry** is an enterprise-grade gateway for Model Context Protocol (MCP) servers. It solves the challenge of securely discovering and executing AI tools in production environments.

### Key Features

* **Federated Registry** - Unified search across local private servers and the official `@modelcontextprotocol/registry`.
* **Unified Interface** - Access local STDIO tools and remote StreamableHTTP servers via a single, standardized HTTP API.
* **Secure Sandbox** - Execute untrusted tools in isolated environments (supports E2B, Daytona, Sandock).
* **OAuth 2.1 Proxy** - Built-in OAuth 2.1 implementation to handle complex authentication flows for your agents. [Integration Guide](./docs/DEVELOPMENT.md#10--oauth-integration)
* **Private & Self-Hosted** - Full control over your data and infrastructure with Docker deployment.
* **Developer-Friendly** - OpenAPI/Swagger documentation and structured JSON configs.

### Use Cases

* **Enterprise AI Gateway** - Centralize tool access for all your internal LLM applications.
* **Secure Tool Execution** - Run community MCP servers without risking your local environment.
* **Protocol Adaptation** - Connect remote agents (via HTTP API) to local CLI tools (via STDIO).
* **Unified Discovery** - One API to search and manage thousands of tools.

### Architecture

```mermaid
graph TD
    subgraph ClientSide ["Client Side"]
        LLM["🤖 AI Agent / LLM"]
        User["👤 User / Developer"]
    end

    subgraph DockerEnv ["🐳 Self-Hosted Infrastructure"]
        
        subgraph RegistryCore ["Registry Core"]
            API["🌐 Registry API"]
            Search["🔍 Meilisearch"]
            DB["📚 Registry Data"]
            OAuth["🔐 OAuth Proxy"]
        end

        subgraph RuntimeEnv ["Runtime Environment"]
            Local["💻 Local Exec"]
            Sandbox["🛡️ Secure Sandbox"]
            MCPServer["⚙️ MCP Server"]
        end
    end

    User -->|Search Tools| API
    LLM -->|Execute Tool| API
    LLM -->|Auth Flow| OAuth
    API <-->|Query Index| Search
    API -->|Read Metadata| DB
    API -->|Run Tool| Local
    API -->|Run Tool| Sandbox
    Local -->|Execute| MCPServer
    Sandbox -->|Execute| MCPServer
```

***

## What You Get

This open-source project provides:

* **Structured Registry** - 4547+ MCP servers with metadata
* **Unified Gateway** - HTTP API to query and execute tools remotely
* **Auto-Generated Docs** - Always up-to-date README and API documentation

### ✅ Validated Packages = One-Line Integration (ToolSDK)

Some packages in this registry are marked as `validated: true`.

> \[!NOTE]
> **What does `validated: true` mean for you?**
>
> * You can load the MCP package directly via our ToolSDK NPM client and get ready-to-use tool adapters (e.g. **Vercel AI SDK tools**) without writing your own tool schema mapping.
> * The registry index includes the discovered `tools` metadata for validated packages, so you can pick a `toolKey` and call it immediately.
>
> **Where is this flag stored?**
>
> * See `indexes/packages-list.json` entries (e.g. `{"validated": true, "tools": { ... } }`).

#### Example: Use a validated package with Vercel AI SDK

Template: `const tool = await toolSDK.package('<packageName>', { ...env }).getAISDKTool('<toolKey>');`

```ts
// import { generateText } from 'ai';
// import { openai } from '@ai-sdk/openai'
import { ToolSDKApiClient } from 'toolsdk/api';

const toolSDK = new ToolSDKApiClient({ apiKey: process.env.TOOLSDK_AI_API_KEY });
const searchMCP = await toolSDK.package('@toolsdk.ai/tavily-mcp', { TAVILY_API_KEY: process.env.TAVILY_API_KEY });
const searchTool = await searchMCP.getAISDKTool('tavily-search');

// const completion = await generateText({
//   model: openai('gpt-4.1'),
//   messages: [{
//       role: 'user',
//       content: 'Help me search for the latest AI news',
//   }],
//   tools: { searchTool, emailTool },
// });
```

**Available as:**

* **Docker Image** - Full-featured Gateway & Registry
* **NPM Package** - TypeScript/JavaScript SDK for data access
* **Raw Data** - JSON endpoints for direct integration

***

<a id="mcp-servers"></a>

## MCP Servers Directory

**4547+ AI Agent Tools, LLM Integrations & Automation Servers**

> \[!NOTE]
> ⭐ **Featured below**: Hand-picked, production-ready MCP servers verified by our team.
>
> 📚 **Looking for all 4547+ servers?** Check out [**All MCP Servers**](./docs/ALL-MCP-SERVERS.md) for the complete list.

> \[!TIP]
> If a package is marked as `validated: true` in the index, you can usually wire it up in minutes via ToolSDK (e.g. `getAISDKTool(toolKey)`).

Browse by category: Developer Tools, AI Agents, Databases, Cloud Platforms, APIs, and more!

<a id="uncategorized"></a>

<details>
<summary><strong>Uncategorized</strong></summary>

Tools that haven’t been sorted into a category yet. AI will categorize it later.

* [✅ @toolsdk-remote/dev-ohmyposh-validator](https://github.com/JanDeDobbeleer/oh-my-posh) ⭐ 23,387 | 🐛 10 | 🌐 Go | 📅 2026-09-01: Validate oh-my-posh configurations and segment snippets against the official schema.  (2 tools) (node)
* [✅ @toolsdk-remote/exa](https://github.com/exa-labs/exa-mcp-server) ⭐ 4,952 | 🐛 33 | 🌐 TypeScript | 📅 2026-08-21: Fast, intelligent web search and web crawling.
* [✅ @antv/mcp-server-chart](https://github.com/antvis/mcp-server-chart) ⭐ 4,347 | 🐛 6 | 🌐 TypeScript | 📅 2026-08-27: A visualization mcp contains 25+ visual charts using @antvis. Using for chart generation and data analysis.  (25 tools) (node)
* [✅ @toolsdk-remote/com-cloudflare-mcp-mcp](https://github.com/cloudflare/mcp-server-cloudflare) ⭐ 4,134 | 🐛 60 | 🌐 TypeScript | 📅 2026-09-01: Cloudflare MCP servers  (2 tools) (node)
* [✅ @browserbasehq/mcp-server-browserbase](https://github.com/browserbase/mcp-server-browserbase) ⚠️ Archived: MCP server for AI web browser automation using Browserbase and Stagehand  (9 tools) (node)
* [✅ @toolsdk-remote/com-microsoft-microsoft-learn-mcp](https://github.com/MicrosoftDocs/mcp) ⭐ 1,867 | 🐛 23 | 🌐 TypeScript | 📅 2026-08-12: Official Microsoft Learn MCP Server – real-time, trusted docs & code samples for AI and LLMs.  (3 tools) (node)
* [✅ @tigerdata/pg-aiguide](https://github.com/timescale/pg-aiguide) ⭐ 1,827 | 🐛 34 | 🌐 Python | 📅 2026-08-31: Comprehensive PostgreSQL documentation and best practices, including ecosystem tools  (3 tools) (node)
* [✅ @delorenj/mcp-server-trello](https://github.com/delorenj/mcp-server-trello) ⭐ 436 | 🐛 23 | 🌐 TypeScript | 📅 2026-08-29: MCP server for Trello boards with rate limiting, type safety, and comprehensive API integration.  (34 tools) (node)
* [✅ @f2c/mcp](https://github.com/f2c-ai/f2c-mcp) ⭐ 411 | 🐛 22 | 🌐 TypeScript | 📅 2025-11-27: Bridges Figma design files to code generation, enabling direct conversion of designs into HTML, CSS, and other assets with customizable output paths and file organization.  (2 tools) (node)
* [✅ @shodh/memory-mcp](https://github.com/varun29ankuS/shodh-memory) ⭐ 275 | 🐛 28 | 🌐 Rust | 📅 2026-09-01: Persistent AI memory with semantic search. Store and recall context across sessions.  (10 tools) (node)
* [✅ @browserstack/mcp-server](https://github.com/browserstack/mcp-server) ⭐ 150 | 🐛 44 | 🌐 TypeScript | 📅 2026-09-01: Integrates with BrowserStack's testing infrastructure to enable automated and manual testing across browsers, devices, and platforms for debugging cross-browser issues and verifying mobile app functionality.  (20 tools) (node)
* [✅ @cyanheads/pubmed-mcp-server](https://github.com/cyanheads/pubmed-mcp-server) ⭐ 142 | 🐛 9 | 🌐 TypeScript | 📅 2026-08-21: Enables AI systems to search, retrieve, and analyze biomedical literature from PubMed for evidence-based research, citation generation, and data visualization  (5 tools) (node)
* [✅ @professional-wiki/mediawiki-mcp-server](https://github.com/professionalwiki/mediawiki-mcp-server) ⭐ 125 | 🐛 11 | 🌐 TypeScript | 📅 2026-08-31: Integrates with MediaWiki instances through REST API to enable searching pages, retrieving content in multiple formats, accessing file information, viewing revision history, and performing authenticated operations like creating and updating pages with automatic wiki discovery and dynamic configuration management.  (7 tools) (node)
* [✅ @microsoft/clarity-mcp-server](https://github.com/microsoft/clarity-mcp-server) ⭐ 114 | 🐛 14 | 🌐 TypeScript | 📅 2026-02-24: Enables AI to fetch and analyze Microsoft Clarity website analytics data including metrics like scroll depth, engagement time, and traffic with filtering by browser, device, and country.  (1 tools) (node)
* [✅ @portel/ncp](https://github.com/portel-dev/ncp) ⭐ 97 | 🐛 1 | 🌐 JavaScript | 📅 2026-06-12: N-to-1 MCP Orchestration. Unified gateway for multiple MCP servers with intelligent tool discovery.  (2 tools) (node)
* [✅ @atlassian-dc-mcp/bitbucket](https://github.com/b1ff/atlassian-dc-mcp) ⭐ 93 | 🐛 4 | 🌐 TypeScript | 📅 2026-08-30: MCP server for Atlassian Bitbucket Data Center - interact with repositories and code  (9 tools) (node)
* [✅ @atlassian-dc-mcp/jira](https://github.com/b1ff/atlassian-dc-mcp) ⭐ 93 | 🐛 4 | 🌐 TypeScript | 📅 2026-08-30: MCP server for Atlassian Jira Data Center - search, view, and create issues  (6 tools) (node)
* [✅ @bankless/onchain-mcp](https://github.com/bankless/onchain-mcp) ⭐ 80 | 🐛 10 | 🌐 TypeScript | 📅 2026-05-05: Integrates with blockchain networks to enable smart contract interaction, transaction history access, and on-chain data exploration through specialized tools for reading contract state, retrieving ABIs, and filtering event logs.  (10 tools) (node)
* [✅ @mzxrai/mcp-openai](https://github.com/mzxrai/mcp-openai) ⭐ 76 | 🐛 4 | 🌐 JavaScript | 📅 2024-12-06: Generate text using OpenAI's language models.  (1 tools) (node)
* [✅ @toolsdk-remote/com-sonatype-dependency-management-mcp-server](https://github.com/sonatype/dependency-management-mcp-server) ⭐ 73 | 🐛 5 | 📅 2026-01-14: Sonatype component intelligence: versions, security analysis, and Trust Score recommendations  (3 tools) (node)
* [✅ @ivotoby/contentful-management-mcp-server](https://github.com/ivo-toby/contentful-mcp) ⭐ 62 | 🐛 10 | 🌐 TypeScript | 📅 2026-01-17: Integrate with Contentful's Content Management API for CMS management.  (40 tools) (node)
* [✅ @bnb-chain/mcp](https://github.com/bnb-chain/bnbchain-mcp) ⭐ 60 | 🐛 3 | 🌐 TypeScript | 📅 2026-04-17: Enables direct interaction with BNB Chain and other EVM-compatible networks for blockchain operations including block exploration, smart contract interaction, token management, wallet operations, and Greenfield storage functionality.  (40 tools) (node)
* [✅ @mapbox/mcp-devkit-server](https://github.com/mapbox/mcp-devkit-server) ⭐ 60 | 🐛 7 | 🌐 TypeScript | 📅 2026-08-26: Provides AI assistants with direct access to Mapbox developer APIs and documentation.  (17 tools) (node)
* [✅ @toolsdk-remote/dev-promplate-hmr](https://github.com/promplate/hmr) ⭐ 59 | 🐛 28 | 🌐 Python | 📅 2026-08-31: Docs for hot-module-reload and reactive programming for Python (`hmr` on PyPI)  (3 tools) (node)
* [✅ @smartbear/mcp](https://github.com/SmartBear/smartbear-mcp) ⭐ 44 | 🐛 29 | 🌐 TypeScript | 📅 2026-09-01: MCP server for AI access to SmartBear tools, including BugSnag, Reflect, Swagger, PactFlow.  (67 tools) (node)
* [✅ @decodo/mcp-server](https://github.com/Decodo/mcp-web-scraper) ⭐ 35 | 🐛 3 | 🌐 TypeScript | 📅 2026-08-25: Enable your AI agents to scrape and parse web content dynamically, including geo-restricted sites  (5 tools) (node)
* [✅ @pubnub/mcp](https://github.com/pubnub/pubnub-mcp-server) ⭐ 33 | 🐛 0 | 🌐 TypeScript | 📅 2026-07-22: Enables AI assistants to interact with PubNub's realtime communication platform for retrieving documentation, accessing SDK information, and utilizing messaging APIs without leaving their conversation context.  (11 tools) (node)
* [✅ @connorbritain/mssql-mcp-server](https://github.com/ConnorBritain/mssql-mcp-server) ⭐ 32 | 🐛 1 | 🌐 C# | 📅 2026-08-04: MCP server for Microsoft SQL Server - schema discovery, profiling, and safe data operations  (20 tools) (node)
* [✅ @kirbah/mcp-youtube](https://github.com/kirbah/mcp-youtube) ⭐ 29 | 🐛 1 | 🌐 TypeScript | 📅 2026-09-01: YouTube MCP server for token-optimized, structured data using the YouTube Data API v3.  (9 tools) (node)
* [✅ @localstack/localstack-mcp-server](https://github.com/localstack/localstack-mcp-server) ⭐ 26 | 🐛 16 | 🌐 TypeScript | 📅 2026-09-01: A LocalStack MCP Server providing essential tools for local cloud development & testing  (8 tools) (node)
* [✅ @flightradar24/fr24api-mcp](https://github.com/Flightradar24/fr24api-mcp) ⭐ 24 | 🐛 0 | 🌐 TypeScript | 📅 2026-01-22: MCP server providing access to the Flightradar24 API for real-time and historical flight data  (15 tools) (node)
* [✅ @noditlabs/nodit-mcp-server](https://github.com/noditlabs/nodit-mcp-server) ⭐ 24 | 🐛 3 | 🌐 TypeScript | 📅 2026-06-23: Provides blockchain context through Nodit's APIs, enabling real-time interaction with multiple protocols including Ethereum, Polygon, and Aptos for token information and on-chain activity analysis.  (9 tools) (node)
* [✅ @mfukushim/map-traveler-mcp](https://github.com/mfukushim/map-traveler-mcp) ⭐ 23 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-03: Integrates with Google Maps to create virtual travel experiences where users can navigate real-world routes with customizable avatars, discover nearby facilities, and share journeys on Bluesky.  (8 tools) (node)
* [✅ @configcat/mcp-server](https://github.com/configcat/mcp-server) ⭐ 17 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-27: Enables AI agents to interact with ConfigCat, a feature flag service for teams.  (77 tools) (node)
* [✅ @glifxyz/mymcpspace-mcp-server](https://github.com/glifxyz/mymcpspace-mcp-server) ⚠️ Archived: Enables AI interaction with MyMCPSpace social media platform for creating posts, replying to content, toggling likes, retrieving feed data, and updating usernames through authenticated API communication.  (5 tools) (node)
* [✅ @sumup/mcp](https://github.com/sumup/sumup-agent-toolkit) ⭐ 12 | 🐛 11 | 🌐 TypeScript | 📅 2026-08-26: Tools to explore SumUp accounts, payments, customers, and payouts.  (49 tools) (node)
* [✅ @gonetone/mcp-server-taiwan-weather](https://github.com/GoneTone/mcp-server-taiwan-weather) ⭐ 11 | 🐛 0 | 🌐 TypeScript | 📅 2025-09-21: 用於取得臺灣中央氣象署 API 資料的 Model Context Protocol (MCP) Server  (1 tools) (node)
* [✅ @dubuqingfeng/gitlab-mcp-server](https://github.com/dubuqingfeng/gitlab-mcp-server) ⭐ 9 | 🐛 0 | 🌐 TypeScript | 📅 2025-09-10: GitLab MCP (Model Context Protocol) server for AI agents  (13 tools) (node)
* [✅ @kontent-ai/mcp-server](https://github.com/kontent-ai/mcp-server) ⭐ 9 | 🐛 4 | 🌐 TypeScript | 📅 2026-08-31: Connect to Kontent.ai to manage content, types, taxonomies, and workflows via natural language  (40 tools) (node)
* [✅ @shopana/novaposhta-mcp-server](https://github.com/shopanaio/carrier-api) ⭐ 7 | 🐛 0 | 🌐 HTML | 📅 2026-08-18: MCP Server for Nova Poshta API integration with AI assistants  (50 tools) (node)
* [✅ @tigerdata/tiger-skills-mcp-server](https://github.com/timescale/tiger-skills-mcp-server) ⭐ 7 | 🐛 0 | 🌐 TypeScript | 📅 2026-07-01: Provider agnostic skills implementation, with skills sourced from local paths or GitHub repositories  (1 tools) (node)
* [✅ @koki-develop/esa-mcp-server](https://github.com/koki-develop/esa-mcp-server.git) ⚠️ Archived: A Model Context Protocol (MCP) server for esa.io  (10 tools) (node)
* [✅ @index9/mcp](https://github.com/index9-org/mcp) ⚠️ Archived: Real-time model intelligence for your AI assistant.  (3 tools) (node)
* [✅ @mehmetsenol/gorev-mcp-server](https://github.com/msenol/Gorev) ⭐ 1 | 🐛 1 | 🌐 Go | 📅 2026-01-11: Task management system for AI assistants with MCP protocol, templates, and bilingual support (TR/EN)  (41 tools) (node)
* [✅ @dinesh-nalla-se/playwright-mcp](https://github.com/dinesh-nalla-se/playwright-mcp) ⭐ 0 | 🐛 0 | 📅 2025-11-14: Playwright Tools for MCP  (22 tools) (node)
* [✅ @symbioticsec/symbiotic-mcp-server](https://github.com/SymbioticSec/mcp) ⭐ 0 | 🐛 2 | 🌐 TypeScript | 📅 2025-12-15: Symbiotic CLI MCP Server for security scanning and analysis  (4 tools) (node)
* [✅ @toolsdk-remote/com-wallet-connectors-wallet-verifier-mcp](https://github.com/TalaoDAO/connectors) ⭐ 0 | 🐛 0 | 🌐 Python | 📅 2026-03-10: MCP server for verifying EUDI/Talao wallet data via OIDC4VP (pull) for AI agents.  (2 tools) (node)
* [✅ @duongkhuong/mcp-backlog](https://github.com/vfa-khuongdv/mcp-backlog): MCP server for Backlog API integration with AI agents.  (37 tools) (node)
* [✅ @duongkhuong/mcp-redmine](https://github.com/vfa-khuongdv/mcp_readmine): MCP server for Redmine API integration with AI agents.  (14 tools) (node)
* [✅ @incodetech/incode-idv-mcp](https://github.com/IncodeTechnologies/incode-idv-mcp): MCP server for Incode IDV, providing identity verification tools for AI assistants.  (5 tools) (node)
* [✅ @kekwanulabs/syncline-mcp-server](https://github.com/KekwanuLabs/syncline): Syncline MCP Server (TypeScript) - AI-powered meeting scheduling with intelligent auto-scheduling  (4 tools) (node)
* [✅ @letta-ai/memory-mcp](https://github.com/letta-ai/memory-mcp): MCP server for AI memory management using Letta - Standard MCP format  (5 tools) (node)
* [✅ @moralisweb3/api-mcp-server](https://github.com/moralisweb3/moralis-mcp-server): Integrates with Moralis Web3 API to enable blockchain data access, token analysis, and smart contract interactions without requiring deep Web3 development knowledge  (93 tools) (node)
* [✅ @picahq/mcp](https://github.com/picahq/mcp): A Model Context Protocol Server for Pica  (4 tools) (node)
* [✅ @studious-xiaoyu/oracle-link](https://github.com/StudiousXiaoYu/oracle-link): Oracle MCP Query Server (Node.js) - Read-only SELECT via MCP  (3 tools) (node)
* [✅ @toolsdk-remote/com-mermaidchart-mermaid-mcp](https://github.com/Mermaid-Chart/mermaid-mcp): MCP server for Mermaid diagram validation and rendering  (2 tools) (node)
* [✅ @toolsdk-remote/com-redpanda-docs-mcp](https://github.com/redpanda-data/docs-site): Get authoritative answers to questions about Redpanda.  (1 tools) (node)

New mcp tool: Exa-code is a context tool for coding   (1 tools) (node)

* [✅ @upstash/context7-mcp](https://github.com/upstash/context7) ⭐ 61,479 | 🐛 68 | 🌐 TypeScript | 📅 2026-09-01: Connects to Context7.com's documentation database to provide up-to-date library and framework documentation with intelligent project ranking and customizable token limits.  (2 tools) (node)
* [✅ firecrawl-mcp](https://github.com/mendableai/firecrawl-mcp-server) ⭐ 7,362 | 🐛 124 | 🌐 TypeScript | 📅 2026-09-01: Integration with FireCrawl to provide advanced web scraping capabilities for extracting structured data from complex websites.  (8 tools) (node)
* [✅ xcodebuildmcp](https://github.com/cameroncooke/xcodebuildmcp) ⭐ 6,319 | 🐛 20 | 🌐 TypeScript | 📅 2026-09-01: Enables building, running, and debugging iOS and macOS applications through Xcode with tools for project discovery, simulator management, app deployment, and UI automation testing.  (83 tools) (node)
* [✅ @toolsdk-remote/klavis-strata](https://github.com/Klavis-AI/klavis) ⭐ 5,797 | 🐛 294 | 🌐 Python | 📅 2026-06-01: MCP server for progressive tool usage at any scale (see <https://klavis.ai>)  (1 tools) (node)
* [✅ @toolsdk.ai/tavily-mcp](https://github.com/tavily-ai/tavily-mcp) ⭐ 2,365 | 🐛 48 | 🌐 JavaScript | 📅 2026-08-20: An MCP server that implements web search, extract, mapping, and crawling through the Tavily API.  (4 tools) (node)
* [✅ ref-tools-mcp](https://github.com/ref-tools/ref-tools-mcp) ⭐ 1,164 | 🐛 6 | 🌐 TypeScript | 📅 2026-06-26: Integrates with Ref.tools documentation search service to provide curated technical documentation access, web search fallback, and URL-to-markdown conversion for efficient developer reference during coding workflows.  (2 tools) (node)
* [✅ octocode-mcp](https://github.com/bgauryy/octocode-mcp) ⭐ 919 | 🐛 3 | 🌐 TypeScript | 📅 2026-08-25: AI code research platform. Search, analyze, and extract insights from any GitHub repository.  (5 tools) (node)
* [✅ unreal-engine-mcp-server](https://github.com/ChiR24/Unreal_mcp.git) ⭐ 850 | 🐛 25 | 🌐 TypeScript | 📅 2026-08-29: MCP server for Unreal Engine 5 with 13 tools for game development automation.  (13 tools) (node)
* [✅ strava-mcp-server](https://github.com/r-huijts/strava-mcp) ⭐ 475 | 🐛 19 | 🌐 TypeScript | 📅 2026-06-13: MCP server for accessing Strava API  (19 tools) (node)
* [✅ airtable-mcp-server](https://github.com/domdomegg/airtable-mcp-server.git) ⭐ 456 | 🐛 2 | 🌐 TypeScript | 📅 2026-08-11: Read and write access to Airtable database schemas, tables, and records.  (15 tools) (node)
* [✅ mcp-local-rag](https://github.com/shinpr/mcp-local-rag) ⭐ 378 | 🐛 2 | 🌐 TypeScript | 📅 2026-08-29: Easy-to-setup local RAG server with minimal configuration  (5 tools) (node)
* [✅ @toolsdk-remote/packmind-mcp-server](https://github.com/PackmindHub/packmind) ⭐ 307 | 🐛 51 | 🌐 TypeScript | 📅 2026-09-01: Packmind captures, scales, and enforces your organization's technical decisions.  (1 tools) (node)
* [✅ opik-mcp](https://github.com/comet-ml/opik-mcp) ⭐ 217 | 🐛 33 | 🌐 Python | 📅 2026-09-01: Interact with Opik prompts, traces, and metrics through the Model Context Protocol.  (13 tools) (node)
* [✅ mcp-arr-server](https://github.com/aplaceforallmystuff/mcp-arr) ⭐ 207 | 🐛 10 | 🌐 JavaScript | 📅 2026-08-09: MCP server for \*arr media suite - Sonarr, Radarr, Lidarr, Readarr, Prowlarr  (67 tools) (node)
* [✅ mcp-rubber-duck](https://github.com/nesquikm/mcp-rubber-duck) ⭐ 176 | 🐛 28 | 🌐 TypeScript | 📅 2026-08-19: An MCP server that bridges to multiple OpenAI-compatible LLMs - your AI rubber duck debugging panel  (14 tools) (node)
* [✅ korea-stock-mcp](https://github.com/jjlabsio/korea-stock-mcp) ⭐ 175 | 🐛 2 | 🌐 TypeScript | 📅 2026-09-01: MCP server for korea stock  (8 tools) (node)
* [✅ @toolsdk-remote/garden-stanislav-svelte-llm-svelte-llm-mcp](https://github.com/khromov/svelte-llm-mcp) ⭐ 160 | 🐛 7 | 🌐 TypeScript | 📅 2026-02-14: An MCP server that provides access to Svelte 5 and SvelteKit documentation  (2 tools) (node)
* [✅ mcp-image](https://github.com/shinpr/mcp-image) ⭐ 157 | 🐛 2 | 🌐 TypeScript | 📅 2026-08-30: AI image generation MCP server using Nano Banana Pro with intelligent prompt enhancement  (1 tools) (node)
* [✅ @toolsdk-remote/io-github-payram-payram-helper-mcp](https://github.com/PayRam/payram-helper-mcp-server) ⭐ 155 | 🐛 5 | 🌐 TypeScript | 📅 2026-08-19: Remote MCP server to integrate and validate self-hosted Payram deployments.  (35 tools) (node)
* [✅ hostinger-api-mcp](https://github.com/hostinger/api-mcp-server) ⭐ 148 | 🐛 5 | 🌐 TypeScript | 📅 2026-09-01: MCP server for Hostinger API  (118 tools) (node)
* [✅ minimax-mcp-js](https://github.com/minimax-ai/minimax-mcp-js) ⭐ 128 | 🐛 12 | 🌐 TypeScript | 📅 2026-08-21: Official JavaScript implementation that integrates with MiniMax's multimodal capabilities for image generation, video creation, text-to-speech, and voice cloning across multiple transport modes.  (10 tools) (node)
* [✅ fred-mcp-server](https://github.com/stefanoamorelli/fred-mcp-server) ⭐ 117 | 🐛 5 | 🌐 TypeScript | 📅 2026-08-22: Provides a bridge to the Federal Reserve Economic Data API for retrieving economic time series data like Overnight Reverse Repurchase Agreements and Consumer Price Index with customizable parameters for date ranges and sorting options.  (1 tools) (node)
* [✅ sub-agents-mcp](https://github.com/shinpr/sub-agents-mcp) ⭐ 96 | 🐛 0 | 🌐 TypeScript | 📅 2026-09-01: MCP server for delegating tasks to specialized AI assistants in Cursor, Claude, and Gemini  (1 tools) (node)
* [✅ clinicaltrialsgov-mcp-server](https://github.com/cyanheads/clinicaltrialsgov-mcp-server) ⭐ 91 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-21: Integrates with ClinicalTrials.gov REST API to search clinical trials by conditions, interventions, locations, and status, plus retrieve detailed study information by NCT ID with automatic data cleaning and local backup storage.  (2 tools) (node)
* [✅ anilist-mcp](https://github.com/yuna0x0/anilist-mcp) ⭐ 85 | 🐛 2 | 🌐 TypeScript | 📅 2026-07-13: MCP server that interfaces with the AniList API, allowing LLM clients to access and interact with anime, manga, character, staff, and user data from AniList  (44 tools) (node)
* [✅ taskqueue-mcp](https://github.com/chriscarrollsmith/taskqueue-mcp) ⭐ 70 | 🐛 17 | 🌐 TypeScript | 📅 2025-04-12: Structured task management system that breaks down complex projects into manageable tasks with progress tracking, user approval checkpoints, and support for multiple LLM providers.  (14 tools) (node)
* [✅ attio-mcp](https://github.com/kesslerio/attio-mcp-server) ⭐ 68 | 🐛 89 | 🌐 TypeScript | 📅 2026-09-01: AI-powered Attio CRM access. Manage contacts, companies, deals, tasks, notes and workflows.  (34 tools) (node)
* [✅ garth-mcp-server](https://github.com/matin/garth-mcp-server) ⭐ 63 | 🐛 2 | 🌐 Python | 📅 2026-02-01: Integrates with Garmin Connect to provide access to fitness and health data including sleep statistics, daily stress, and intensity minutes with customizable date ranges.  (30 tools) (python)
* [✅ yazio-mcp](https://github.com/fliptheweb/yazio-mcp) ⭐ 58 | 🐛 2 | 🌐 JavaScript | 📅 2026-08-13: MCP server for accessing Yazio user & nutrition data (unofficial)  (14 tools) (node)
* [✅ videodb-director-mcp](https://github.com/video-db/agent-toolkit/tree/HEAD/modelcontextprotocol) ⭐ 47 | 🐛 7 | 🌐 Python | 📅 2026-03-26: Bridges to VideoDB's video processing capabilities for searching, indexing, subtitling, and manipulating video content through specialized context resources.  (4 tools) (python)
* [✅ flowbite-mcp](https://github.com/themesberg/flowbite-mcp) ⭐ 41 | 🐛 1 | 🌐 JavaScript | 📅 2026-01-04: MCP server to convert Figma designs to Flowbite UI components in Tailwind CSS  (2 tools) (node)
* [✅ feuse-mcp](https://github.com/panzer-jack/feuse-mcp) ⭐ 38 | 🐛 4 | 🌐 TypeScript | 📅 2025-09-17: Automates Figma design-to-code workflows by extracting design data, downloading SVG assets, analyzing color variables, and generating API models with design token conversion for CSS frameworks like UnoCSS and TailwindCSS.  (8 tools) (node)
* [✅ @variflight-ai/variflight-mcp](https://github.com/variflight/variflight-mcp) ⭐ 31 | 🐛 3 | 🌐 JavaScript | 📅 2026-04-20: Integrates with Variflight API to provide real-time flight information, schedules, aircraft tracking, airport weather forecasts, and comfort metrics for travel planning and aviation monitoring applications.  (8 tools) (node)
* [✅ @toolsdk-remote/io-github-ksaklfszf921-riksdag-regering-mcp](https://github.com/KSAklfszf921/Riksdag-Regering-MCP) ⭐ 30 | 🐛 0 | 🌐 TypeScript | 📅 2026-05-07: Svenska Riksdagens och Regeringskansliets öppna data - 27 verktyg för politik, dokument och analys  (32 tools) (node)
* [✅ @toolsdk-remote/explorium-mcp](https://github.com/explorium-ai/mcp-explorium) ⭐ 21 | 🐛 5 | 🌐 Dockerfile | 📅 2026-03-03: Access live company and contact data from Explorium's AgentSource B2B platform.  (1 tools) (node)
* [✅ gologin-mcp](https://github.com/gologinapp/gologin-mcp) ⭐ 19 | 🐛 1 | 🌐 JavaScript | 📅 2026-06-19: Manage your GoLogin browser profiles and automation directly through AI conversations. This MCP server connects to the GoLogin API, letting you create, configure, and control browser profiles using natural language.  (59 tools) (node)
* [✅ etherscan-mcp](https://github.com/xiaok/etherscan-mcp) ⭐ 18 | 🐛 0 | 🌐 TypeScript | 📅 2025-09-09: Provides a bridge to the Etherscan API for querying Ethereum blockchain data including account balances, transactions, contracts, tokens, gas metrics, and network statistics.  (6 tools) (node)
* [✅ mcp-turso-cloud](https://github.com/spences10/mcp-turso-cloud) ⭐ 18 | 🐛 11 | 🌐 TypeScript | 📅 2026-08-29: Provides a bridge between AI assistants and Turso SQLite databases, enabling organization-level management and database-level queries with persistent context, schema exploration, and vector similarity search capabilities.  (9 tools) (node)
* [✅ inner-monologue-mcp](https://github.com/abhinav-mangla/inner-monologue-mcp) ⭐ 17 | 🐛 1 | 🌐 TypeScript | 📅 2025-08-12: An MCP (Model Context Protocol) server that implements a cognitive reasoning tool inspired by Google DeepMind's Inner Monologue research.  (1 tools) (node)
* [✅ tachibot-mcp](https://github.com/byPawel/tachibot-mcp) ⭐ 15 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-30: Multi-model AI orchestration with 31 tools, YAML workflows, and 5 token-optimized profiles.  (23 tools) (node)
* [✅ @toolsdk-remote/io-github-isakskogstad-kolada-mcp](https://github.com/isakskogstad/kolada-mcp) ⭐ 12 | 🐛 6 | 🌐 TypeScript | 📅 2026-04-22: Swedish municipality statistics from Kolada API. 6000+ KPIs for all 290 municipalities.  (21 tools) (node)
* [✅ meta-api-mcp-server](https://github.com/savhascelik/meta-api-mcp-server) ⭐ 12 | 🐛 0 | 🌐 JavaScript | 📅 2025-09-09: You can connect any API to LLMs. This enables AI to interact directly with APIs  (69 tools) (node)
* [✅ mcp-server-tempmail](https://github.com/Selenium39/mcp-server-tempmail) ⭐ 10 | 🐛 0 | 🌐 JavaScript | 📅 2025-09-09: MCP server for temporary email management using ChatTempMail API  (9 tools) (node)
* [✅ testdino-mcp](https://github.com/testdino-inc/testdino-mcp) ⭐ 9 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-10: A MCP server for TestDino  (6 tools) (node)
* [✅ mcp-pihole-server](https://github.com/aplaceforallmystuff/mcp-pihole) ⭐ 8 | 🐛 5 | 🌐 JavaScript | 📅 2026-08-12: Pi-hole v6 MCP server - manage DNS blocking, stats, whitelists/blacklists  (16 tools) (node)
* [✅ mcp-server-ens](https://github.com/justaname-id/ens-mcp-server) ⭐ 8 | 🐛 1 | 🌐 TypeScript | 📅 2026-01-13: Integrates with the Ethereum Name Service to resolve ENS names to addresses, perform lookups, retrieve records, check availability, get prices, and explore name history through configurable Ethereum network providers.  (8 tools) (node)
* [✅ @toolsdk-remote/io-github-isakskogstad-scb-mcp](https://github.com/isakskogstad/SCB-MCP) ⭐ 7 | 🐛 0 | 🌐 TypeScript | 📅 2026-04-22: MCP server for Statistics Sweden (SCB) - 1200+ tables with population, economy, environment data  (10 tools) (node)
* [✅ altmetric-mcp](https://github.com/altmetric/altmetric-mcp) ⭐ 7 | 🐛 0 | 🌐 JavaScript | 📅 2026-08-28: MCP server for Altmetric APIs - track research attention across news, policy, social media, and more  (9 tools) (node)
* [✅ gmail-mcp](https://github.com/vinayak-mehta/gmail-mcp) ⭐ 7 | 🐛 1 | 🌐 Python | 📅 2025-03-03: Integrates with Gmail to enable email search, retrieval, and interaction for natural language-driven email management and analysis tasks.  (6 tools) (python)
* [✅ mcp-cook](https://github.com/disdjj/mcp-cook) ⭐ 7 | 🐛 1 | 🌐 JavaScript | 📅 2025-04-16: Provides access to a collection of over 200 food and cocktail recipes, enabling dish information retrieval and ingredient-based meal suggestions.  (2 tools) (node)
* [✅ mcp-threatintel-server](https://github.com/aplaceforallmystuff/mcp-threatintel) ⭐ 7 | 🐛 5 | 🌐 JavaScript | 📅 2026-08-12: Unified threat intel - OTX, AbuseIPDB, GreyNoise, abuse.ch, Feodo Tracker  (17 tools) (node)
* [✅ qweather-mcp](https://github.com/overstarry/qweather-mcp) ⭐ 7 | 🐛 0 | 🌐 TypeScript | 📅 2026-05-10: a qweather mcp server  (9 tools) (node)
* [✅ selenium-webdriver-mcp](https://github.com/pshivapr/selenium-mcp) ⭐ 6 | 🐛 1 | 🌐 TypeScript | 📅 2025-10-12: Selenium Tools for MCP  (56 tools) (node)
* [✅ @toolsdk.ai/mixpanel-mcp-server](https://github.com/moonbirdai/mixpanel-mcp-server) ⭐ 5 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-17: A Model Context Protocol (MCP) server for integrating Mixpanel analytics into AI workflows. This server allows AI assistants like Claude to track events, page views, user signups, and update user profiles in Mixpanel.  (4 tools) (node)
* [✅ image-recognition-mcp](https://github.com/mcp-s-ai/image-recognition-mcp) ⭐ 5 | 🐛 0 | 🌐 JavaScript | 📅 2025-09-10: MCP server for AI-powered image recognition and description using OpenAI vision models.  (1 tools) (node)
* [✅ image-recongnition-mcp](https://github.com/mcp-s-ai/image-recongnition-mcp) ⭐ 5 | 🐛 0 | 🌐 JavaScript | 📅 2025-09-10: MCP server for AI-powered image recognition and description using OpenAI vision models.  (1 tools) (node)
* [✅ kubeview-mcp](https://github.com/mikhae1/kubeview-mcp) ⭐ 5 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-12: Read-only Model Context Protocol MCP server enabling code-driven AI analysis of Kubernetes clusters.  (10 tools) (node)
* [✅ math-mcp-learning-server](https://github.com/clouatre-labs/math-mcp-learning-server) ⭐ 5 | 🐛 1 | 🌐 Python | 📅 2026-09-01: Educational MCP server with 12 math/stats tools, visualizations, and persistent workspace  (17 tools) (python)
* [✅ mcp-zebrunner](https://github.com/maksimsarychau/mcp-zebrunner) ⭐ 5 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-21: Unified Zebrunner MCP server for TCM test cases, suites, coverage analysis, launchers, etc.  (6 tools) (node)
* [✅ mixpanel-mcp-server](https://github.com/moonbirdai/mixpanel-mcp-server) ⭐ 5 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-17: A Model Context Protocol (MCP) server for integrating Mixpanel analytics into AI workflows. This server allows AI assistants like Claude to track events, page views, user signups, and update user profiles in Mixpanel.  (4 tools) (node)
* [✅ tmdb-mcp-server](https://github.com/tcehjaava/tmdb-mcp-server) ⭐ 5 | 🐛 0 | 🌐 TypeScript | 📅 2025-10-08: MCP server for The Movie Database (TMDB) API  (13 tools) (node)
* [✅ kit-mcp-server](https://github.com/aplaceforallmystuff/mcp-kit) ⚠️ Archived: MCP server for Kit.com (ConvertKit) - manage subscribers, tags, sequences, broadcasts  (29 tools) (node)
* [✅ mcp-property-valuation-server](https://github.com/creis-ai/mcp-property-valuation-server) ⭐ 4 | 🐛 2 | 🌐 JavaScript | 📅 2025-12-05: MCP服务器，提供房产小区评级和评估功能  (3 tools) (node)
* [✅ starling-bank-mcp](https://github.com/domdomegg/starling-bank-mcp.git) ⭐ 4 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-05: Allow AI systems to view and control your Starling Bank account via MCP.  (24 tools) (node)
* [✅ base-network-mcp-server](https://github.com/fakepixels/base-mcp-server) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2025-03-04: Provides a bridge to the Base blockchain network for wallet management, balance checking, and transaction execution through natural language commands, eliminating the need to manage technical blockchain details.  (4 tools) (node)
* [✅ mcp-fathom-analytics](https://github.com/mackenly/mcp-fathom-analytics) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2025-06-17: Integrates with Fathom Analytics to retrieve account information, manage sites, track events, generate reports, and monitor real-time visitor data using the @mackenly/fathom-api SDK  (5 tools) (node)
* [✅ mcp-neo4j-cypher](https://github.com/guanxinyuan/neo4j) ⭐ 3 | 🐛 1 | 🌐 TypeScript | 📅 2025-03-14: Provides natural language interfaces to Neo4j graph databases for executing Cypher queries, storing knowledge graph data, and building persistent memory structures through conversational interactions.  (3 tools) (python)
* [✅ mcp-pickaxe](https://github.com/aplaceforallmystuff/mcp-pickaxe) ⚠️ Archived: MCP server for Pickaxe API - manage AI agents, knowledge bases, users, and analytics  (17 tools) (node)
* [✅ source-map-parser-mcp](https://github.com/masonchow/source-map-parser-mcp) ⭐ 2 | 🐛 0 | 🌐 TypeScript | 📅 2025-09-28: Maps minified JavaScript stack traces back to original source code locations for efficient production error debugging.  (2 tools) (node)
* [✅ todoist-mcp-server](https://github.com/stevengonsalvez/todoist-mcp) ⭐ 2 | 🐛 5 | 🌐 JavaScript | 📅 2026-02-19: Provides a bridge to the Todoist task management platform, enabling advanced project and task management capabilities like creating tasks, organizing projects, managing deadlines, and team collaboration.  (33 tools) (node)
* [✅ @withinfocus/tba-mcp-server](https://github.com/withinfocus/tba-mcp-server) ⭐ 1 | 🐛 5 | 🌐 TypeScript | 📅 2026-08-28: The Blue Alliance MCP Server  (61 tools) (node)
* [✅ linkly-mcp-server](https://github.com/Linkly-HQ/linkly-mcp-server) ⭐ 1 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-31: Create and manage short links, track clicks, and automate URL management  (19 tools) (node)
* [✅ uranium-tools-mcp](https://github.com/xkelxmc/uranium-mcp) ⭐ 1 | 🐛 4 | 🌐 TypeScript | 📅 2026-02-20: MCP for Uranium NFT tools to mint, list, and manage digital assets on the permaweb.  (4 tools) (node)
* [✅ @toolsdk-remote/io-cycloid-mcp-server](https://github.com/cycloidio/cycloid-mcp-server) ⭐ 0 | 🐛 0 | 🌐 Python | 📅 2026-06-09: An MCP server that let you interact with Cycloid.io Internal Development Portal and Platform  (6 tools) (node)
* [✅ ha-mcp-server](https://github.com/Koneisto/HomeAssistant-Light-MCP) ⭐ 0 | 🐛 0 | 🌐 JavaScript | 📅 2026-01-27: Control Home Assistant lights and scenes. Lights only by design for safety.  (11 tools) (node)
* [✅ welcome-text-generator-mcp](https://github.com/goodfel10w/WelcomeTextGenerator) ⭐ 0 | 🐛 0 | 🌐 TypeScript | 📅 2025-12-12: MCP Server für automatische Generierung professioneller Willkommenstexte für neue Mitarbeiter  (3 tools) (node)
* [✅ @toolsdk-remote/io-github-selisedigitalplatforms-l0-py-blocks-mcp](https://github.com/SELISEdigitalplatforms/l0-py-blocks-mcp): A Model Context Protocol (MCP) server for Selise Blocks Cloud integration  (36 tools) (node)
* [✅ @wildcard-ai/deepcontext](https://github.com/Wildcard-Official/deepcontext): Advanced codebase indexing and semantic search MCP server  (4 tools) (node)
* [✅ cerebras-code-mcp](https://github.com/kevint-cerebras/cerebras-code-mcp): Model Context Protocol (MCP) server for Cerebras to make coding faster in AI-first IDEs  (1 tools) (node)

</details>

<a id="aggregators"></a>

<details>
<summary><strong>Aggregators</strong></summary>

Servers that let you access multiple apps and tools through one MCP server.

* [✅ @modelcontextprotocol/server-everything](https://github.com/modelcontextprotocol/servers/tree/HEAD/src/everything) ⭐ 90,001 | 🐛 515 | 🌐 TypeScript | 📅 2026-08-31: Test protocol features and tools for client compatibility.  (8 tools) (node)
* [✅ @wopal/mcp-server-hotnews](https://github.com/wopal-cn/mcp-hotnews-server) ⭐ 201 | 🐛 0 | 📅 2024-12-22: Aggregates real-time trending topics from major Chinese social platforms and news sites.  (1 tools) (node)
* [✅ @illuminaresolutions/n8n-mcp-server](https://github.com/illuminaresolutions/n8n-mcp-server) ⭐ 119 | 🐛 5 | 🌐 TypeScript | 📅 2025-02-19: Bridges Claude with n8n automation workflows, enabling direct creation, execution, and management of workflows, credentials, and enterprise features without switching contexts.  (33 tools) (node)
* [✅ mcp-hub-mcp](https://github.com/warpdev/mcp-hub-mcp) ⭐ 62 | 🐛 2 | 🌐 TypeScript | 📅 2025-12-04: Centralizes multiple MCP servers into a unified hub, enabling seamless tool discovery and routing across specialized servers for complex workflows without managing individual connections.  (7 tools) (node)
* [✅ hal-mcp](https://github.com/deanward/hal) ⭐ 38 | 🐛 0 | 🌐 JavaScript | 📅 2025-10-22: Transforms OpenAPI/Swagger specifications into dynamic HTTP tools with secret management and URL restrictions, enabling secure API integration through automatic tool generation from API documentation.  (8 tools) (node)
* [✅ @pinkpixel/mindbridge](https://github.com/pinkpixel-dev/mindbridge-mcp) ⭐ 37 | 🐛 2 | 🌐 TypeScript | 📅 2026-03-13: Bridges multiple LLM providers including OpenAI, Anthropic, Google, DeepSeek, OpenRouter, and Ollama through a unified interface, enabling comparison of responses and leveraging specialized reasoning capabilities across different models.  (3 tools) (node)
* [✅ acp-mcp-server](https://github.com/gongrzhe/acp-mcp-server) ⚠️ Archived: Bridges Agent Communication Protocol networks with MCP clients, enabling access to complex multi-agent workflows through intelligent agent discovery, routing, and multi-modal message conversion with support for synchronous, asynchronous, and streaming execution patterns.  (16 tools) (python)
* [✅ @noveum-ai/mcp-server](https://github.com/noveum/api-market-mcp-server) ⭐ 4 | 🐛 0 | 🌐 TypeScript | 📅 2025-04-01: Converts OpenAPI specifications from API.market into tools for accessing over 200 services including image generation, geocoding, and content detection through a unified authentication system  (34 tools) (node)

</details>

<a id="art-and-culture"></a>

<details>
<summary><strong>Art & Culture</strong></summary>

Explore art collections, museums, and cultural heritage with AI-friendly tools.

* [✅ blender-mcp](https://github.com/ahujasid/blender-mcp) ⭐ 26,628 | 🐛 20 | 🌐 Python | 📅 2026-09-01: Enables natural language control of Blender for 3D scene creation, manipulation, and rendering without requiring knowledge of Blender's interface or Python API.  (17 tools) (python)
* [✅ ableton-mcp](https://github.com/ahujasid/ableton-mcp) ⭐ 2,951 | 🐛 34 | 🌐 Python | 📅 2026-08-30: Enables control of Ableton Live music production software through a bidirectional communication system that supports track creation, MIDI editing, playback control, instrument loading, and library browsing for music composition and sound design workflows.  (16 tools) (python)
* [✅ penpot-mcp](https://github.com/montevive/penpot-mcp) ⭐ 237 | 🐛 15 | 🌐 Python | 📅 2025-11-01: Integrates with Penpot's API to enable project browsing, file retrieval, object searching, and visual component export with automatic screenshot generation for converting UI designs into functional code.  (10 tools) (python)
* [✅ figma-mcp](https://github.com/matthewdailey/figma-mcp) ⭐ 213 | 🐛 0 | 🌐 TypeScript | 📅 2025-05-29: Interact with Figma design files through the Figma REST API for design analysis, feedback, and collaboration.  (5 tools) (node)
* [✅ minimax-mcp-js](https://github.com/minimax-ai/minimax-mcp-js) ⭐ 128 | 🐛 12 | 🌐 TypeScript | 📅 2026-08-21: Official JavaScript implementation that integrates with MiniMax's multimodal capabilities for image generation, video creation, text-to-speech, and voice cloning across multiple transport modes.  (10 tools) (node)
* [✅ discogs-mcp-server](https://github.com/cswkim/discogs-mcp-server) ⭐ 120 | 🐛 6 | 🌐 TypeScript | 📅 2026-07-06: Provides a bridge to the Discogs API for searching music databases, managing collections, and accessing marketplace listings with comprehensive artist and release information.  (53 tools) (node)
* [✅ replicate-flux-mcp](https://github.com/awkoy/replicate-flux-mcp) ⭐ 107 | 🐛 0 | 🌐 TypeScript | 📅 2026-06-14: Integrates with Replicate's Flux image generation model, enabling image creation capabilities within conversation interfaces through a simple API token setup and TypeScript implementation available as both an npm module and Docker container.  (7 tools) (node)
* [✅ grasshopper-mcp](https://github.com/alfredatnycu/grasshopper-mcp) ⭐ 97 | 🐛 8 | 🌐 C# | 📅 2025-03-22: Connects Grasshopper parametric design software with Claude through a bidirectional TCP server and Python bridge, enabling natural language control of architectural and engineering modeling workflows.  (8 tools) (python)
* [✅ nasa-mcp-server](https://github.com/programcomputer/nasa-mcp-server) ⭐ 92 | 🐛 0 | 🌐 TypeScript | 📅 2026-07-04: Integrates with NASA and JPL APIs to provide access to astronomy images, satellite data, space weather information, Mars rover photos, and more through a unified interface built with TypeScript.  (13 tools) (node)
* [✅ mcp-server-stability-ai](https://github.com/tadasant/mcp-server-stability-ai) ⭐ 84 | 🐛 9 | 🌐 TypeScript | 📅 2025-06-24: Integrates Stability AI's image generation and manipulation capabilities for editing, upscaling, and more via Stable Diffusion models.  (13 tools) (node)
* [✅ @jayarrowz/mcp-figma](https://github.com/thirdstrandstudio/mcp-figma) ⭐ 76 | 🐛 2 | 🌐 TypeScript | 📅 2026-02-21: Integrates with Figma's API to enable viewing, manipulating, and collaborating on design files through comprehensive access to file operations, comments, components, and team resources.  (31 tools) (node)
* [✅ @recraft-ai/mcp-recraft-server](https://github.com/recraft-ai/mcp-recraft-server) ⚠️ Archived: Integrates with Recraft's image generation API to create and edit raster and vector images, apply custom styles, manipulate backgrounds, upscale images, and perform vectorization with fine-grained control over artistic properties.  (9 tools) (node)
* [✅ sketchfab-mcp-server](https://github.com/gregkop/sketchfab-mcp-server) ⭐ 39 | 🐛 2 | 🌐 JavaScript | 📅 2025-03-09: Integrates with Sketchfab to enable searching, viewing details, and downloading 3D models in various formats using an API key for authentication.  (4 tools) (node)
* [✅ grok2-image-mcp-server](https://github.com/fl0w1nd/grok2-image-mcp-server) ⭐ 29 | 🐛 0 | 🌐 TypeScript | 📅 2026-03-03: Enables AI assistants to generate images through the Grok2 model using stdio transport for seamless integration into existing workflows.  (1 tools) (node)
* [✅ @cloudwerxlab/gpt-image-1-mcp](https://github.com/cloudwerx-dev/gpt-image-1-mcp) ⭐ 18 | 🐛 4 | 🌐 JavaScript | 📅 2025-05-07: Enables direct image generation and editing through OpenAI's gpt-image-1 model with support for text prompts, file paths, and base64 encoded inputs for creative workflows and visual content creation.  (2 tools) (node)
* [✅ mcp-openverse](https://github.com/neno-is-ooo/mcp-openverse) ⭐ 18 | 🐛 0 | 🌐 JavaScript | 📅 2025-06-12: Integrates with Openverse's Creative Commons image collection to search and retrieve openly-licensed images with detailed filtering options, attribution information, and specialized essay illustration features for finding relevant academic content.  (5 tools) (node)
* [✅ mcp-sonic-pi](https://github.com/vinayak-mehta/mcp-sonic-pi) ⭐ 18 | 🐛 1 | 🌐 Python | 📅 2025-03-22: Connects AI systems to the Sonic Pi music programming environment, enabling creation and control of musical compositions through Ruby code with features for playback, pattern access, and live coding.  (4 tools) (python)
* [✅ wikipedia-mcp](https://github.com/timjuenemann/wikipedia-mcp) ⭐ 15 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-18: Provides a structured interface for searching and retrieving Wikipedia articles in clean Markdown format, enabling access to up-to-date encyclopedia information without hallucinating facts.  (2 tools) (node)
* [✅ midi-file-mcp](https://github.com/xiaolaa2/midi-file-mcp) ⭐ 11 | 🐛 1 | 🌐 JavaScript | 📅 2025-05-09: Parse and manipulate MIDI files based on Tone.js  (11 tools) (node)
* [✅ together-mcp](https://github.com/manascb1344/together-mcp-server) ⭐ 10 | 🐛 3 | 🌐 JavaScript | 📅 2025-05-23: Integrates with Together AI's Flux.1 Schnell model to provide high-quality image generation with customizable dimensions, clear error handling, and optional image saving.  (1 tools) (node)
* [✅ @kailashg101/mcp-figma-to-code](https://github.com/kailashappdev/figma-mcp-toolkit) ⭐ 8 | 🐛 3 | 🌐 TypeScript | 📅 2025-03-24: Extracts and analyzes components from Figma design files, enabling seamless integration between Figma designs and React Native development through component hierarchy processing and metadata generation.  (3 tools) (node)
* [✅ @openmcprouter/mcp-server-ghibli-video](https://github.com/michaelyangjson/mcp-ghibli-video) ⭐ 4 | 🐛 2 | 🌐 TypeScript | 📅 2025-05-27: Transforms static images into animated Ghibli-style videos through the GPT4O Image Generator API with tools for credit balance checking and task monitoring.  (3 tools) (node)
* [✅ 4oimage-mcp](https://github.com/antipas/4oimage-mcp) ⭐ 4 | 🐛 1 | 🌐 JavaScript | 📅 2025-04-26: Provides a bridge between AI systems and the 4o-image API for generating and editing high-quality images through text prompts with real-time progress updates.  (1 tools) (node)
* [✅ sketchfab-mcp](https://github.com/eddydpyl/sketchfab_mcp) ⭐ 1 | 🐛 0 | 🌐 Python | 📅 2025-03-06: Provides a streamlined interface to the Sketchfab API for searching and downloading 3D models with filtering options for animated or rigged content.  (1 tools) (python)

</details>

<a id="browser-automation"></a>

<details>
<summary><strong>Browser Automation</strong></summary>

Tools for browsing, scraping, and automating web content in AI-compatible formats.

* [✅ mcp-server-fetch](https://github.com/modelcontextprotocol/servers/tree/HEAD/src/fetch) ⭐ 90,001 | 🐛 515 | 🌐 TypeScript | 📅 2026-08-31: Retrieve and convert web content to markdown for analysis.  (1 tools) (python)
* [✅ @playwright/mcp](https://github.com/microsoft/playwright-mcp) ⭐ 36,697 | 🐛 3 | 🌐 TypeScript | 📅 2026-09-01: A Model Context Protocol (MCP) server that provides browser automation capabilities using Playwright. This server enables LLMs to interact with web pages through structured accessibility snapshots, bypassing the need for screenshots or visually-tuned models.  (21 tools) (node)
* [✅ firecrawl-mcp](https://github.com/mendableai/firecrawl-mcp-server) ⭐ 7,362 | 🐛 124 | 🌐 TypeScript | 📅 2026-09-01: Integration with FireCrawl to provide advanced web scraping capabilities for extracting structured data from complex websites.  (8 tools) (node)
* [✅ @agentdeskai/browser-tools-mcp](https://github.com/AgentDeskAI/browser-tools-mcp) ⭐ 7,305 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-12: A Model Context Protocol (MCP) server that provides AI-powered browser tools integration. This server works in conjunction with the Browser Tools Server to provide AI capabilities for browser debugging and analysis.  (14 tools) (node)
* [✅ @executeautomation/playwright-mcp-server](https://github.com/executeautomation/mcp-playwright) ⭐ 5,638 | 🐛 35 | 🌐 TypeScript | 📅 2025-12-13: A Model Context Protocol server that provides browser automation capabilities using Playwright. This server enables LLMs to interact with web pages, take screenshots, generate test code, web scraps the page and execute JavaScript in a real browser environment.  (32 tools) (node)
* [✅ exa-mcp-server](https://github.com/exa-labs/exa-mcp-server) ⭐ 4,952 | 🐛 33 | 🌐 TypeScript | 📅 2026-08-21: A Model Context Protocol (MCP) server lets AI assistants like Claude use the Exa AI Search API for web searches. This setup allows AI models to get real-time web information in a safe and controlled way.  (6 tools) (node)
* [✅ fetcher-mcp](https://github.com/jae-jae/fetcher-mcp) ⭐ 1,079 | 🐛 12 | 🌐 TypeScript | 📅 2026-01-14: Fetches and extracts web content using Playwright's headless browser capabilities, delivering clean, readable content from JavaScript-heavy websites in HTML or Markdown format for research and information gathering.  (2 tools) (node)
* [✅ hyperbrowser-mcp](https://github.com/hyperbrowserai/mcp) ⭐ 789 | 🐛 10 | 🌐 TypeScript | 📅 2025-11-20: Enables web browsing capabilities through tools for content extraction, link following, and browser automation with customizable parameters for scraping, data collection, and web crawling tasks.  (10 tools) (node)
* [✅ @angiejones/mcp-selenium](https://github.com/angiejones/mcp-selenium) ⭐ 427 | 🐛 5 | 🌐 JavaScript | 📅 2026-02-23: Automates web browser actions with Selenium WebDriver.  (14 tools) (node)
* [✅ @automatalabs/mcp-server-playwright](https://github.com/automata-labs-team/mcp-server-playwright) ⭐ 299 | 🐛 10 | 🌐 JavaScript | 📅 2025-06-05: Control browsers to perform sophisticated web interactions and visual tasks.  (10 tools) (node)
* [✅ @modelcontextprotocol/server-puppeteer](https://github.com/modelcontextprotocol/servers-archived/tree/HEAD/src/puppeteer) ⚠️ Archived: Navigate websites, fill forms, and capture screenshots programmatically.  (7 tools) (node)
* [✅ @peng-shawn/mermaid-mcp-server](https://github.com/peng-shawn/mermaid-mcp-server) ⭐ 234 | 🐛 6 | 🌐 JavaScript | 📅 2025-06-21: Converts Mermaid diagrams to PNG images using Puppeteer for high-quality headless browser rendering, supporting multiple themes and customizable backgrounds.  (1 tools) (node)
* [✅ @just-every/mcp-read-website-fast](https://github.com/just-every/mcp-read-website-fast) ⭐ 161 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-29: Extracts web content and converts it to clean Markdown format using Mozilla Readability for intelligent article detection, with disk-based caching, robots.txt compliance, and concurrent crawling capabilities for fast content processing workflows.  (1 tools) (node)
* [✅ fetch-mcp](https://github.com/egoist/fetch-mcp) ⭐ 156 | 🐛 2 | 🌐 TypeScript | 📅 2025-04-04: Fetches web content and YouTube video transcripts, converting HTML to Markdown and extracting timestamps for reference in conversations.  (2 tools) (node)
* [✅ @browserstack/mcp-server](https://github.com/browserstack/mcp-server) ⭐ 150 | 🐛 44 | 🌐 TypeScript | 📅 2026-09-01: Integrates with BrowserStack's testing infrastructure to enable automated and manual testing across browsers, devices, and platforms for debugging cross-browser issues and verifying mobile app functionality.  (20 tools) (node)
* [✅ scrapling-fetch-mcp](https://github.com/cyberchitta/scrapling-fetch-mcp) ⭐ 113 | 🐛 0 | 🌐 Python | 📅 2026-08-02: Enables AI to access text content from websites protected by bot detection mechanisms through three protection levels (basic, stealth, max-stealth), retrieving complete pages or specific content patterns without manual copying.  (2 tools) (python)
* [✅ @debugg-ai/debugg-ai-mcp](https://github.com/debugg-ai/debugg-ai-mcp) ⭐ 68 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-20: Provides zero-configuration end-to-end testing for web applications by creating secure tunnels to local development servers and spawning testing agents that interact with web interfaces through natural language descriptions, returning detailed test results with execution recordings and screenshots.  (1 tools) (node)
* [✅ vibe-eyes](https://github.com/monteslu/vibe-eyes) ⭐ 54 | 🐛 1 | 🌐 JavaScript | 📅 2026-02-04: Enables LLMs to visualize and debug browser-based games and applications by capturing canvas content, console logs, and errors, then processing visual data into compact SVG representations for seamless debugging.  (1 tools) (node)
* [✅ mcp-rquest](https://github.com/xxxbrian/mcp-rquest) ⭐ 48 | 🐛 3 | 🌐 Python | 📅 2025-03-22: Enables LLMs to make advanced HTTP requests with realistic browser emulation, bypassing anti-bot measures while supporting all HTTP methods, authentication, and automatic response handling for web scraping and API interactions.  (10 tools) (python)
* [✅ @kazuph/mcp-fetch](https://github.com/kazuph/mcp-fetch) ⭐ 41 | 🐛 4 | 🌐 JavaScript | 📅 2026-06-12: Integrates web scraping and image processing capabilities to fetch, extract, and optimize web content.  (1 tools) (node)
* [✅ @tokenizin/mcp-npx-fetch](https://github.com/tokenizin-agency/mcp-npx-fetch) ⭐ 41 | 🐛 3 | 🌐 TypeScript | 📅 2024-12-26: Fetches and converts web content to Markdown using JSDOM and Turndown.  (4 tools) (node)
* [✅ mcp-server-weibo](https://github.com/selenium39/mcp-server-weibo) ⭐ 33 | 🐛 1 | 🌐 TypeScript | 📅 2025-09-09: Enables scraping of Weibo user information, feeds, and search functionality with tools for user discovery, profile retrieval, and feed access  (5 tools) (node)
* [✅ playwright-mcp](https://github.com/ashish-bansal/playwright-mcp) ⭐ 32 | 🐛 5 | 🌐 TypeScript | 📅 2025-11-05: Playwright MCP enables browser automation and interaction recording by capturing DOM interactions, screenshots, and page navigation events to generate reproducible test scripts through a visual, context-driven workflow.  (5 tools) (node)
* [✅ mcp-jinaai-reader](https://github.com/spences10/mcp-jinaai-reader) ⚠️ Archived: Extracts and processes web content for efficient parsing and analysis of online information  (1 tools) (node)
* [✅ @cmann50/mcp-chrome-google-search](https://github.com/cmann50/mcp-chrome-google-search) ⭐ 25 | 🐛 1 | 🌐 HTML | 📅 2025-02-01: Integrates Google search and webpage content extraction via Chrome browser automation, enabling access up-to-date web information for tasks like fact-checking and research.  (2 tools) (node)
* [✅ @octomind/octomind-mcp](https://github.com/octomind-dev/octomind-mcp) ⚠️ Archived: Enables AI-driven test automation through the Octomind platform for creating, executing, and analyzing end-to-end tests without leaving your development environment.  (19 tools) (node)
* [✅ blowback-context](https://github.com/esnark/blowback) ⭐ 23 | 🐛 3 | 🌐 TypeScript | 📅 2026-03-23: Integrates with frontend development environments to provide real-time feedback and debugging capabilities through browser automation, capturing console logs, monitoring HMR events, and enabling DOM interaction without leaving the conversation interface.  (11 tools) (node)
* [✅ gologin-mcp](https://github.com/gologinapp/gologin-mcp) ⭐ 19 | 🐛 1 | 🌐 JavaScript | 📅 2026-06-19: Manage your GoLogin browser profiles and automation directly through AI conversations. This MCP server connects to the GoLogin API, letting you create, configure, and control browser profiles using natural language.  (59 tools) (node)
* [✅ @deventerprisesoftware/scrapi-mcp](https://github.com/deventerprisesoftware/scrapi-mcp) ⭐ 18 | 🐛 2 | 🌐 JavaScript | 📅 2026-08-06: Enables web scraping from sites with bot detection, captchas, or geolocation restrictions through residential proxies and automated captcha solving for content extraction in HTML or Markdown formats.  (2 tools) (node)
* [✅ mcp-server-chatgpt-app](https://github.com/cdpath/mcp-server-chatgpt-app) ⭐ 17 | 🐛 2 | 🌐 Python | 📅 2025-07-10: Enables interaction with the ChatGPT macOS app through AppleScript automation, allowing tools to send prompts via keyboard input simulation without switching interfaces.  (1 tools) (python)
* [✅ @kazuph/mcp-browser-tabs](https://github.com/kazuph/mcp-browser-tabs) ⭐ 14 | 🐛 2 | 🌐 JavaScript | 📅 2026-06-12: Integrates with Chrome on macOS to retrieve and manage browser tab information using AppleScript.  (4 tools) (node)
* [✅ mcp-node-fetch](https://github.com/mcollina/mcp-node-fetch) ⭐ 11 | 🐛 3 | 🌐 TypeScript | 📅 2025-03-10: Enables web content retrieval and processing with tools for fetching URLs, extracting HTML fragments, and checking site availability using Node.js's undici library.  (3 tools) (node)
* [✅ mcp-playwright-scraper](https://github.com/dennisgl/mcp-playwright-scraper) ⭐ 11 | 🐛 2 | 🌐 Python | 📅 2025-03-09: Leverages Playwright and BeautifulSoup to enable robust web scraping and content extraction, converting complex JavaScript-heavy web pages into high-quality Markdown with browser automation capabilities.  (1 tools) (python)
* [✅ chrome-debug-mcp](https://github.com/rainmen-xia/chrome-debug-mcp) ⭐ 10 | 🐛 0 | 🌐 JavaScript | 📅 2025-06-30: Provides browser automation capabilities through Chrome's debugging protocol with session persistence, enabling web scraping, testing, and automation tasks with tools for screenshots, navigation, element interaction, and content retrieval.  (10 tools) (node)
* [✅ hyper-mcp-browser](https://github.com/bigsweetpotatostudio/hyper-mcp-browser) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2025-03-29: Enables web browsing capabilities through Puppeteer and Chrome, allowing navigation, content extraction, and interaction with websites for scraping, analysis, and automated testing workflows.  (2 tools) (node)
* [✅ mcp-cookie-server](https://github.com/bnookala/mcp-cookiejar) ⭐ 2 | 🐛 0 | 🌐 JavaScript | 📅 2025-06-22: Provides cookie management capabilities for web automation and testing workflows, enabling storage, retrieval, and manipulation of session state and authentication cookies across different web services.  (6 tools) (node)
* [✅ @kwp-lab/mcp-fetch](https://github.com/kwp-lab/mcp-fetch) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2025-05-11: A Model Context Protocol server that provides web content fetching capabilities  (1 tools) (node)
* [✅ mcp-jinaai-grounding](https://github.com/spences10/mcp-jinaai-grounding) ⚠️ Archived: Integrates JinaAI's content extraction and analysis capabilities for web scraping, documentation parsing, and text analysis tasks.  (1 tools) (node)
* [✅ mcp-web-content-pick](https://github.com/kilicmu/mcp-web-content-pick) ⭐ 1 | 🐛 0 | 🌐 TypeScript | 📅 2025-03-05: Extracts structured content from web pages using customizable selectors for crawling, parsing, and analyzing HTML elements without leaving the assistant interface.  (1 tools) (node)

</details>

<a id="cloud-platforms"></a>

<details>
<summary><strong>Cloud Platforms</strong></summary>

Integrate with cloud services to manage and interact with cloud infrastructure.

* [✅ awslabs.cdk-mcp-server](https://github.com/awslabs/mcp/tree/HEAD/src/cdk-mcp-server) ⭐ 9,652 | 🐛 247 | 🌐 Python | 📅 2026-09-01: Integration for AWS Cloud Development Kit (CDK) best practices, infrastructure as code patterns, and security compliance with CDK Nag.  (7 tools) (python)
* [✅ mcp-server-kubernetes](https://github.com/Flux159/mcp-server-kubernetes) ⭐ 1,578 | 🐛 8 | 🌐 TypeScript | 📅 2026-08-31: MCP server for managing Kubernetes clusters, enabling LLMs to interact with and control Kubernetes resources.  (22 tools) (node)
* [✅ @cloudbase/cloudbase-mcp](https://github.com/tencentcloudbase/cloudbase-ai-toolkit) ⭐ 1,088 | 🐛 1 | 🌐 TypeScript | 📅 2026-09-01: Enables AI systems to deploy, monitor, and manage full-stack applications on Tencent CloudBase through tools for cloud environments, databases, functions, hosting services, and storage resources.  (39 tools) (node)
* [✅ @masonator/coolify-mcp](https://github.com/stumason/coolify-mcp) ⭐ 574 | 🐛 23 | 🌐 TypeScript | 📅 2026-09-01: Integrates with Coolify to enable natural language management of servers, projects, applications, and databases through the Coolify API, allowing users to perform DevOps operations without leaving their conversation interface.  (5 tools) (node)
* [✅ edgeone-pages-mcp](https://github.com/tencentedgeone/edgeone-pages-mcp) ⭐ 433 | 🐛 9 | 🌐 TypeScript | 📅 2026-07-07: Enables rapid deployment of HTML content to Tencent's EdgeOne Pages service with integrated Functions and KV store support for edge hosting  (2 tools) (node)
* [✅ @strowk/mcp-k8s](https://github.com/strowk/mcp-k8s-go) ⭐ 386 | 🐛 11 | 🌐 Go | 📅 2025-12-22: Control and monitor K8s clusters for management and debugging.  (8 tools) (node)
* [✅ alibabacloud-mcp-server](https://github.com/aliyun/alibaba-cloud-ops-mcp-server) ⭐ 128 | 🐛 4 | 🌐 Python | 📅 2026-03-16: Provides a bridge to Alibaba Cloud services for managing ECS instances, viewing resources, monitoring metrics, and configuring VPC networks through natural language commands  (26 tools) (python)
* [✅ google-cloud-mcp](https://github.com/krzko/google-cloud-mcp) ⭐ 82 | 🐛 8 | 🌐 TypeScript | 📅 2025-12-15: Integrates with Google Cloud services to provide direct access to Logging, Spanner, and Monitoring resources within conversations through authenticated connections.  (17 tools) (node)
* [✅ @digitalocean/mcp](https://github.com/digitalocean/digitalocean-mcp) ⚠️ Archived: Integrates with DigitalOcean's cloud platform API to enable management of cloud resources, deployment of applications, and monitoring of infrastructure through natural language commands.  (32 tools) (node)
* [✅ @netlify/mcp](https://github.com/netlify/netlify-mcp) ⭐ 59 | 🐛 13 | 🌐 TypeScript | 📅 2026-08-13: Integrates with Netlify's platform for complete site management including project operations, deployments with zip uploads, team administration, extension configuration, and documentation access across hosting, build, and collaboration workflows.  (6 tools) (node)
* [✅ coolify-mcp-server](https://github.com/wrediam/coolify-mcp-server) ⭐ 45 | 🐛 1 | 🌐 JavaScript | 📅 2026-08-13: Enables comprehensive Coolify infrastructure management by exposing tools for creating, deploying, and tracking servers, applications, and team resources with robust operational capabilities.  (26 tools) (node)
* [✅ apisix-mcp](https://github.com/api7/apisix-mcp) ⭐ 38 | 🐛 2 | 🌐 TypeScript | 📅 2025-06-16: Bridge LLMs with the APISIX Admin API to manage and analyze API gateway information.  (32 tools) (node)
* [✅ aws-s3-mcp](https://github.com/samuraikun/aws-s3-mcp) ⭐ 30 | 🐛 12 | 🌐 TypeScript | 📅 2026-02-14: Provides direct access to Amazon S3 storage for listing buckets, browsing objects, and retrieving file contents with automatic text extraction from PDFs and other file types.  (3 tools) (node)
* [✅ mcp-server-esa](https://github.com/aliyun/mcp-server-esa) ⭐ 26 | 🐛 14 | 🌐 TypeScript | 📅 2026-05-18: Provides a bridge to Alibaba Cloud's Edge Security Acceleration service for managing edge routines, deployments, routes, and sites through authenticated API operations.  (23 tools) (node)
* [✅ alibabacloud-fc-mcp-server](https://github.com/aliyun/alibabacloud-fc-mcp-server) ⭐ 9 | 🐛 8 | 🌐 JavaScript | 📅 2026-02-14: Integrates with Alibaba Cloud Function Compute to deploy and manage serverless functions with multi-language runtime support, custom domain routing, and VPC configuration for automated cloud function lifecycle management.  (12 tools) (node)
* [✅ @felixallistar/coolify-mcp](https://github.com/felixallistar/coolify-mcp) ⚠️ Archived: Integrates with Coolify's deployment platform to manage self-hosted applications, databases, and infrastructure including 110+ one-click services, 8 database types, server connectivity validation, and environment variable handling.  (10 tools) (node)
* [✅ @osaas/mcp-server](https://github.com/eyevinnosc/mcp-server) ⚠️ Archived: EyevinnOSC's MCP server enables AI assistants to provision and manage vendor-independent cloud infrastructure for databases, storage, and media processing through an open source API.  (3 tools) (node)
* [✅ multicluster-mcp-server](https://github.com/yanmxa/multicluster-mcp-server) ⭐ 4 | 🐛 2 | 🌐 Python | 📅 2025-07-08: Provides a bridge to Kubernetes multi-cluster environments for managing distributed resources through kubectl commands, service account connections, and seamless cross-cluster operations without switching contexts.  (4 tools) (node)
* [✅ akave-mcp-js](https://github.com/akave-ai/akave-mcp) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2025-05-29: Integrates with Akave's S3-compatible storage platform to manage buckets and objects, upload/download files, generate signed URLs, and handle file operations with automatic text cleaning for common formats.  (13 tools) (node)
* [✅ aliyun-mcp-server](https://github.com/nailuogg/aliyun-mcp-server) ⭐ 2 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-26: Integrates with Alibaba Cloud services to query and filter SLS logs, with future support for ECS instance management and serverless function deployment.  (1 tools) (node)
* [✅ cloudinary-mcp-server](https://github.com/yoavniran/cloudinary-mcp-server) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-26: Provides direct access to Cloudinary's Upload and Admin APIs for uploading, retrieving, searching, and managing digital media assets in your Cloudinary cloud.  (5 tools) (node)

</details>

<a id="code-execution"></a>

<details>
<summary><strong>Code Execution</strong></summary>

Run code securely, perfect for coding agents and AI-driven programming tasks.

* [✅ gemini-mcp-tool](https://github.com/jamubc/gemini-mcp-tool) ⭐ 2,278 | 🐛 28 | 🌐 TypeScript | 📅 2026-07-21: Integrates with Google's Gemini CLI to leverage massive token windows for analyzing large files and codebases, providing general queries, sandbox-mode code execution for safe testing, and structured response handling with behavioral flags for context control.  (6 tools) (node)
* [✅ @e2b/mcp-server](https://github.com/e2b-dev/mcp-server/tree/HEAD/packages/js) ⚠️ Archived: A Model Context Protocol server for running code in a secure sandbox by E2B.  (1 tools) (node)
* [✅ mcp-server-code-runner](https://github.com/formulahendry/mcp-server-code-runner) ⭐ 244 | 🐛 17 | 🌐 TypeScript | 📅 2026-02-05: Executes code snippets in over 30 programming languages by creating temporary files and running them with appropriate interpreters, enabling direct testing and demonstration within conversations.  (1 tools) (node)
* [✅ mcp-llm](https://github.com/sammcj/mcp-llm) ⭐ 78 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-15: Integrates with LlamaIndexTS to provide access to various LLM providers for code generation, documentation writing, and question answering tasks  (4 tools) (node)
* [✅ python-local](https://github.com/alec2435/python_mcp) ⭐ 54 | 🐛 4 | 🌐 Python | 📅 2024-12-04: Provides an interactive Python REPL environment for executing code within conversations, maintaining separate state for each session and supporting both expressions and statements.  (1 tools) (python)
* [✅ mcp-python](https://github.com/hdresearch/mcp-python) ⭐ 42 | 🐛 3 | 🌐 Python | 📅 2025-12-15: Provides a persistent Python execution environment for interactive code development, data analysis, and rapid prototyping.  (3 tools) (python)
* [✅ nrepl-mcp-server](https://github.com/johancodinha/nrepl-mcp-server) ⭐ 37 | 🐛 2 | 🌐 TypeScript | 📅 2025-06-18: Integrates with Clojure nREPL instances to enable code evaluation, namespace listing, and public var inspection for AI-assisted Clojure development.  (3 tools) (node)
* [✅ @riza-io/riza-mcp](https://github.com/riza-io/riza-mcp) ⭐ 13 | 🐛 3 | 🌐 JavaScript | 📅 2024-12-17: Provides a secure bridge between LLMs and Riza's isolated code interpreter API, enabling writing, saving, editing, and executing code safely in a sandboxed environment with persistent tool management across conversations.  (6 tools) (node)
* [✅ js-sandbox-mcp-server](https://github.com/garc33/js-sandbox-mcp-server) ⭐ 5 | 🐛 0 | 🌐 JavaScript | 📅 2025-10-10: Provides a secure JavaScript sandbox for executing code with configurable time and memory limits, enabling safe testing and evaluation of algorithms.  (1 tools) (node)
* [✅ sandock-mcp](https://github.com/sandock-ai/sandock) ⭐ 4 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-26: A Model Context Protocol server for running code in a secure sandbox by Sandock.  (6 tools) (node)
* [✅ node-code-sandbox-mcp](https://github.com/ssdeanx/node-code-sandbox-mcp) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2025-05-21: Provides a secure Docker-based environment for executing Node.js code with npm dependencies, shell commands, and file operations while maintaining proper isolation for testing and web development prototyping.  (7 tools) (node)

</details>

<a id="coding-agents"></a>

<details>
<summary><strong>Coding Agents</strong></summary>

AI tools that can autonomously read, write, and execute code to solve programming tasks.

* [✅ @steipete/claude-code-mcp](https://github.com/steipete/claude-code-mcp) ⚠️ Archived: Provides a streamlined interface for executing complex coding tasks including file operations, Git commands, and web searches without permission interruptions by automatically bypassing constraints.  (1 tools) (node)
* [✅ mcp-neovim-server](https://github.com/bigcodegen/mcp-neovim-server) ⭐ 318 | 🐛 9 | 🌐 TypeScript | 📅 2025-10-11: Integrates Claude Desktop with Neovim, enabling AI-enhanced coding assistance within the familiar Vim environment through direct interaction with buffers and commands.  (19 tools) (node)
* [✅ mcp-coco](https://github.com/disdjj/mcp-coco) ⭐ 4 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-18: MCP-Coco provides a pair programming tool that guides technical discussions by transforming code snippets into structured frameworks for critical inquiry about performance, security, and maintainability.  (1 tools) (node)
* [✅ mcp-server-code-assist](https://github.com/abhishekbhakat/mcp_server_code_assist) ⚠️ Archived: Enables code modification and generation tasks through file operations, search-and-replace, and version control integration for automated refactoring and codebase maintenance.  (17 tools) (python)

</details>

<a id="command-line"></a>

<details>
<summary><strong>Command Line</strong></summary>

Run shell commands and interact with command-line tools easily.

* [✅ @steipete/peekaboo-mcp](https://github.com/steipete/peekaboo) ⭐ 5,096 | 🐛 9 | 🌐 Swift | 📅 2026-08-31: Enables macOS screen capture and window management with tools for taking screenshots, analyzing images, and controlling application windows  (3 tools) (node)
* [✅ @steipete/macos-automator-mcp](https://github.com/steipete/macos-automator-mcp) ⭐ 874 | 🐛 0 | 🌐 TypeScript | 📅 2026-09-01: Automates macOS tasks through AppleScript and JavaScript for Automation with a rich library of pre-defined scripts for application control, file operations, and system interactions.  (2 tools) (node)
* [✅ wcgw](https://github.com/rusiaaman/wcgw/tree/HEAD/src/wcgw/client/mcp_server) ⭐ 673 | 🐛 7 | 🌐 Python | 📅 2026-08-07: Access shell and filesystem in order to automate tasks and run code  (6 tools) (python)
* [✅ iterm-mcp](https://github.com/ferrislucas/iterm-mcp) ⭐ 567 | 🐛 9 | 🌐 TypeScript | 📅 2025-09-20: Enables direct execution of shell commands in the active iTerm tab, streamlining terminal-based workflows and automation tasks.  (3 tools) (node)
* [✅ @peakmojo/applescript-mcp](https://github.com/peakmojo/applescript-mcp) ⭐ 462 | 🐛 4 | 🌐 Python | 📅 2026-02-22: Enables AI to execute AppleScript code on macOS systems, providing access to applications and system features like Notes, Calendar, Contacts, Messages, and Finder through a lightweight server implementation.  (1 tools) (node)
* [✅ @simonb97/server-win-cli](https://github.com/simonb97/win-cli-mcp-server) ⚠️ Archived: Control Windows command-line interfaces securely.  (9 tools) (node)
* [✅ phone-mcp](https://github.com/hao-cyber/phone-mcp) ⭐ 244 | 🐛 10 | 🌐 Python | 📅 2025-05-08: Enables remote control of Android phones through ADB commands for making calls, sending texts, taking screenshots, managing contacts, launching apps, and retrieving system information.  (21 tools) (python)
* [✅ mcp-server-commands](https://github.com/g0t4/mcp-server-commands) ⭐ 232 | 🐛 8 | 🌐 TypeScript | 📅 2026-08-24: Execute system commands and scripts on the host machine.  (1 tools) (node)
* [✅ mcp-server-siri-shortcuts](https://github.com/dvcrn/mcp-server-siri-shortcuts) ⭐ 192 | 🐛 6 | 🌐 TypeScript | 📅 2026-08-29: Integrates with macOS Shortcuts to dynamically expose and execute user-defined automation workflows through generated tools.  (3 tools) (node)
* [✅ mcp-shell-server](https://github.com/tumf/mcp-shell-server) ⭐ 191 | 🐛 0 | 🌐 Python | 📅 2026-08-15: Execute whitelisted shell commands on the host system via asyncio.  (1 tools) (python)
* [✅ shell-command-mcp](https://github.com/egoist/shell-command-mcp) ⭐ 60 | 🐛 3 | 🌐 TypeScript | 📅 2025-04-07: Secure shell command execution server that allows running system commands in a controlled environment through an allowlist system, returning results in YAML format.  (1 tools) (node)
* [✅ mcp-shell](https://github.com/hdresearch/mcp-shell) ⭐ 41 | 🐛 6 | 🌐 JavaScript | 📅 2025-12-15: Secure shell command execution server for AI models to interact with local systems while maintaining strict security controls.  (1 tools) (node)
* [✅ macos-notification-mcp](https://github.com/devizor/macos-notification-mcp) ⭐ 37 | 🐛 2 | 🌐 Python | 📅 2025-05-09: Enables macOS system notifications, banner alerts, and text-to-speech capabilities with customizable parameters like voice selection and speech rate.  (5 tools) (python)
* [✅ apple-notifier-mcp](https://github.com/turlockmike/apple-notifier-mcp) ⭐ 26 | 🐛 0 | 🌐 TypeScript | 📅 2026-01-21: Enables interaction with macOS notifications and system dialogs for desktop alerts, user input, and system operations.  (5 tools) (node)
* [✅ server-cmd](https://github.com/phialsbasement/cmd-mcp-server) ⭐ 25 | 🐛 3 | 🌐 JavaScript | 📅 2025-02-14: Cross-platform MCP server for executing command-line operations and SSH connections on Windows and Linux systems through a standardized interface.  (2 tools) (node)
* [✅ mcp-wsl-exec](https://github.com/spences10/mcp-wsl-exec) ⭐ 22 | 🐛 12 | 🌐 TypeScript | 📅 2026-09-01: Provides secure command execution in WSL with built-in safety features like path validation, timeouts, and error handling.  (2 tools) (node)
* [✅ mcp-kubernetes-server](https://github.com/feiskyer/mcp-kubernetes-server) ⭐ 20 | 🐛 3 | 🌐 Python | 📅 2026-02-04: Enables direct Kubernetes cluster management through kubectl command execution, providing a bridge for real-time resource administration within conversations.  (1 tools) (python)
* [✅ super-shell-mcp](https://github.com/cfdude/super-shell-mcp) ⭐ 20 | 🐛 4 | 🌐 JavaScript | 📅 2026-08-13: Enables secure execution of shell commands across Windows, macOS, and Linux with a three-tier whitelist security model for controlled system access.  (9 tools) (node)
* [✅ mcp-apple-calendars](https://github.com/shadowfax92/apple-calendar-mcp) ⭐ 15 | 🐛 2 | 🌐 TypeScript | 📅 2025-03-12: Provides a TypeScript-based server for reading, creating, updating, and deleting macOS calendar events through a local HTTP bridge, enabling seamless scheduling and calendar management for desktop applications.  (7 tools) (node)
* [✅ iterm\_mcp\_server](https://github.com/rishabkoul/iterm-mcp-server) ⭐ 14 | 🐛 3 | 🌐 JavaScript | 📅 2025-03-23: Enables AI interaction with iTerm2 terminals on macOS through AppleScript and Node.js, allowing command execution, output capture, and terminal management without context switching.  (5 tools) (node)
* [✅ mcp-cli-exec](https://github.com/jakenuts/mcp-cli-exec) ⭐ 12 | 🐛 2 | 🌐 TypeScript | 📅 2025-03-05: Provides powerful CLI command execution capabilities, enabling structured output for shell commands with features like timeout handling, ANSI code stripping, and error management for system administration and DevOps workflows.  (2 tools) (node)
* [✅ @rinardnick/mcp-terminal](https://github.com/rinardnick/mcp-terminal) ⭐ 11 | 🐛 1 | 🌐 JavaScript | 📅 2025-01-20: Provides a secure terminal server for executing whitelisted shell commands with strict resource controls and security boundaries.  (1 tools) (node)
* [✅ mcp-server-macos-defaults](https://github.com/g0t4/mcp-server-macos-defaults) ⭐ 11 | 🐛 2 | 🌐 Python | 📅 2024-11-30: Enables interaction with macOS system preferences via the 'defaults' command for querying and modifying configurations.  (4 tools) (python)
* [✅ @devyhan/xcode-mcp](https://github.com/devyhan/xcode-mcp) ⭐ 9 | 🐛 1 | 🌐 JavaScript | 📅 2025-03-28: Provides Xcode-related command-line tools to enables project inspection, building, testing, archiving, code signing, and Xcode simulator management through natural language commands.  (9 tools) (node)
* [✅ os-info-mcp-server](https://github.com/anurag-dhamala/os-info-mcp-server) ⭐ 1 | 🐛 0 | 🌐 TypeScript | 📅 2025-04-19: Provides real-time system information about the host computer, including CPU, memory, operating system, disk, battery, and process details for monitoring resources and troubleshooting performance issues.  (1 tools) (node)
* [✅ perm-shell-mcp](https://github.com/mcollina/perm-shell-mcp) ⭐ 1 | 🐛 3 | 🌐 TypeScript | 📅 2025-04-10: Enables secure execution of shell commands through desktop notifications that require explicit user approval for each operation, maintaining strong security boundaries for local system access.  (2 tools) (node)
* [✅ @kevinwatt/shell-mcp](https://github.com/kevinwatt/shell-mcp): Provides a shell command execution interface for secure and controlled access to local system operations, enabling automation tasks and system management.  (20 tools) (node)

</details>

<a id="communication"></a>

<details>
<summary><strong>Communication</strong></summary>

Connect with messaging platforms to manage chats and interact with team tools.

* [✅ @toolsdk.ai/mcp-send-email](https://github.com/resend/mcp-send-email) ⭐ 566 | 🐛 14 | 🌐 TypeScript | 📅 2026-09-01: Integrates with the Resend API to enable sending plain text emails with scheduling options and configurable reply-to addresses through command-line or environment variable configuration.  (1 tools) (node)
* [✅ @enescinar/twitter-mcp](https://github.com/enescinr/twitter-mcp) ⭐ 401 | 🐛 3 | 🌐 TypeScript | 📅 2025-07-17: Interact with X (Twitter) by posting tweets and searching for tweets through the X API.  (2 tools) (node)
* [✅ @modelcontextprotocol/server-slack](https://github.com/modelcontextprotocol/servers-archived/tree/HEAD/src/slack) ⚠️ Archived: Send messages, manage channels, and access workspace history.  (8 tools) (node)
* [✅ @greirson/mcp-todoist](https://github.com/greirson/mcp-todoist) ⭐ 243 | 🐛 19 | 🌐 TypeScript | 📅 2026-05-01: Integrates with Todoist API to manage tasks, projects, sections, and comments with support for bulk operations, natural language search, and comprehensive CRUD functionality.  (28 tools) (node)
* [✅ @floriscornel/teams-mcp](https://github.com/floriscornel/teams-mcp) ⭐ 133 | 🐛 32 | 🌐 TypeScript | 📅 2026-05-23: Integrates with Microsoft Teams through Graph API to search messages, manage chats and channels, send messages, create group chats, and handle user/team operations with device code authentication for secure access.  (19 tools) (node)
* [✅ wecom-bot-mcp-server](https://github.com/loonghao/wecom-bot-mcp-server) ⭐ 102 | 🐛 10 | 🌐 Python | 📅 2026-08-30: Integrates WeCom (WeChat Work) bot functionality for enterprise messaging, notifications, and interactive chatbots.  (1 tools) (python)
* [✅ mcp-server-email](https://github.com/shy2593666979/mcp-server-email) ⭐ 80 | 🐛 11 | 🌐 Python | 📅 2025-04-27: Enables language models to compose and send emails with attachments through SMTP servers, supporting multiple providers and secure transmission for automated email workflows.  (2 tools) (python)
* [✅ ntfy-me-mcp](https://github.com/gitmotion/ntfy-me-mcp) ⭐ 72 | 🐛 3 | 🌐 TypeScript | 📅 2026-04-11: Enables sending push notifications through the ntfy service with customizable titles, summaries, priority levels, and tags for alerting users about completed tasks or status updates.  (2 tools) (node)
* [✅ mcp-mailtrap](https://github.com/railsware/mailtrap-mcp) ⭐ 65 | 🐛 0 | 🌐 TypeScript | 📅 2026-09-01: Enables sending transactional emails through the Mailtrap Email API.  (1 tools) (node)
* [✅ @horizondatawave/mcp](https://github.com/horizondatawave/hdw-mcp-server) ⭐ 63 | 🐛 1 | 🌐 JavaScript | 📅 2026-08-06: Bridges AI systems with LinkedIn's API for searching users, retrieving profiles, accessing posts, managing connections, and sending messages to support sales prospecting, recruitment, and professional networking workflows.  (23 tools) (node)
* [✅ @waystation/mcp](https://github.com/waystation-ai/mcp) ⭐ 62 | 🐛 1 | 🌐 JavaScript | 📅 2025-09-10: Connects productivity tools like Monday, Asana, Notion, and Slack through a secure integration hub, enabling seamless access directly from chat interfaces without switching applications.  (69 tools) (node)
* [✅ @shinzolabs/gmail-mcp](https://github.com/shinzo-labs/gmail-mcp) ⭐ 58 | 🐛 21 | 🌐 JavaScript | 📅 2025-11-25: Manage your emails effortlessly with a standardized interface for drafting, sending, retrieving, and organizing messages. Streamline your email workflow with complete Gmail API coverage, including label and thread management.  (64 tools) (node)
* [✅ @taazkareem/clickup-mcp-server](https://github.com/taazkareem/clickup-mcp-server) ⭐ 50 | 🐛 2 | 🌐 Dockerfile | 📅 2026-09-01: Integrates ClickUp task management with AI systems to enable automated task creation, updates, and retrieval for enhanced project workflow efficiency.  (36 tools) (node)
* [✅ outlook-calendar-mcp](https://github.com/merajmehrabi/outlook_calendar_mcp) ⭐ 41 | 🐛 8 | 🌐 VBScript | 📅 2025-09-20: Integrates with Microsoft Outlook Calendar to enable event management, scheduling, and attendee status updates for enhanced productivity workflows.  (7 tools) (node)
* [✅ @abhaybabbar/retellai-mcp-server](https://github.com/abhaybabbar/retellai-mcp-server) ⭐ 39 | 🐛 5 | 🌐 TypeScript | 📅 2026-04-09: Integrates with RetellAI's voice services for creating and managing phone conversations, enabling call initiation, agent configuration, and voice selection for tasks like customer service, appointment scheduling, and information gathering.  (24 tools) (node)
* [✅ mcp-server-monday](https://github.com/sakce/mcp-server-monday) ⭐ 34 | 🐛 2 | 🌐 Python | 📅 2025-09-10: Integrates with Monday.com to enable creating items, retrieving board groups, adding comments, listing boards, and managing sub-items for project management and team collaboration workflows.  (21 tools) (python)
* [✅ @pubnub/mcp](https://github.com/pubnub/pubnub-mcp-server) ⭐ 33 | 🐛 0 | 🌐 TypeScript | 📅 2026-07-22: Enables AI assistants to interact with PubNub's realtime communication platform for retrieving documentation, accessing SDK information, and utilizing messaging APIs without leaving their conversation context.  (11 tools) (node)
* [✅ @kevinwatt/mcp-webhook](https://github.com/kevinwatt/mcp-webhook) ⭐ 29 | 🐛 1 | 🌐 JavaScript | 📅 2026-01-08: Enables sending customizable messages to external webhook endpoints, facilitating automated notifications and workflow integrations.  (1 tools) (node)
* [✅ @mbelinky/x-mcp-server](https://github.com/mbelinky/x-mcp-server) ⭐ 21 | 🐛 3 | 🌐 TypeScript | 📅 2025-06-27: Integrates with Twitter/X API using dual OAuth authentication (1.0a and 2.0) to enable tweet posting with media attachments, tweet searching, and tweet deletion with intelligent rate limiting designed for free-tier API usage.  (3 tools) (node)
* [✅ outlook-meetings-scheduler](https://github.com/anoopt/outlook-meetings-scheduler-mcp-server) ⭐ 16 | 🐛 10 | 🌐 TypeScript | 📅 2025-12-24: Integrates with Microsoft Outlook to create, read, update, and delete calendar events, find people, and schedule meetings with specific parameters like time, location, and attendees.  (8 tools) (node)
* [✅ @kazuph/mcp-gmail-gas](https://github.com/kazuph/mcp-gmail-gas) ⭐ 12 | 🐛 2 | 🌐 JavaScript | 📅 2024-12-27: Integrates Gmail functionality, enabling email search, message retrieval, and attachment downloads via Google Apps Script.  (3 tools) (node)
* [✅ voyp-mcp](https://github.com/paulotaylor/voyp-mcp) ⭐ 10 | 🐛 3 | 🌐 JavaScript | 📅 2025-11-10: Integrates with VOYP API to enable automated call handling, routing, and intelligent voice responses for enhanced call center operations.  (7 tools) (node)
* [✅ @prathamesh0901/zoom-mcp-server](https://github.com/prathamesh0901/zoom-mcp-server) ⭐ 8 | 🐛 1 | 🌐 TypeScript | 📅 2025-06-08: Provides a bridge between Zoom API and virtual meeting management, enabling creation, updating, deletion, and fetching of meetings without navigating the Zoom interface or handling authentication flows.  (4 tools) (node)
* [✅ x-com-mcp-server](https://github.com/tiovikram/x.com-mcp-server) ⭐ 8 | 🐛 2 | 🌐 TypeScript | 📅 2025-06-04: Integrates with X.com's API v2 through OAuth 2.0 authentication to provide complete post management capabilities including creation, deletion, search, timelines, retweets, likes, bookmarks, and engagement tracking for social media automation and content analysis workflows.  (21 tools) (node)
* [✅ @grec0/mcp-s2s-asterisk](https://github.com/gcorroto/mcp-s2s-asterisk) ⭐ 7 | 🐛 0 | 🌐 TypeScript | 📅 2025-06-20: Integrates with Asterisk phone systems to enable outbound call operations, conversation monitoring, call history retrieval, and telephony system metrics tracking for business automation workflows.  (9 tools) (node)
* [✅ gmail-mcp](https://github.com/vinayak-mehta/gmail-mcp) ⭐ 7 | 🐛 1 | 🌐 Python | 📅 2025-03-03: Integrates with Gmail to enable email search, retrieval, and interaction for natural language-driven email management and analysis tasks.  (6 tools) (python)
* [✅ resend-mcp](https://github.com/hawstein/resend-mcp) ⭐ 7 | 🐛 1 | 🌐 JavaScript | 📅 2025-04-14: Enables AI to compose and send emails through the Resend API with customizable sender addresses, reply-to fields, and scheduled delivery options  (1 tools) (node)
* [✅ @kydycode/todoist-mcp-server-ext](https://github.com/kydycode/todoist-mcp-server-ext) ⭐ 6 | 🐛 0 | 🌐 JavaScript | 📅 2026-02-15: Integrates with Todoist API to provide enhanced task management capabilities including task creation, updating, completion, project organization, label management, and natural language quick-add functionality with support for subtasks, priorities, due dates, and bulk operations.  (30 tools) (node)
* [✅ mcp-wechat-moments](https://github.com/geminiwen/mcp-wechat-moments) ⭐ 6 | 🐛 0 | 🌐 Python | 📅 2025-03-19: Enables publishing content to WeChat Moments on macOS through AppleScript automation and mouse event emulation, providing a server interface for social media management workflows.  (1 tools) (python)
* [✅ @toolsdk.ai/aws-ses-mcp](https://github.com/omd01/aws-ses-mcp) ⭐ 3 | 🐛 2 | 🌐 TypeScript | 📅 2025-03-22: Enables direct email sending through Amazon SES with support for HTML content, CC/BCC recipients, and reply-to addressing while maintaining AWS security standards.  (1 tools) (node)
* [✅ mcp-clickup](https://github.com/mikah13/mcp-clickup) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2025-02-18: Integrates with ClickUp's API to enable task management, team collaboration, and workflow automation for AI-driven project management and reporting.  (4 tools) (node)
* [✅ @cristip73/mcp-server-asana](https://github.com/cristip73/mcp-server-asana) ⭐ 2 | 🐛 2 | 🌐 TypeScript | 📅 2025-06-14: Integrates with Asana's API to enable task management, project organization, and collaboration workflows through 30+ tools for searching, creating, and visualizing projects and tasks.  (41 tools) (node)
* [✅ discord-mcp](https://github.com/mastra-ai/discord-mcp-server) ⭐ 2 | 🐛 0 | 🌐 TypeScript | 📅 2025-03-26: A Model Context Protocol (MCP) server that provides Discord integration capabilities, including full file attachment support, rate limiting, and comprehensive Discord API features. Built with the official @modelcontextprotocol/sdk for maximum compatibility with Claude Code and other MCP clients.  (19 tools) (node)
* [✅ mcp-fleur](https://github.com/fleuristes/fleur-mcp) ⭐ 2 | 🐛 0 | 🌐 Python | 📅 2025-03-23: Integrates with the Fleur application to enable direct access to external apps like Gmail, Linear, and Slack without leaving the chat interface through platform-specific launch methods for macOS and Windows.  (2 tools) (python)
* [✅ trello-mcp-server](https://github.com/yairhaimo/trello-mcp-server) ⭐ 0 | 🐛 0 | 🌐 JavaScript | 📅 2025-02-22: Integrates with Trello's API to enable AI-powered project management tasks like automated workflow optimization and task creation.  (15 tools) (node)

</details>

<a id="customer-data-platforms"></a>

<details>
<summary><strong>Customer Data Platforms</strong></summary>

Access customer profiles and data from customer data platforms.

* [✅ @tsmztech/mcp-server-salesforce](https://github.com/tsmztech/mcp-server-salesforce) ⭐ 167 | 🐛 10 | 🌐 TypeScript | 📅 2026-08-28: Integrates with Salesforce CRM for natural language-driven data management, querying, and administration tasks.  (15 tools) (node)
* [✅ @clayhq/clay-mcp](https://github.com/clay-inc/clay-mcp) ⭐ 34 | 🐛 3 | 📅 2026-07-17: Provides a bridge to Clay's personal CRM platform for searching, retrieving, and managing contact information, interactions, and professional relationships through natural language queries.  (11 tools) (node)
* [✅ attio-mcp-server](https://github.com/hmk/attio-mcp-server) ⭐ 16 | 🐛 5 | 🌐 JavaScript | 📅 2025-03-07: Integrates with Attio's API for reading and writing company records and notes, enabling CRM operations without direct interface navigation.  (4 tools) (node)
* [✅ @hubspot/mcp-server](https://github.com/hubspot/mcp-server) ⭐ 5 | 🐛 9 | 📅 2025-04-25: Integrates with HubSpot CRM to enable secure access to contact information, company records, deal data, and task management with customizable data access through Private App scopes.  (21 tools) (node)

</details>

<a id="databases"></a>

<details>
<summary><strong>Databases</strong></summary>

Securely access and query databases with options for read-only permissions.

* [✅ postgres-mcp](https://github.com/crystaldba/postgres-mcp) ⭐ 3,245 | 🐛 90 | 🌐 Python | 📅 2026-08-17: Helps you and your AI agents throughout the entire development process—from writing SQL to tuning performance safely.  (9 tools) (python)
* [✅ mongodb-mcp-server](https://github.com/mongodb-js/mongodb-mcp-server) ⭐ 1,116 | 🐛 26 | 🌐 TypeScript | 📅 2026-09-01: MongoDB Model Context Protocol Server  (21 tools) (node)
* [✅ mongodb-mcp-server](https://github.com/mongodb-js/mongodb-mcp-server) ⭐ 1,116 | 🐛 26 | 🌐 TypeScript | 📅 2026-09-01: MongoDB Model Context Protocol Server  (21 tools) (node)
* [✅ chroma-mcp](https://github.com/chroma-core/chroma-mcp) ⭐ 588 | 🐛 32 | 🌐 Python | 📅 2025-09-17: Integrates with Chroma vector database to enable collection management, document operations, and vector search capabilities for knowledge bases and context-aware conversations.  (12 tools) (python)
* [✅ mcp-server-sqlite](https://github.com/modelcontextprotocol/servers-archived/tree/HEAD/src/sqlite) ⚠️ Archived: Query and analyze SQLite databases directly.  (6 tools) (python)
* [✅ @f4ww4z/mcp-mysql-server](https://github.com/f4ww4z/mcp-mysql-server) ⭐ 168 | 🐛 11 | 🌐 JavaScript | 📅 2025-11-14: Interact with MySQL databases to execute queries and manage connections.  (5 tools) (node)
* [✅ mysql-mcp-server](https://github.com/dpflucas/mysql-mcp-server) ⭐ 72 | 🐛 6 | 🌐 JavaScript | 📅 2025-04-22: Provides secure, read-only access to MySQL databases for exploring schemas and executing SELECT queries with built-in safeguards against SQL injection, query timeouts, and row limits.  (4 tools) (node)
* [✅ @pinecone-database/mcp](https://github.com/pinecone-io/pinecone-mcp) ⭐ 71 | 🐛 21 | 🌐 TypeScript | 📅 2026-08-26: Develop with Pinecone, the vector database built for knowledgeable AI.  (9 tools) (node)
* [✅ mcp-firebird](https://github.com/purodelphi/mcpfirebird) ⭐ 64 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-31: Enables secure access to Firebird SQL databases through natural language, supporting table listing, schema descriptions, query execution, and field metadata retrieval with comprehensive security features like data masking and operation restrictions.  (16 tools) (node)
* [✅ @joshuarileydev/supabase-mcp-server](https://github.com/joshuarileydev/supabase-mcp-server) ⭐ 52 | 🐛 1 | 🌐 JavaScript | 📅 2024-12-06: Control Supabase projects and organizations.  (8 tools) (node)
* [✅ adb-mysql-mcp-server](https://github.com/aliyun/alibabacloud-adb-mysql-mcp-server) ⭐ 32 | 🐛 17 | 🌐 Python | 📅 2026-07-30: Connects to Alibaba Cloud's Adb MySQL databases for executing SQL queries, analyzing query plans, and retrieving database metadata with minimal configuration requirements  (3 tools) (python)
* [✅ greptimedb-mcp-server](https://github.com/greptimeteam/greptimedb-mcp-server) ⭐ 29 | 🐛 6 | 🌐 Python | 📅 2026-08-16: Enables AI interaction with GreptimeDB time-series databases through MySQL protocol for data exploration, analysis, and SQL query execution with built-in security protections.  (1 tools) (python)
* [✅ ydb-mcp](https://github.com/ydb-platform/ydb-mcp) ⭐ 29 | 🐛 3 | 🌐 Python | 📅 2026-09-01: Provides a bridge between AI and YDB databases, enabling natural language interactions for executing SQL queries, exploring schema information, and retrieving connection status.  (5 tools) (python)
* [✅ @kevinwatt/mysql-mcp](https://github.com/kevinwatt/mysql-mcp) ⭐ 18 | 🐛 3 | 🌐 JavaScript | 📅 2025-02-24: Provides secure MySQL database access for LLMs, enabling read/write operations with transaction support and security features for AI-assisted data management tasks.  (4 tools) (node)
* [✅ mcp-postgres-server](https://github.com/antonorlov/mcp-postgres-server/tree/main/src) ⭐ 18 | 🐛 3 | 🌐 JavaScript | 📅 2025-04-14: Provides a bridge to PostgreSQL databases for executing SQL queries, managing tables, and inspecting schemas with support for prepared statements and multiple parameter styles  (6 tools) (node)
* [✅ mcp-turso-cloud](https://github.com/spences10/mcp-turso-cloud) ⭐ 18 | 🐛 11 | 🌐 TypeScript | 📅 2026-08-29: Provides a bridge between AI assistants and Turso SQLite databases, enabling organization-level management and database-level queries with persistent context, schema exploration, and vector similarity search capabilities.  (9 tools) (node)
* [✅ @niledatabase/nile-mcp-server](https://github.com/niledatabase/nile-mcp-server) ⭐ 17 | 🐛 4 | 🌐 TypeScript | 📅 2025-03-10: Integrates with Nile Database services to enable database operations through TypeScript-based server implementation supporting both stdio and HTTP communication modes for seamless database functionality in AI workflows.  (11 tools) (node)
* [✅ mysql-query-mcp-server](https://github.com/devakone/mysql-query-mcp-server) ⭐ 13 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-06: Provides a secure, read-only bridge to MySQL databases, enabling natural language querying across multiple environments with strict validation and comprehensive error handling.  (3 tools) (node)
* [✅ mcp-timeplus](https://github.com/jovezhong/mcp-timeplus) ⭐ 12 | 🐛 0 | 🌐 Python | 📅 2025-07-24: Integrates with Timeplus to enable SQL query execution and database information retrieval for real-time analytics and data exploration.  (7 tools) (python)
* [✅ nostrdb-mcp](https://github.com/damus-io/nostrdb-mcp) ⭐ 11 | 🐛 2 | 🌐 JavaScript | 📅 2025-03-06: Integrates with nostrdb to enable local Nostr data querying and analysis.  (2 tools) (node)
* [✅ @malove86/mcp-mysql-server](https://github.com/malove86/mcp-mysql-server) ⭐ 10 | 🐛 1 | 🌐 JavaScript | 📅 2025-04-16: Provides direct interface to MySQL databases for executing SQL queries and retrieving relational data with configurable connection parameters.  (4 tools) (node)
* [✅ mysqldb-mcp-server](https://github.com/burakdirin/mysqldb-mcp-server) ⭐ 9 | 🐛 4 | 🌐 Python | 📅 2025-03-18: Enables direct SQL query execution and database connections to MySQL databases through a simple interface that returns results in JSON format.  (2 tools) (python)
* [✅ @identimoji/mcp-server-emojikey](https://github.com/identimoji/mcp-server-emojikey) ⭐ 4 | 🐛 2 | 🌐 TypeScript | 📅 2025-06-16: Integrates with Supabase to persist and retrieve LLM interaction styles using emojikeys, enabling consistent personalized experiences across conversations.  (4 tools) (node)
* [✅ mcp-neo4j-cypher](https://github.com/guanxinyuan/neo4j) ⭐ 3 | 🐛 1 | 🌐 TypeScript | 📅 2025-03-14: Provides natural language interfaces to Neo4j graph databases for executing Cypher queries, storing knowledge graph data, and building persistent memory structures through conversational interactions.  (3 tools) (python)
* [✅ mochow-mcp-server](https://github.com/baidu/mochow-mcp-server-python) ⭐ 3 | 🐛 0 | 🌐 Python | 📅 2026-06-08: Provides direct access to Mochow vector database capabilities for managing databases, tables, and performing vector similarity and full-text searches with filtering options.  (14 tools) (python)
* [✅ clickhouse-mcp-server](https://github.com/burakdirin/clickhouse-mcp-server) ⭐ 2 | 🐛 2 | 🌐 Python | 📅 2025-03-17: Integrates with ClickHouse databases to execute SQL queries and retrieve results in JSON format, enabling data analysis and exploration directly within conversation interfaces.  (2 tools) (python)
* [✅ dynamo-readonly-mcp](https://github.com/jjikky/dynamo-readonly-mcp) ⭐ 2 | 🐛 1 | 🌐 JavaScript | 📅 2025-08-23: Provides read-only access to AWS DynamoDB databases, enabling natural language interactions for listing tables, scanning data, querying with conditions, and retrieving table schemas without requiring direct database credentials.  (7 tools) (node)
* [✅ oracle-mcp-server](https://github.com/zhengwanbo/oracle-mcp-server) ⭐ 2 | 🐛 0 | 🌐 Python | 📅 2025-06-20: Connects to Oracle databases with intelligent caching and lazy loading to provide schema exploration, query execution with explain plans, and cross-schema operations for efficient database management without loading entire schemas upfront.  (3 tools) (python)
* [✅ mcp-server-starrocks](https://github.com/hagsmand/mcp-server-starrocks) ⭐ 1 | 🐛 2 | 🌐 Python | 📅 2025-03-09: Enables AI models to interact with StarRocks databases by providing read and write access to tables, schemas, and data through a Python-based server with configurable modes.  (5 tools) (python)

</details>

<a id="data-platforms"></a>

<details>
<summary><strong>Data Platforms</strong></summary>

Tools for integrating, transforming, and managing data pipelines.

* [✅ graphlit-mcp-server](https://github.com/graphlit/graphlit-mcp-server) ⭐ 379 | 🐛 6 | 🌐 TypeScript | 📅 2026-01-12: Graphlit MCP Server for AI, RAG, OpenAI, PDF parsing and preprocessing  (64 tools) (node)
* [✅ mcp-google-analytics](https://github.com/surendranb/google-analytics-mcp) ⭐ 240 | 🐛 6 | 🌐 Python | 📅 2026-08-18: A Model Context Protocol (MCP) server for Google Analytics integration. This server provides tools for interacting with Google Analytics, including running reports, querying accounts and properties, and accessing metadata.  (4 tools) (python)
* [✅ powerplatform-mcp](https://github.com/michsob/powerplatform-mcp) ⭐ 42 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-30: Integrates with Microsoft PowerPlatform/Dataverse to enable intelligent access to entity metadata, attributes, relationships, and records with support for OData queries and context-rich prompts for data modeling and exploration.  (8 tools) (node)
* [✅ mcp-server-opendal](https://github.com/xuanwo/mcp-server-opendal) ⭐ 35 | 🐛 2 | 🌐 Python | 📅 2025-04-10: Integrates with OpenDAL to provide unified access to diverse storage backends, enabling LLMs to read from and write to various storage systems for data management tasks.  (3 tools) (python)
* [✅ json-mcp-server](https://github.com/vadimnastoyashchy/json-mcp) ⭐ 14 | 🐛 5 | 🌐 JavaScript | 📅 2025-05-26: Provides tools for splitting large JSON files into manageable chunks and merging multiple JSON files into a consolidated output for efficient data processing workflows.  (2 tools) (node)
* [✅ opengov-mcp-server](https://github.com/srobbin/opengov-mcp-server) ⭐ 14 | 🐛 1 | 🌐 TypeScript | 📅 2025-04-09: Enables access to public government datasets from Socrata-powered portals through a unified tool for searching, querying, and analyzing data like budgets, crime statistics, and transportation information without requiring an API key.  (1 tools) (node)
* [✅ @powerdrillai/powerdrill-mcp](https://github.com/powerdrillai/powerdrill-mcp) ⭐ 13 | 🐛 2 | 🌐 TypeScript | 📅 2025-11-18: Provides tools to interact with Powerdrill datasets to perform data work.  (9 tools) (node)
* [✅ @apitable/aitable-mcp-server](https://github.com/apitable/aitable-mcp-server) ⭐ 12 | 🐛 0 | 🌐 TypeScript | 📅 2025-07-04: AITable.ai Model Context Protocol Server enables AI agents to connect and work with AITable datasheets.  (6 tools) (node)
* [✅ mcp-server-axiom](https://github.com/thetabird/mcp-server-axiom-js) ⭐ 1 | 🐛 1 | 🌐 JavaScript | 📅 2025-01-28: Integrates with Axiom for executing APL queries and listing datasets, enabling log analysis, anomaly detection, and data-driven decision making.  (3 tools) (node)

</details>

<a id="developer-tools"></a>

<details>
<summary><strong>Developer Tools</strong></summary>

Enhance your development workflow with tools for coding and environment management.

* [✅ xcodebuildmcp](https://github.com/cameroncooke/xcodebuildmcp) ⭐ 6,319 | 🐛 20 | 🌐 TypeScript | 📅 2026-09-01: Enables building, running, and debugging iOS and macOS applications through Xcode with tools for project discovery, simulator management, app deployment, and UI automation testing.  (83 tools) (node)
* [✅ @jpisnice/shadcn-ui-mcp-server](https://github.com/Jpisnice/shadcn-ui-mcp-server) ⭐ 2,970 | 🐛 3 | 🌐 TypeScript | 📅 2026-05-16: A mcp server to allow LLMS gain context about shadcn ui component structure,usage and installation  (7 tools) (node)
* [✅ ios-simulator-mcp](https://github.com/joshuayoes/ios-simulator-mcp) ⭐ 2,153 | 🐛 25 | 🌐 JavaScript | 📅 2026-08-13: Enables Claude to control iOS simulators for testing and debugging applications by providing tools for UI interaction, element inspection, and device information retrieval through Facebook's IDB tool.  (10 tools) (node)
* [✅ freecad-mcp](https://github.com/neka-nat/freecad-mcp) ⭐ 1,970 | 🐛 35 | 🌐 Python | 📅 2026-08-26: Enables AI-driven CAD modeling by providing a remote procedure call (RPC) server that allows programmatic control of FreeCAD, supporting operations like creating documents, inserting parts, editing objects, and executing Python code for generative design workflows.  (10 tools) (python)
* [✅ mcp-nixos](https://github.com/utensils/mcp-nixos) ⭐ 815 | 🐛 2 | 🌐 Python | 📅 2026-08-12: Provides a server for accessing NixOS packages, system options, Home Manager, and nix-darwin configurations with multi-level caching and advanced search capabilities  (18 tools) (python)
* [✅ software-planning-tool](https://github.com/nighttrek/software-planning-mcp) ⭐ 396 | 🐛 7 | 🌐 JavaScript | 📅 2025-03-14: Guides developers through a structured, question-based approach to break down software goals into actionable implementation plans with detailed task lists, complexity scores, and code examples.  (6 tools) (node)
* [✅ @sveltejs/mcp](https://github.com/sveltejs/mcp) ⭐ 314 | 🐛 31 | 🌐 TypeScript | 📅 2026-09-01: The official Svelte MCP server providing docs and autofixing tools for Svelte development  (4 tools) (node)
* [✅ mcp-server-tree-sitter](https://github.com/wrale/mcp-server-tree-sitter) ⚠️ Archived: Provides code analysis capabilities through tree-sitter parsing, enabling structured understanding and manipulation of source code across multiple programming languages for tasks like code review, refactoring, and documentation generation.  (26 tools) (python)
* [✅ @hyperdrive-eng/mcp-nodejs-debugger](https://github.com/workbackai/mcp-nodejs-debugger) ⚠️ Archived: Connects Claude Code to Node.js's Inspector Protocol for real-time debugging capabilities, enabling breakpoint setting, variable inspection, and code execution stepping without leaving the conversation interface.  (13 tools) (node)
* [✅ ultra-mcp](https://github.com/realmikechong/ultra-mcp) ⭐ 275 | 🐛 3 | 🌐 TypeScript | 📅 2025-08-25: Unified server providing access to OpenAI O3, Google Gemini 2.5 Pro, and Azure OpenAI models with automatic usage tracking, cost estimation, and nine specialized development tools for code analysis, debugging, and documentation generation.  (23 tools) (node)
* [✅ gistpad-mcp](https://github.com/lostintangent/gistpad-mcp) ⭐ 203 | 🐛 2 | 🌐 TypeScript | 📅 2026-01-16: Transforms GitHub Gists into a personal knowledge management system with specialized handling for daily notes, reusable prompts with frontmatter support, and comprehensive gist operations including creation, updating, archiving, and commenting for version-controlled knowledge storage.  (28 tools) (node)
* [✅ @magicuidesign/mcp](https://github.com/magicuidesign/mcp) ⭐ 202 | 🐛 0 | 🌐 TypeScript | 📅 2026-04-07: Provides structured access to Magic UI's component library for generating accurate code suggestions with proper installation instructions for implementing visually appealing UI elements in web applications.  (8 tools) (node)
* [✅ mcp-svelte-docs](https://github.com/spences10/mcp-svelte-docs) ⭐ 123 | 🐛 13 | 🌐 TypeScript | 📅 2026-08-26: Integrates with Svelte documentation to enable efficient querying and retrieval of framework-specific content for development assistance.  (12 tools) (node)
* [✅ terraform-mcp-server](https://github.com/thrashr888/terraform-mcp-server) ⚠️ Archived: Integrates with the Terraform Registry API to enable provider lookup, resource usage examples, module recommendations, and schema details retrieval for infrastructure-as-code development.  (10 tools) (node)
* [✅ @opentofu/opentofu-mcp-server](https://github.com/opentofu/opentofu-mcp-server) ⭐ 108 | 🐛 0 | 🌐 TypeScript | 📅 2026-07-15: Enables AI systems to search for and retrieve detailed information about OpenTofu Registry components including providers, modules, resources, and documentation for infrastructure-as-code tasks.  (5 tools) (node)
* [✅ @circleci/mcp-server-circleci](https://github.com/circleci-public/mcp-server-circleci) ⭐ 92 | 🐛 27 | 🌐 TypeScript | 📅 2026-08-06: Enables agents to talk to CircleCI. Fetch build failure logs to fix issues.  (14 tools) (node)
* [✅ mcp-postman](https://github.com/shannonlal/mcp-postman) ⭐ 84 | 🐛 7 | 🌐 TypeScript | 📅 2025-03-25: Executes Postman collections to run API tests, validate responses, and generate reports for automated testing and documentation workflows.  (1 tools) (node)
* [✅ mcp-azure-devops](https://github.com/vortiago/mcp-azure-devops) ⭐ 79 | 🐛 15 | 🌐 Python | 📅 2025-10-29: Integrates with Azure DevOps services to enable natural language interactions for querying work items, retrieving project information, and managing team resources without navigating the complex interface directly.  (21 tools) (python)
* [✅ mcp-package-docs](https://github.com/sammcj/mcp-package-docs) ⚠️ Archived: Provides efficient access to NPM/Go/Python package documentation through smart parsing and caching, enabling quick retrieval of up-to-date library information.  (10 tools) (node)
* [✅ vscode-mcp-server](https://github.com/block/vscode-mcp) ⚠️ Archived: Enables direct interaction with VS Code through bidirectional communication, providing tools for file diffing, project navigation, shell command execution, and editor information retrieval for seamless coding assistance.  (9 tools) (node)
* [✅ @mcp-get-community/server-llm-txt](https://github.com/mcp-get/community-servers/tree/HEAD/src/server-llm-txt) ⚠️ Archived: Access up-to-date API documentation efficiently.  (3 tools) (node)
* [✅ @mcp-get-community/server-macos](https://github.com/mcp-get/community-servers/blob/main/src/server-macos) ⚠️ Archived: MCP server for macOS system operations  (2 tools) (node)
* [✅ @jsonresume/mcp](https://github.com/jsonresume/mcp) ⭐ 63 | 🐛 0 | 🌐 TypeScript | 📅 2026-06-12: Enhances JSON Resumes with GitHub project information by analyzing codebases, fetching existing resumes, and intelligently updating profiles with relevant project details using OpenAI's API.  (3 tools) (node)
* [✅ @rtuin/mcp-mermaid-validator](https://github.com/rtuin/mcp-mermaid-validator) ⭐ 56 | 🐛 2 | 🌐 JavaScript | 📅 2026-02-27: Validates and renders Mermaid diagrams as SVG images, providing detailed error messages for invalid syntax to enhance visualization capabilities within conversations.  (1 tools) (node)
* [✅ a11y-mcp](https://github.com/priyankark/a11y-mcp) ⭐ 51 | 🐛 0 | 🌐 JavaScript | 📅 2026-03-22: Perform accessibility audits on webpages using axe-core. Use the results in an agentic loop with your favorite AI assistants (Cline/Cursor/GH Copilot) and let them fix a11y issues for you.  (2 tools) (node)
* [✅ mcp-server-taskwarrior](https://github.com/awwaiid/mcp-server-taskwarrior) ⭐ 50 | 🐛 5 | 🌐 JavaScript | 📅 2026-03-27: Integrates with TaskWarrior to enable viewing, adding, and completing tasks, facilitating automated task management for productivity and project workflows.  (3 tools) (node)
* [✅ @serverless-dna/powertools-mcp](https://github.com/aws-powertools/powertools-mcp) ⚠️ Archived: Enables AI to search and retrieve AWS Lambda Powertools documentation across multiple runtimes through a TypeScript server with efficient local search capabilities and content caching.  (2 tools) (node)
* [✅ sf-mcp](https://github.com/codefriar/sf-mcp) ⭐ 36 | 🐛 7 | 🌐 JavaScript | 📅 2026-02-14: Exposes Salesforce CLI functionality for interacting with Salesforce orgs, enabling developers to query data, deploy code, and manage orgs through dynamically discovered commands.  (5 tools) (node)
* [✅ cursor-chat-history-mcp](https://github.com/vltansky/cursor-chat-history-mcp) ⭐ 33 | 🐛 8 | 🌐 TypeScript | 📅 2026-01-12: Analyzes local Cursor chat history to extract development patterns, usage insights, and coding best practices with tools for searching conversations, generating analytics, and exporting data in multiple formats for personalized development assistance.  (8 tools) (node)
* [✅ @heilgar/shadcn-ui-mcp-server](https://github.com/heilgar/shadcn-ui-mcp-server) ⭐ 25 | 🐛 0 | 🌐 TypeScript | 📅 2025-05-21: Provides tools for managing and installing shadcn/ui components directly through assistants, enabling efficient component discovery, documentation retrieval, and installation command generation with multiple package manager support.  (6 tools) (node)
* [✅ @tgomareli/macos-tools-mcp](https://github.com/tornikegomareli/macos-tools-mcp-server) ⭐ 24 | 🐛 0 | 🌐 TypeScript | 📅 2025-08-02: Provides macOS system monitoring with SQLite-based historical data storage and enhanced file search with tagging support, collecting real-time CPU, memory, disk, and network metrics while offering content-based file searching with regex support and macOS file tagging operations through native utilities like Spotlight and extended attributes.  (2 tools) (node)
* [✅ @growthbook/mcp](https://github.com/growthbook/growthbook-mcp) ⭐ 23 | 🐛 19 | 🌐 TypeScript | 📅 2026-08-31: Enables AI to manage feature flags, experiments, environments, and SDK connections in GrowthBook, providing tools for searching documentation, creating targeting rules, and generating implementation code for various programming languages.  (18 tools) (node)
* [✅ qasphere-mcp](https://github.com/hypersequent/qasphere-mcp) ⚠️ Archived: Integration with QA Sphere test management system, enabling LLMs to discover, summarize, and interact with test cases directly from AI-powered IDEs.  (6 tools) (node)
* [✅ it-tools-mcp](https://github.com/wrenchpilot/it-tools-mcp) ⚠️ Archived: Provides 50+ developer utilities including cryptographic operations, text processing, data format conversion, network calculations, and encoding functions through a containerized TypeScript server with security features and rate limiting.  (119 tools) (node)
* [✅ mcp-chain-of-thought](https://github.com/liorfranko/mcp-chain-of-thought) ⭐ 21 | 🐛 0 | 🌐 TypeScript | 📅 2025-05-07: Task management system that converts natural language into organized development tasks with dependency tracking, implementation guides, and verification criteria through structured reasoning phases.  (15 tools) (node)
* [✅ @currents/mcp](https://github.com/currents-dev/currents-mcp) ⭐ 20 | 🐛 2 | 🌐 TypeScript | 📅 2026-09-01: Provides a bridge to Currents test results platform, enabling AI to analyze failing tests, optimize test suites, and troubleshoot CI/CD pipeline issues through direct access to test execution data.  (3 tools) (node)
* [✅ @ahdev/dokploy-mcp](https://github.com/andradehenrique/dokploy-mcp) ⭐ 18 | 🐛 1 | 🌐 TypeScript | 📅 2025-06-05: Integrates with Dokploy platform API for creating, updating, duplicating, and removing deployment projects, enabling teams to automate deployment workflows through AI interactions.  (56 tools) (node)
* [✅ @stakpak/mcp](https://github.com/stakpak/mcp) ⭐ 18 | 🐛 1 | 🌐 JavaScript | 📅 2026-07-04: Integrates with Stakpak API to generate infrastructure code for projects, enabling developers to quickly create configurations through a dedicated tool that works with various IDEs.  (1 tools) (node)
* [✅ @cdugo/docs-fetcher-mcp](https://github.com/cdugo/package-documentation-mcp) ⭐ 15 | 🐛 1 | 🌐 JavaScript | 📅 2025-03-13: Integrates with multiple package registries and documentation sources to provide up-to-date library information for code assistance, dependency analysis, and learning about new libraries.  (4 tools) (node)
* [✅ @yodakeisuke/mcp-micromanage](https://github.com/yodakeisuke/mcp-micromanage-your-agent) ⭐ 13 | 🐛 1 | 🌐 TypeScript | 📅 2025-04-01: Task management system that visualizes development work as interactive flowcharts, enabling structured breakdown of tickets into minimal PRs and commits with progress tracking capabilities.  (3 tools) (node)
* [✅ uiflowchartcreator](https://github.com/umshere/uiflowchartcreator) ⭐ 11 | 🐛 1 | 🌐 TypeScript | 📅 2025-01-05: Generates UI flowcharts based on input specifications, enabling visual representation of user interfaces and interactions for design communication and workflow analysis.  (1 tools) (node)
* [✅ @chriswhiterocks/sushimcp](https://github.com/maverickg59/sushimcp) ⭐ 9 | 🐛 3 | 🌐 TypeScript | 📅 2026-02-19: Delivers documentation context from various technology sources to improve code generation by fetching and serving relevant llms.txt documentation on demand.  (4 tools) (node)
* [✅ @container-inc/mcp](https://github.com/f-inc/containerinc-mcp) ⭐ 9 | 🐛 2 | 🌐 TypeScript | 📅 2025-04-03: Enables seamless deployment of containerized applications directly from code editors through a three-step workflow of GitHub authentication, repository setup, and automated Docker image publishing.  (3 tools) (node)
* [✅ project-mcp](https://github.com/pouyanafisi/project-mcp) ⭐ 9 | 🐛 0 | 🌐 JavaScript | 📅 2026-08-27: AI-native project management with intent-based documentation search, Jira-like task IDs (PROJECT-001), backlog workflow (import → promote → archive), thought processing (brain dumps → structured tasks with intent analysis), and project file management. (40 tools, 12 prompts)  (42 tools) (node)
* [✅ @kailashg101/mcp-figma-to-code](https://github.com/kailashappdev/figma-mcp-toolkit) ⭐ 8 | 🐛 3 | 🌐 TypeScript | 📅 2025-03-24: Extracts and analyzes components from Figma design files, enabling seamless integration between Figma designs and React Native development through component hierarchy processing and metadata generation.  (3 tools) (node)
* [✅ @pinkpixel/npm-helper-mcp](https://github.com/pinkpixel-dev/npm-helper-mcp) ⭐ 8 | 🐛 2 | 🌐 JavaScript | 📅 2026-03-31: Provides specialized tools for searching npm packages, fetching documentation, checking outdated dependencies, and safely upgrading Node.js packages with version constraint management  (10 tools) (node)
* [✅ @buouui/supaui-mcp](https://github.com/buoooou/mcp-ui-gen) ⭐ 7 | 🐛 0 | 🌐 TypeScript | 📅 2025-05-11: Enables React UI component generation, fetching, and management through natural language interactions on the buouui.com platform, leveraging TypeScript and developer-focused design workflows.  (3 tools) (node)
* [✅ deepsource-mcp-server](https://github.com/sapientpants/deepsource-mcp-server) ⚠️ Archived: Integrates with DeepSource's code quality platform to provide access to project metrics, issues, and analysis results for monitoring and troubleshooting code quality directly in conversations.  (10 tools) (node)
* [✅ mcp-server-restart](https://github.com/non-dirty/mcp-server-restart) ⭐ 6 | 🐛 0 | 🌐 Python | 📅 2024-12-02: Enables automated restarts of Claude Desktop on macOS by leveraging psutil to safely terminate and relaunch the application process.  (1 tools) (python)
* [✅ @nextdrive/github-action-trigger-mcp](https://github.com/nextdriveioe/github-action-trigger-mcp) ⭐ 3 | 🐛 2 | 🌐 JavaScript | 📅 2026-04-17: Enables GitHub Actions integration for triggering workflows, fetching action details, and retrieving repository releases through authenticated API interactions  (4 tools) (node)
* [✅ @wenbopan/things-mcp](https://github.com/wbopan/things-mcp) ⭐ 3 | 🐛 1 | 🌐 TypeScript | 📅 2025-12-20: Integrates with Things.app task management for macOS, enabling task and project creation with full metadata support, update operations including completion status, database export functionality, and summary generation through URL scheme and direct database access.  (6 tools) (node)
* [✅ shadow-cljs-mcp](https://github.com/bigsy/shadow-cljs-mcp) ⭐ 3 | 🐛 2 | 🌐 JavaScript | 📅 2025-04-15: Monitors ClojureScript builds in real-time, providing detailed status information including compilation status, warnings, errors, and file-specific details for verifying build success after code changes.  (1 tools) (node)
* [✅ @auto-browse/unbundle-openapi-mcp](https://github.com/auto-browse/unbundle_openapi_mcp) ⭐ 2 | 🐛 2 | 🌐 JavaScript | 📅 2025-05-21: Splits and extracts portions of OpenAPI specification files into smaller, more focused files while preserving referenced components for improved documentation and maintainability.  (2 tools) (node)
* [✅ @coderide/mcp](https://github.com/pixdataorg/coderide-mcp) ⭐ 2 | 🐛 1 | 🌐 TypeScript | 📅 2026-02-01: Integrates with CodeRide's task management platform to provide project retrieval, task operations, prompt extraction, and project initialization with knowledge graphs and Mermaid diagrams for development workflows.  (9 tools) (node)
* [✅ github-mcp](https://github.com/Seey215/github-mcp) ⭐ 2 | 🐛 0 | 🌐 TypeScript | 📅 2025-11-28: A powerful GitHub automation tool that seamlessly connects AI assistants to your GitHub repositories  (2 tools) (node)
* [✅ source-map-parser-mcp](https://github.com/masonchow/source-map-parser-mcp) ⭐ 2 | 🐛 0 | 🌐 TypeScript | 📅 2025-09-28: Maps minified JavaScript stack traces back to original source code locations for efficient production error debugging.  (2 tools) (node)
* [✅ tree-hugger-js-mcp](https://github.com/qckfx/tree-hugger-js-mcp) ⭐ 2 | 🐛 0 | 🌐 JavaScript | 📅 2025-06-30: Provides JavaScript and TypeScript code analysis through AST parsing for function extraction, scope analysis, identifier renaming, unused import removal, and code transformation with safety previews and history tracking.  (12 tools) (node)
* [✅ metatag-genie](https://github.com/terryso/meta_tag_genie) ⭐ 1 | 🐛 1 | 🌐 TypeScript | 📅 2025-05-11: Enables AI to write standardized metadata to various image file formats including HEIC and PNG for automated tagging, photo organization, and copyright embedding without switching contexts.  (1 tools) (node)
* [✅ @toolsdk-remote/discovery-oracle-402bot](https://github.com/sam00101011/402.bot-public) ⭐ 0 | 🐛 0 | 🌐 JavaScript | 📅 2026-03-17: Discover live agent APIs, ranked endpoints, trust, payment telemetry, and x402 surfaces.  (4 tools) (node)
* [✅ bika-mcp-server](https://github.com/bika-ai/bika-mcp-server) ⭐ 0 | 🐛 0 | 📅 2025-05-03: A Model Context Protocol server that provides read and write access to Bika.ai. This server enables LLMs to list spaces, list nodes, list records, create records and upload attachments in Bika.ai.  (6 tools) (node)
* [✅ jnews-mcp-server](https://github.com/juhemcp/jnews-mcp-server) ⭐ 0 | 🐛 1 | 🌐 Python | 📅 2025-03-18: Lightweight Python FastAPI server implementation for streamlined server-side interactions, using modern tooling like uv for dependency management and GitHub Actions for automated testing and deployment.  (2 tools) (python)
* [✅ mcp-developer-name](https://github.com/seriawei/mcp-developer-name) ⭐ 0 | 🐛 1 | 🌐 JavaScript | 📅 2025-03-18: Provides customizable developer information through a lightweight Node.js server that can be run via npx command or deployed as a Docker container.  (1 tools) (node)
* [✅ @shopify/dev-mcp](https://github.com/shopify/dev-mcp): Integrates with Shopify Dev. Supports various tools to interact with different Shopify APIs.  (4 tools) (node)

</details>

<a id="data-science-tools"></a>

<details>
<summary><strong>Data Science Tools</strong></summary>

Simplify data analysis and exploration with tools for data science workflows.

* [✅ @arizeai/phoenix-mcp](https://github.com/arize-ai/phoenix/tree/HEAD/js/packages/phoenix-mcp) ⭐ 11,280 | 🐛 944 | 🌐 Python | 📅 2026-09-01: Provides a unified interface to Arize Phoenix's capabilities for managing prompts, exploring datasets, and running experiments across different LLM providers  (19 tools) (node)
* [✅ @antv/mcp-server-chart](https://github.com/antvis/mcp-server-chart) ⭐ 4,347 | 🐛 6 | 🌐 TypeScript | 📅 2026-08-27: A visualization mcp contains 25+ visual charts using @antvis. Using for chart generation and data analysis.  (25 tools) (node)
* [✅ mcp-excel-server](https://github.com/yzfly/mcp-excel-server) ⭐ 94 | 🐛 0 | 🌐 Python | 📅 2026-07-02: Enables Excel file operations and data analysis with tools for statistical analysis, data filtering, pivot table creation, and visualization through charts and plots.  (8 tools) (python)
* [✅ @gongrzhe/server-json-mcp](https://github.com/gongrzhe/json-mcp-server) ⚠️ Archived: Provides a JSON manipulation interface using JSONPath syntax for querying, transforming, and analyzing structured data across diverse datasets.  (2 tools) (node)
* [✅ optuna-mcp](https://github.com/optuna/optuna-mcp) ⭐ 85 | 🐛 0 | 🌐 Python | 📅 2026-08-05: Provides automated hyperparameter optimization and analysis using Optuna framework with support for multiple samplers, multi-objective optimization, parameter importance analysis, and interactive visualizations including optimization history and Pareto fronts.  (26 tools) (python)
* [✅ kaggle-mcp](https://github.com/54yyyu/kaggle-mcp) ⭐ 35 | 🐛 3 | 🌐 Python | 📅 2025-06-11: Integrates with Kaggle's API to enable competition participation, dataset management, kernel operations, and model submissions for data scientists and machine learning practitioners.  (1 tools) (python)
* [✅ code-context-provider-mcp](https://github.com/ab498/code-context-provider-mcp) ⭐ 20 | 🐛 0 | 🌐 JavaScript | 📅 2025-09-22: Analyzes project directories to extract code structure and symbols using Tree-sitter parsers, providing tools for generating directory trees and performing deep code analysis of JavaScript, TypeScript, and Python files.  (1 tools) (node)
* [✅ scmcp](https://github.com/huang-sh/scmcp) ⭐ 12 | 🐛 0 | 🌐 Python | 📅 2025-05-15: Provides natural language access to single-cell RNA sequencing analysis through Scanpy, enabling bioinformatics workflows like clustering, dimensionality reduction, and cell type annotation without writing code.  (51 tools) (python)

</details>

<a id="embedded-system"></a>

<details>
<summary><strong>Embedded System</strong></summary>

Access resources and shortcuts for working with embedded devices.

* [✅ @mobilenext/mobile-mcp](https://github.com/mobile-next/mobile-mcp) ⭐ 6,285 | 🐛 60 | 🌐 TypeScript | 📅 2026-09-01: Enables remote control of Android and iOS devices through commands for screenshots, app management, screen interactions, and UI navigation, ideal for automated testing and demonstrations.  (17 tools) (node)
* [✅ mcp-server-ida](https://github.com/mxiris-reverse-engineering/ida-mcp-server) ⚠️ Archived: Enables programmatic reading and searching of IDA Pro databases via large language models, providing tools for reverse engineering and binary analysis automation.  (19 tools) (python)
* [✅ frida-mcp](https://github.com/dnakov/frida-mcp) ⭐ 428 | 🐛 5 | 🌐 Python | 📅 2025-05-12: Enables dynamic instrumentation of mobile and desktop applications through Frida toolkit, providing capabilities for process management, device enumeration, and script injection for application analysis and debugging workflows.  (13 tools) (python)
* [✅ mcp-3d-printer-server](https://github.com/dmontgomery40/mcp-3d-printer-server) ⭐ 230 | 🐛 1 | 🌐 JavaScript | 📅 2026-07-30: Integrates with multiple 3D printer management systems to enable remote control, file handling, and advanced STL manipulation for automated print job management and custom model modifications.  (15 tools) (node)
* [✅ mcp-gdb](https://github.com/signal-slot/mcp-gdb) ⭐ 158 | 🐛 4 | 🌐 JavaScript | 📅 2026-07-28: Integrates with GDB to provide debugging capabilities for C/C++ programs, enabling breakpoint setting, code stepping, memory examination, and call stack viewing without leaving the conversation interface.  (16 tools) (node)
* [✅ adb-mcp](https://github.com/srmorete/adb-mcp) ⚠️ Archived: Bridges AI with Android devices through ADB, enabling device management, shell commands, app installation, file transfers, and UI inspection without requiring direct ADB knowledge.  (8 tools) (node)
* [✅ mcp2serial](https://github.com/mcp2everything/mcp2serial) ⭐ 49 | 🐛 3 | 🌐 Python | 📅 2024-12-19: Bridges with physical hardware devices (e.g. Raspberry Pi) via serial communication, enabling real-world control and interaction for IoT and robotics applications.  (python)
* [✅ @noahlozevski/mcp-idb](https://github.com/noahlozevski/mcp-idb) ⭐ 7 | 🐛 1 | 🌐 JavaScript | 📅 2025-03-07: Integrates with Facebook's iOS Development Bridge (idb) to enable automated iOS device management, test execution, UI interactions, and app installation through a simple npm module.  (1 tools) (node)
* [✅ @taskjp/server-systemd-coredump](https://github.com/signal-slot/mcp-systemd-coredump) ⭐ 3 | 🐛 2 | 🌐 JavaScript | 📅 2025-03-07: Provides a bridge to systemd-coredump functionality for accessing, managing, and analyzing system core dumps in Linux environments, including listing available coredumps, retrieving information, extracting dumps, and generating stack traces using GDB.  (6 tools) (node)

</details>

<a id="file-systems"></a>

<details>
<summary><strong>File Systems</strong></summary>

Manage files and directories with tools for reading, writing, and organizing files.

* [✅ @shtse8/pdf-reader-mcp](https://github.com/sylphxltd/pdf-reader-mcp) ⭐ 909 | 🐛 2 | 🌐 TypeScript | 📅 2026-08-30: Securely extracts text, metadata, and page information from PDF files within a project directory using pdfjs-dist for both local files and remote URLs.  (1 tools) (node)
* [✅ @isaacphi/mcp-gdrive](https://github.com/isaacphi/mcp-gdrive) ⭐ 283 | 🐛 17 | 🌐 TypeScript | 📅 2025-05-07: Integrates Google Drive and Sheets functionality for file operations and spreadsheet data manipulation.  (4 tools) (node)
* [✅ mcp-text-editor](https://github.com/tumf/mcp-text-editor) ⭐ 199 | 🐛 1 | 🌐 Python | 📅 2026-03-17: Perform efficient line-oriented operations on text files.  (2 tools) (python)
* [✅ mcp-docs-service](https://github.com/alekspetrov/mcp-docs-service) ⭐ 58 | 🐛 2 | 🌐 TypeScript | 📅 2026-02-11: Enables AI interaction with markdown documentation files through a SQL-like query format for creating, reading, updating, and searching documentation with YAML frontmatter metadata support in Node.js and Deno environments.  (14 tools) (node)
* [✅ @inkdropapp/mcp-server](https://github.com/inkdropapp/mcp-server) ⭐ 57 | 🐛 0 | 🌐 JavaScript | 📅 2026-07-17: Integrates with Inkdrop's note-taking application to enable searching, reading, creating, and updating Markdown notes directly within conversations through seven specialized tools for managing notes, notebooks, and tags.  (7 tools) (node)
* [✅ @cyanheads/filesystem-mcp-server](https://github.com/cyanheads/filesystem-mcp-server) ⭐ 50 | 🐛 16 | 🌐 TypeScript | 📅 2025-07-22: Provides a secure interface for interacting with local filesystems, enabling file reading, writing, updating, and directory management with robust path resolution and security measures.  (10 tools) (node)
* [✅ @zhiweixu/excel-mcp-server](https://github.com/zhiwei5576/excel-mcp-server) ⭐ 49 | 🐛 3 | 🌐 TypeScript | 📅 2025-04-13: Enables direct interaction with Excel files for reading sheet names, extracting data, and managing workbook caching to improve performance with large spreadsheets.  (8 tools) (node)
* [✅ mcp-server-text-editor](https://github.com/bhouston/mcp-server-text-editor) ⭐ 36 | 🐛 7 | 🌐 TypeScript | 📅 2025-03-17: Implements Claude's text editor tool as a server, enabling viewing, editing, and creating text files with persistent state across command calls.  (1 tools) (node)
* [✅ @exoticknight/mcp-file-merger](https://github.com/exoticknight/mcp-file-merger) ⭐ 27 | 🐛 0 | 🌐 JavaScript | 📅 2026-08-08: Merge multiple files into one.  (2 tools) (node)
* [✅ @sworddut/mcp-ffmpeg-helper](https://github.com/sworddut/mcp-ffmpeg-helper) ⭐ 25 | 🐛 3 | 🌐 TypeScript | 📅 2026-01-04: Provides access to FFmpeg's video processing capabilities including format conversion, audio extraction, trimming, watermarking, frame extraction, and media information retrieval via simple tool calls  (8 tools) (node)
* [✅ mcp-ipfs](https://github.com/alexbakers/mcp-ipfs) ⭐ 21 | 🐛 4 | 🌐 TypeScript | 📅 2025-04-10: Enables AI access to the IPFS Storacha Network for decentralized file storage, content addressing, and persistent data management through the w3cli interface.  (35 tools) (node)
* [✅ @bunas/fs-mcp](https://github.com/bunasq/fs) ⭐ 12 | 🐛 3 | 🌐 TypeScript | 📅 2025-03-11: Enables file system access to read local files with optional API key authentication, providing a simple interface for analyzing and working with file content without manual uploads.  (1 tools) (node)
* [✅ @puchunjie/doc-tools-mcp](https://github.com/puchunjie/doc-tools-mcp) ⭐ 11 | 🐛 4 | 🌐 TypeScript | 📅 2025-03-26: Enables natural language-driven Word document creation, editing, and formatting through a Node.js server that integrates seamlessly with development environments for document automation workflows.  (7 tools) (node)
* [✅ mcp-wordcounter](https://github.com/qpd-v/mcp-wordcounter) ⭐ 10 | 🐛 1 | 🌐 JavaScript | 📅 2024-12-19: Analyze text documents, including counting words and characters, through Node.js.  (1 tools) (node)
* [✅ @shtse8/filesystem-mcp](https://github.com/sylphxltd/filesystem-mcp) ⚠️ Archived: Provides secure, controlled filesystem operations within a project's root directory, enabling safe file listing, reading, writing, and searching with robust path validation.  (13 tools) (node)
* [✅ @myuon/refactor-mcp](https://github.com/myuon/refactor-mcp) ⭐ 7 | 🐛 1 | 🌐 TypeScript | 📅 2025-07-05: Provides regex-based code refactoring capabilities for bulk search-and-replace operations across file systems with pattern matching, context filtering, and glob-based file discovery to enable large-scale code transformations and migrations.  (2 tools) (node)
* [✅ excel-to-pdf-mcp](https://github.com/kmexnx/excel-to-pdf-mcp) ⭐ 6 | 🐛 1 | 🌐 TypeScript | 📅 2025-08-08: Converts Excel and Apple Numbers spreadsheets to PDF format, enabling file sharing with stakeholders who lack access to the original applications.  (2 tools) (node)
* [✅ @sunwood-ai-labs/source-sage-mcp-server](https://github.com/sunwood-ai-labs/source-sage-mcp-server) ⭐ 5 | 🐛 2 | 🌐 TypeScript | 📅 2024-12-22: Integrates directory structure visualization and analysis tools to generate and reason about project file hierarchies for codebase understanding.  (1 tools) (node)
* [✅ magick-mcp](https://github.com/aroglahcim/magick-mcp) ⭐ 3 | 🐛 0 | 🌐 JavaScript | 📅 2025-02-07: Integrates with ImageMagick's CLI to enable image processing and manipulation tasks like resizing, format conversion, and applying filters or effects.  (1 tools) (node)
* [✅ @qpd-v/mcp-delete](https://github.com/qpd-v/mcp-delete) ⭐ 1 | 🐛 1 | 🌐 JavaScript | 📅 2025-03-05: File deletion capabilities to safely delete files when needed, with support for both relative and absolute paths.  (1 tools) (node)
* [✅ @team-jd/mcp-project-explorer](https://github.com/mausrundung362/mcp-explorer) ⭐ 1 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-12: Provides secure file system exploration and project analysis for TypeScript/JavaScript codebases with tools for project exploration, file search with regex support, file management operations, package dependency analysis, and directory access control through configurable allowed paths.  (6 tools) (node)
* [✅ find-files-mcp](https://github.com/thuhoai27/find-files-mcp) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-23: Powerful file search tool that enables wildcard patterns, attribute filtering, and content searching across multiple directories, returning detailed file information including path, size, dates, and MIME type.  (1 tools) (node)
* [✅ mcp-tree-explorer](https://github.com/carterlasalle/directory_structure_mcp) ⭐ 0 | 🐛 0 | 🌐 Python | 📅 2025-03-17: Lightweight directory tree visualization tool for Cursor that automatically installs the 'tree' command, provides smart filtering of large directories, and supports customizable ignore/keep patterns across Windows, macOS, and Linux.  (1 tools) (python)
* [✅ @aindreyway/mcp-neurolora](https://github.com/aindreyway/mcp-neurolora): Extract and document code from your local filesystem, enabling automated documentation and codebase analysis.  (4 tools) (node)

</details>

<a id="finance-fintech"></a>

<details>
<summary><strong>Finance & Fintech</strong></summary>

Work with financial data, market info, and trading platforms using AI tools.

* [✅ @xeroapi/xero-mcp-server](https://github.com/xeroapi/xero-mcp-server) ⭐ 358 | 🐛 120 | 🌐 TypeScript | 📅 2026-06-05: Provides a bridge to the Xero accounting API, enabling financial data interactions and accounting operations through OAuth2 custom connections for automated workflow management.  (51 tools) (node)
* [✅ investor-agent](https://github.com/ferdousbhai/investor-agent) ⭐ 345 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-28: Provides real-time financial analysis tools leveraging market data from yfinance and CNN's Fear & Greed Index for investment research, portfolio analysis, and market sentiment evaluation.  (7 tools) (python)
* [✅ akshare-one-mcp](https://github.com/zwldarren/akshare-one-mcp) ⭐ 226 | 🐛 8 | 🌐 Python | 📅 2026-03-14: Provides access to Chinese stock market data including historical prices, real-time information, financial statements, and news from sources like Eastmoney, Sina, and Xueqiu.  (7 tools) (python)
* [✅ yfmcp](https://github.com/narumiruna/yfinance-mcp) ⭐ 188 | 🐛 0 | 🌐 Python | 📅 2026-08-30: Provides real-time financial data from Yahoo Finance through specialized tools for retrieving stock information, market trends, and news for investment research and analysis.  (5 tools) (python)
* [✅ octagon-mcp](https://github.com/octagonai/octagon-mcp-server) ⭐ 147 | 🐛 4 | 🌐 TypeScript | 📅 2026-07-09: Provides specialized investment research tools for analyzing SEC filings, earnings calls, financial data, stock market information, private company details, funding rounds, M\&A transactions, and web scraping capabilities.  (3 tools) (node)
* [✅ @mcpfun/mcp-server-ccxt](https://github.com/doggybee/mcp-server-ccxt) ⭐ 145 | 🐛 5 | 🌐 TypeScript | 📅 2025-06-03: High-performance CCXT MCP server for cryptocurrency exchange integration.  (24 tools) (node)
* [✅ fred-mcp-server](https://github.com/stefanoamorelli/fred-mcp-server) ⭐ 117 | 🐛 5 | 🌐 TypeScript | 📅 2026-08-22: Provides a bridge to the Federal Reserve Economic Data API for retrieving economic time series data like Overnight Reverse Repurchase Agreements and Consumer Price Index with customizable parameters for date ranges and sorting options.  (1 tools) (node)
* [✅ square-mcp-server](https://github.com/square/square-mcp-server) ⭐ 107 | 🐛 15 | 🌐 TypeScript | 📅 2026-04-09: Provides a bridge between Square's complete API ecosystem and conversational interfaces, enabling comprehensive e-commerce and payment processing capabilities including payments, orders, inventory, and customer management.  (3 tools) (node)
* [✅ @lazydino/ccxt-mcp](https://github.com/lazy-dinosaur/ccxt-mcp) ⭐ 93 | 🐛 0 | 🌐 TypeScript | 📅 2026-04-23: Bridges the CCXT cryptocurrency trading library with natural language interfaces, enabling monitoring, analysis, and trading operations across 100+ exchanges  (20 tools) (node)
* [✅ coincap-mcp](https://github.com/quantgeekdev/coincap-mcp) ⭐ 93 | 🐛 6 | 🌐 TypeScript | 📅 2025-01-30: Access cryptocurrency price data without authentication.  (3 tools) (node)
* [✅ bitcoin-mcp](https://github.com/abdelstark/bitcoin-mcp) ⭐ 76 | 🐛 5 | 🌐 TypeScript | 📅 2025-08-01: Integrates with Bitcoin network and blockchain data via Blockstream API to enable transaction monitoring, wallet management, and blockchain analysis  (7 tools) (node)
* [✅ dodopayments-mcp](https://github.com/dodopayments/dodopayments-node/tree/HEAD/packages/mcp-server) ⭐ 54 | 🐛 6 | 🌐 TypeScript | 📅 2026-09-01: Provides a lightweight, serverless-compatible interface for AI-driven payment operations like billing, subscriptions, and customer management using the Dodo Payments API.  (57 tools) (node)
* [✅ @snjyor/binance-mcp](https://github.com/snjyor/binance-mcp) ⭐ 45 | 🐛 3 | 🌐 JavaScript | 📅 2025-07-05: Integrates with Binance cryptocurrency API to provide real-time market data including prices, order books, candlestick charts, and trading history for cryptocurrency analysis and monitoring.  (12 tools) (node)
* [✅ @mektigboy/server-hyperliquid](https://github.com/mektigboy/server-hyperliquid) ⭐ 44 | 🐛 6 | 🌐 TypeScript | 📅 2025-03-06: Integrates with the Hyperliquid SDK to provide real-time cryptocurrency market data, including mid prices, historical candlesticks, and L2 order book information for traders and analysts.  (3 tools) (node)
* [✅ dexpaprika-mcp](https://github.com/coinpaprika/dexpaprika-mcp) ⭐ 42 | 🐛 0 | 🌐 JavaScript | 📅 2026-08-21: Provides real-time cryptocurrency market data across multiple blockchain networks, including DEX listings, liquidity pools, token details, and price analytics without requiring API keys.  (11 tools) (node)
* [✅ mcp-crypto-price](https://github.com/truss44/mcp-crypto-price) ⭐ 40 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-31: Integrates with CoinCap's API to provide real-time cryptocurrency data, enabling price tracking, market analysis, and historical trend examination for financial applications.  (3 tools) (node)
* [✅ binance-mcp-server](https://github.com/ethancod1ng/binance-mcp-server) ⭐ 32 | 🐛 0 | 🌐 TypeScript | 📅 2026-01-31: A Model Context Protocol server that provides Claude Code with seamless access to Binance exchange API functionality for market data retrieval, account management, and trading operations.  (10 tools) (node)
* [✅ moneybird-mcp-server](https://github.com/vanderheijden86/moneybird-mcp-server) ⭐ 28 | 🐛 1 | 🌐 TypeScript | 📅 2026-03-01: Bridges Moneybird accounting software with natural language interaction, enabling users to manage contacts, invoices, financial accounts, products, projects, and time entries through conversational prompts.  (10 tools) (node)
* [✅ lunchmoney-mcp](https://github.com/leafeye/lunchmoney-mcp-server) ⭐ 27 | 🐛 3 | 🌐 TypeScript | 📅 2025-05-01: Integrates with the Lunchmoney personal finance API to enable transaction retrieval, spending analysis, and budget summaries for enhanced financial management.  (4 tools) (node)
* [✅ @jeromyfu/app-insight-mcp](https://github.com/jiantaofu/appinsightmcp) ⭐ 25 | 🐛 1 | 🌐 JavaScript | 📅 2025-12-08: Analyze data from both the Apple App Store and Google Play Store  (20 tools) (node)
* [✅ bsv-mcp](https://github.com/b-open-io/bsv-mcp) ⭐ 21 | 🐛 2 | 🌐 TypeScript | 📅 2026-07-30: Integrates with Bitcoin SV blockchain to enable wallet operations, transaction management, and NFT interactions while maintaining local private key security  (9 tools) (node)
* [✅ fewsats-mcp](https://github.com/fewsats/fewsats-mcp) ⭐ 21 | 🐛 2 | 🌐 Python | 📅 2025-05-27: Integrates with the Fewsats payment platform to enable secure financial transactions through L402 protocol, allowing wallet balance checks, payment method retrieval, and autonomous purchasing capabilities.  (6 tools) (python)
* [✅ xero-mcp](https://github.com/john-zhang-dev/xero-mcp) ⭐ 21 | 🐛 5 | 🌐 TypeScript | 📅 2026-08-30: Integrates with Xero Accounting Software to access financial data including accounts, transactions, contacts, invoices, and more through authenticated API connections for financial analysis and bookkeeping tasks.  (12 tools) (node)
* [✅ fiscal-data-mcp](https://github.com/quantgeekdev/fiscal-data-mcp) ⭐ 20 | 🐛 2 | 🌐 TypeScript | 📅 2024-12-10: Integrates with the US Treasury's Fiscal Data API to fetch, analyze, and generate reports on treasury statements and historical financial data.  (1 tools) (node)
* [✅ @chargebee/mcp](https://github.com/chargebee/agentkit/tree/main/modelcontextprotocol) ⚠️ Archived: MCP Server that connects AI agents to Chargebee platform.  (2 tools) (node)
* [✅ @coinstats/coinstats-mcp](https://github.com/coinstatshq/coinstats-mcp) ⭐ 16 | 🐛 3 | 🌐 TypeScript | 📅 2026-09-01: Provides real-time cryptocurrency market data, portfolio tracking, and news through direct access to the CoinStats API for retrieving coin information, market trends, wallet balances, and exchange integrations.  (30 tools) (node)
* [✅ manifold-mcp-server](https://github.com/bmorphism/manifold-mcp-server) ⭐ 14 | 🐛 5 | 🌐 JavaScript | 📅 2025-01-11: Integrates with Manifold Markets prediction markets, enabling market search, analysis, betting, and portfolio management with precise limit orders and advanced filtering.  (9 tools) (node)
* [✅ @mcp-dockmaster/mcp-cryptowallet-evm](https://github.com/dcspark/mcp-cryptowallet-evm) ⭐ 10 | 🐛 2 | 🌐 TypeScript | 📅 2025-03-26: Enables direct interaction with Ethereum and EVM-compatible blockchains for wallet creation, balance checking, transaction sending, and smart contract operations through ethers.js v5.  (35 tools) (node)
* [✅ @zbddev/payments-sdk-mcp](https://github.com/zbdpay/zbd-payments-typescript-sdk) ⭐ 10 | 🐛 1 | 🌐 TypeScript | 📅 2026-09-01: Integrates with ZBD Payments API to enable Bitcoin and Lightning Network transactions including sending/receiving payments, wallet operations, voucher management, and OAuth2 authentication flows  (33 tools) (node)
* [✅ findata-mcp-server](https://github.com/xbluecode/findata-mcp-server) ⭐ 9 | 🐛 1 | 🌐 JavaScript | 📅 2025-04-03: Integrates with Alpha Vantage API to provide stock quotes and historical data for financial analysis and market research tasks.  (2 tools) (node)
* [✅ mercadolibre-mcp](https://github.com/lumile/mercadolibre-mcp) ⭐ 7 | 🐛 1 | 🌐 TypeScript | 📅 2025-05-28: Integrates with MercadoLibre's e-commerce platform to simplify product and seller data retrieval, enabling functions like price monitoring, inventory management, and market analysis.  (3 tools) (node)
* [✅ hubble-mcp-tool](https://github.com/hubblevision/hubble-ai-mcp) ⚠️ Archived: Provides a bridge to Solana blockchain data through natural language queries, enabling analytics searches, chart generation, and image downloads for visualizing transaction patterns, price movements, and token distributions.  (2 tools) (node)
* [✅ search-stock-news-mcp](https://github.com/cognitive-stack/search-stock-news-mcp) ⭐ 6 | 🐛 2 | 🌐 TypeScript | 📅 2025-05-25: Provides a specialized tool for retrieving stock-related news using the Tavily API, enabling financial searches by stock symbol and company name with configurable parameters for Vietnamese financial sources like CafeF and Nguoi Quan Sat.  (2 tools) (node)
* [✅ tesouro-direto-mcp](https://github.com/atilioa/tesouro-direto-mcp) ⭐ 5 | 🐛 6 | 🌐 TypeScript | 📅 2026-02-14: Provides real-time access to Brazil's Treasury Direct bond market data, enabling users to retrieve market status, search bonds by type and maturity date, and access detailed information for investment analysis.  (3 tools) (node)
* [✅ @akki91/ankr-mcp](https://github.com/akki91/ankr-mcp) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2026-05-02: Provides blockchain data access through the Ankr API, enabling wallet balance queries across multiple networks with detailed token metadata and dollar values for portfolio tracking and financial analysis.  (1 tools) (node)
* [✅ asset-price-mcp](https://github.com/mk965/asset-price-mcp) ⭐ 3 | 🐛 2 | 🌐 TypeScript | 📅 2025-11-30: Provides real-time financial data for precious metals, cryptocurrencies, and other assets, enabling market price tracking and historical trend analysis without direct API integrations to multiple financial sources.  (1 tools) (node)
* [✅ base-network-mcp-server](https://github.com/fakepixels/base-mcp-server) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2025-03-04: Provides a bridge to the Base blockchain network for wallet management, balance checking, and transaction execution through natural language commands, eliminating the need to manage technical blockchain details.  (4 tools) (node)
* [✅ setu\_mcp\_kyc](https://github.com/setuhq/setu-mcps/tree/HEAD/kyc) ⭐ 3 | 🐛 0 | 🌐 Python | 📅 2025-01-23: Integrates Setu's Digital Gateway APIs to provide KYC verification tools for PAN, GST, and name matching, enabling automated identity checks and regulatory compliance.  (3 tools) (python)
* [✅ setu\_mcp\_upi\_deeplinks](https://github.com/setuhq/setu-mcps/tree/HEAD/upi-deeplinks) ⭐ 3 | 🐛 0 | 🌐 Python | 📅 2025-01-23: Integrates Setu's UPI payment infrastructure to enable seamless generation and management of payment links for applications.  (5 tools) (python)
* [✅ bitrefill-mcp-server](https://github.com/bitrefill/bitrefill-mcp-server) ⭐ 2 | 🐛 4 | 🌐 TypeScript | 📅 2026-04-23: Integrates with Bitrefill's platform to search and retrieve information about gift cards, mobile refills, eSIMs, and digital services available for cryptocurrency purchases.  (12 tools) (node)
* [✅ @bujaayjaay/mcp-coincap-jj](https://github.com/wazzan/mcp-coincap-jj) ⭐ 1 | 🐛 9 | 🌐 TypeScript | 📅 2026-08-20: Integrates with CoinCap's API v3 to provide real-time cryptocurrency price tracking, market analysis, and historical trend data for informed investment decisions.  (3 tools) (node)
* [✅ @iqai/mcp-bamm](https://github.com/iqaicom/mcp-bamm) ⭐ 1 | 🐛 0 | 🌐 TypeScript | 📅 2026-06-16: Enables DeFi operations on Fraxtal blockchain by providing tools for managing BAMM positions, lending Fraxswap LP tokens, and borrowing against collateral  (7 tools) (node)
* [✅ mcp-lighthouse](https://github.com/l3wi/mcp-lighthouse) ⭐ 1 | 🐛 1 | 🌐 JavaScript | 📅 2025-03-09: Integrates with Lighthouse.one cryptocurrency portfolio tracker to fetch and display detailed portfolio data including total value, asset allocations, and major holdings through secure token-based authentication.  (5 tools) (node)
* [✅ @cerebrofoundation/mcp-intent](https://github.com/cerebrofoundation/mcp-intent) ⭐ 0 | 🐛 0 | 🌐 TypeScript | 📅 2025-03-22: Enables Bitcoin address generation and transaction signing using private keys in WIF format and partially signed Bitcoin transactions (PSBTs) through secure cryptographic operations.  (2 tools) (node)
* [✅ octav-api-mcp](https://github.com/Octav-Labs/octav-api-mcp) ⭐ 0 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-06: Multi-chain crypto portfolio tracking MCP server. Access wallet holdings, DeFi protocol positions, transaction history, and token analytics across 20+ blockchains directly from Claude.  (14 tools) (node)
* [✅ supercolony-mcp](https://github.com/TheSuperColony/supercolony-mcp) ⭐ 0 | 🐛 1 | 🌐 JavaScript | 📅 2026-03-12: Real-time intelligence from 140+ autonomous AI agents publishing on-chain observations, analyses, and predictions on Demos blockchain. Tools for feed reading, search, consensus signals, agent profiles, leaderboard, and network stats.  (7 tools) (node)
* [✅ obsd-launchpad-mcp](https://github.com/thryxagi/obsd-launchpad): MCP server for AI agents to deploy, trade, and earn on the OBSD LaunchPad (Base chain). 12 tools: launch tokens, buy/sell, claim fees, referrals, and analytics.  (17 tools) (node)

</details>

<a id="gaming"></a>

<details>
<summary><strong>Gaming</strong></summary>

Connect with gaming data, engines, and related services.

* [✅ @runreal/unreal-mcp](https://github.com/runreal/unreal-mcp) ⭐ 115 | 🐛 5 | 🌐 Python | 📅 2025-06-06: Integrates with Unreal Engine to assist with game development workflows.  (20 tools) (node)
* [✅ @jayarrowz/mcp-osrs](https://github.com/jayarrowz/mcp-osrs) ⭐ 31 | 🐛 2 | 🌐 JavaScript | 📅 2026-06-08: Provides tools for accessing Old School RuneScape game data through wiki searches and structured file queries with pagination support  (19 tools) (node)
* [✅ mcp-chess](https://github.com/jiayao/mcp-chess) ⭐ 23 | 🐛 2 | 🌐 Python | 📅 2025-05-05: Enables playing chess against language models through a visual interface with tools for board visualization, move execution, game initialization, and position analysis from PGN notation.  (6 tools) (python)
* [✅ steam-review-mcp](https://github.com/fenxer/steam-review-mcp) ⭐ 7 | 🐛 2 | 🌐 TypeScript | 📅 2026-01-28: Integrates with Steam's API to fetch and analyze game reviews and information, enabling customizable queries for player feedback, sentiment tracking, and game details.  (1 tools) (node)
* [✅ server-dice-roll](https://github.com/lpbayliss/server-dice-roll) ⭐ 2 | 🐛 13 | 🌐 TypeScript | 📅 2026-08-29: Provides cryptographically secure dice rolling with standard notation and Fate/Fudge dice support for tabletop RPGs and games requiring reliable random number generation.  (2 tools) (node)
* [✅ pokemon-paste-mcp](https://github.com/jpbullalayao/pokemon-paste-mcp) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2025-06-12: Converts Pokemon team compositions into shareable Pokepaste URLs by accepting detailed Pokemon configurations including stats, moves, abilities, items, and competitive metadata, then automatically uploading the formatted team data to pokepast.es for easy collaboration among competitive players and team builders.  (1 tools) (node)
* [✅ pokemon-vgc-calc-mcp](https://github.com/jpbullalayao/pokemon-vgc-calc-mcp) ⭐ 1 | 🐛 0 | 🌐 TypeScript | 📅 2025-06-11: Provides Pokemon VGC damage calculation capabilities using the Smogon calc library, computing battle damage ranges, KO probabilities, and detailed combat scenarios for competitive Pokemon analysis and strategy optimization.  (1 tools) (node)

</details>

<a id="knowledge-memory"></a>

<details>
<summary><strong>Knowledge & Memory</strong></summary>

Store and query structured information for AI models to use across sessions.

* [✅ @modelcontextprotocol/server-aws-kb-retrieval](https://github.com/modelcontextprotocol/servers/blob/main/src/aws-kb-retrieval-server) ⭐ 90,001 | 🐛 515 | 🌐 TypeScript | 📅 2026-08-31: MCP server for AWS Knowledge Base retrieval using Bedrock Agent Runtime  (1 tools) (node)
* [✅ @modelcontextprotocol/server-memory](https://github.com/modelcontextprotocol/servers/tree/HEAD/src/memory) ⭐ 90,001 | 🐛 515 | 🌐 TypeScript | 📅 2026-08-31: Build and query persistent semantic networks for data management.  (9 tools) (node)
* [✅ awslabs.bedrock-kb-retrieval-mcp-server](https://github.com/awslabs/mcp/tree/HEAD/src/bedrock-kb-retrieval-mcp-server) ⭐ 9,652 | 🐛 247 | 🌐 Python | 📅 2026-09-01: Bridge to access Amazon Bedrock Knowledge Bases.  (2 tools) (python)
* [✅ @mem0/mcp-server](https://github.com/mem0ai/mem0-mcp) ⚠️ Archived: A Model Context Protocol (MCP) server that provides memory storage and retrieval capabilities using Mem0. This tool allows you to store and search through memories, making it useful for maintaining context and making informed decisions based on past interactions.  (2 tools) (node)
* [✅ mindmap-mcp-server](https://github.com/yuchenssr/mindmap-mcp-server) ⭐ 236 | 🐛 5 | 🌐 Python | 📅 2025-05-20: Transforms Markdown content into interactive HTML-based mind maps using markmap-cli, enabling visual organization of structured information and knowledge representation.  (1 tools) (python)
* [✅ gistpad-mcp](https://github.com/lostintangent/gistpad-mcp) ⭐ 203 | 🐛 2 | 🌐 TypeScript | 📅 2026-01-16: Transforms GitHub Gists into a personal knowledge management system with specialized handling for daily notes, reusable prompts with frontmatter support, and comprehensive gist operations including creation, updating, archiving, and commenting for version-controlled knowledge storage.  (28 tools) (node)
* [✅ notion-mcp-server](https://github.com/awkoy/notion-mcp-server) ⭐ 168 | 🐛 14 | 🌐 TypeScript | 📅 2026-08-26: Enables AI systems to create, update, and manage Notion pages and blocks through a Node.js bridge to the Notion API, supporting efficient batch operations for documentation and knowledge base maintenance.  (5 tools) (node)
* [✅ mcp-outline](https://github.com/vortiago/mcp-outline) ⭐ 155 | 🐛 12 | 🌐 Python | 📅 2026-08-05: Enables AI systems to search, read, edit, and manage documents within Outline's knowledge management platform through direct API integration with both cloud and self-hosted instances.  (25 tools) (python)
* [✅ @readwise/readwise-mcp](https://github.com/readwiseio/readwise-mcp) ⭐ 152 | 🐛 6 | 🌐 JavaScript | 📅 2026-03-14: Integrates with Readwise to search and retrieve highlights from a user's library, enabling researchers and knowledge workers to reference saved notes during conversations without switching contexts.  (1 tools) (node)
* [✅ @professional-wiki/mediawiki-mcp-server](https://github.com/professionalwiki/mediawiki-mcp-server) ⭐ 125 | 🐛 11 | 🌐 TypeScript | 📅 2026-08-31: Integrates with MediaWiki instances through REST API to enable searching pages, retrieving content in multiple formats, accessing file information, viewing revision history, and performing authenticated operations like creating and updating pages with automatic wiki discovery and dynamic configuration management.  (7 tools) (node)
* [✅ @gleanwork/mcp-server](https://github.com/gleanwork/mcp-server) ⚠️ Archived: Integrates with Glean's enterprise knowledge platform to provide company search, people profile lookup, and AI assistant capabilities directly within your workflow.  (3 tools) (node)
* [✅ @itseasy21/mcp-knowledge-graph](https://github.com/itseasy21/mcp-knowledge-graph) ⭐ 61 | 🐛 1 | 🌐 JavaScript | 📅 2025-08-08: Provides persistent memory for Claude through a local knowledge graph that stores entities with observations and relations, enabling structured information retrieval and complex context retention across conversations.  (11 tools) (node)
* [✅ mcp-think-tank](https://github.com/flight505/mcp-think-tank) ⭐ 61 | 🐛 1 | 🌐 TypeScript | 📅 2025-07-14: Provides structured reasoning and persistent knowledge graph capabilities for complex problem-solving with transparent thinking processes and memory across conversations.  (20 tools) (node)
* [✅ mcpunk](https://github.com/jurasofish/mcpunk) ⭐ 56 | 🐛 5 | 🌐 Python | 📅 2025-06-01: MCPunk provides tools for performing Roaming RAG  (11 tools) (python)
* [✅ @movibe/memory-bank-mcp](https://github.com/movibe/memory-bank-mcp) ⚠️ Archived: TypeScript-based server for tracking project context across sessions, enabling persistent knowledge sharing through modular, markdown-based memory management with support for multiple development modes.  (15 tools) (node)
* [✅ tana-mcp](https://github.com/tim-mcdonnell/tana-mcp) ⚠️ Archived: Integrates with Tana's Input API, enabling creation and manipulation of structured data in Tana workspaces for enhanced note-taking and automated data input tasks.  (11 tools) (node)
* [✅ @cgize/mcp-think-tool](https://github.com/cgize/claude-mcp-think-tool) ⭐ 38 | 🐛 1 | 🌐 JavaScript | 📅 2025-04-11: Provides Claude with a dedicated workspace for structured reasoning through four specialized tools that enable recording, retrieving, clearing, and analyzing thoughts during complex problem-solving tasks.  (4 tools) (node)
* [✅ esa-mcp-server](https://github.com/d-kimuson/esa-mcp-server) ⭐ 37 | 🐛 4 | 🌐 TypeScript | 📅 2025-11-02: Integrates with esa.io API for searching and retrieving team knowledge, enabling efficient information access and collaboration.  (3 tools) (node)
* [✅ onyx-mcp-server](https://github.com/lupuletic/onyx-mcp-server) ⭐ 28 | 🐛 4 | 🌐 TypeScript | 📅 2025-05-21: Bridges Onyx knowledge bases with semantic search and chat capabilities, enabling teams to access organizational knowledge through RAG-powered document retrieval with configurable context windows.  (2 tools) (node)
* [✅ systemprompt-mcp-notion](https://github.com/ejb503/systemprompt-mcp-notion) ⭐ 27 | 🐛 3 | 🌐 TypeScript | 📅 2025-02-18: Bridges to Notion's knowledge management system, enabling creation and manipulation of pages, databases, and content.  (8 tools) (node)
* [✅ think-mcp-server](https://github.com/marcopesani/think-mcp-server) ⭐ 24 | 🐛 5 | 🌐 TypeScript | 📅 2025-08-01: Provides a dedicated thinking space for complex reasoning with encouraging feedback, enabling step-by-step analysis and memory retention without external actions.  (1 tools) (node)
* [✅ @neko0721/memory-bank-mcp](https://github.com/hoppo-chan/memory-bank-mcp) ⭐ 16 | 🐛 0 | 🌐 JavaScript | 📅 2025-06-14: Maintains persistent project context through a structured Memory Bank system of five markdown files that track goals, status, progress, decisions, and patterns with automatic timestamp tracking and workflow guidance for consistent documentation across development sessions.  (3 tools) (node)
* [✅ memory-mcp-server](https://github.com/evangstav/python-memory-mcp-server) ⭐ 16 | 🐛 1 | 🌐 Python | 📅 2026-04-04: Provides a knowledge graph management system for storing, retrieving, and querying information to build and maintain long-term memory across conversations.  (9 tools) (python)
* [✅ think-tool-mcp](https://github.com/abhinav-mangla/think-tool-mcp) ⭐ 16 | 🐛 0 | 🌐 TypeScript | 📅 2025-08-12: Provides a structured thought process management system for maintaining explicit reasoning steps, policy verification, and tool output analysis through persistent memory storage  (1 tools) (node)
* [✅ logseq-mcp](https://github.com/apw124/logseq-mcp) ⭐ 15 | 🐛 3 | 🌐 Python | 📅 2025-07-02: Enables AI interaction with Logseq knowledge graphs for capturing notes, organizing information, and retrieving knowledge from personal databases  (13 tools) (python)
* [✅ @kj455/mcp-kibela](https://github.com/kj455/mcp-kibela) ⭐ 13 | 🐛 2 | 🌐 TypeScript | 📅 2025-04-06: Integrates with Kibela knowledge bases to enable searching, reading, creating, and updating notes directly within conversation interfaces, providing seamless access to organizational documentation.  (6 tools) (node)
* [✅ @landicefu/divide-and-conquer-mcp-server](https://github.com/landicefu/divide-and-conquer-mcp-server) ⭐ 7 | 🐛 2 | 🌐 JavaScript | 📅 2025-06-26: Enables breaking down complex tasks into manageable pieces with structured JSON storage for tracking progress, maintaining checklists, and preserving context across multiple conversations.  (15 tools) (node)
* [✅ notion-readonly-mcp-server](https://github.com/taewoong1378/notion-readonly-mcp-server) ⭐ 4 | 🐛 1 | 🌐 TypeScript | 📅 2025-05-07: Provides an optimized read-only interface to Notion content with parallel processing and intelligent caching for faster document analysis and knowledge retrieval.  (8 tools) (node)
* [✅ @iqai/mcp-iqwiki](https://github.com/iqaicom/mcp-iqwiki) ⭐ 2 | 🐛 1 | 🌐 TypeScript | 📅 2026-06-23: Provides structured access to IQ.wiki's blockchain encyclopedia data through GraphQL queries for retrieving specific wikis, user contributions, and detailed content change histories  (5 tools) (node)
* [✅ flomo-mcp](https://github.com/chatmcp/flomo-mcp) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2025-05-10: Enables direct note-taking in Flomo by capturing Markdown content from conversations and posting it to your knowledge base via webhook integration  (1 tools) (node)
* [✅ sourcesyncai-mcp](https://github.com/scmdr/sourcesyncai-mcp) ⭐ 1 | 🐛 1 | 📅 2025-03-03: Integrates with SourceSync.ai's knowledge management platform to enable semantic search, document management, and content ingestion from diverse sources for AI-driven knowledge retrieval and document analysis.  (25 tools) (node)
* [✅ @suekou/mcp-notion-server](https://github.com/nanahiryu/notion-mcp-server) ⭐ 0 | 🐛 1 | 🌐 JavaScript | 📅 2025-03-24: Integrates with Notion's API to enable document management, knowledge base maintenance, and collaborative workflows through comprehensive tools for managing blocks, pages, databases, users, and comments.  (18 tools) (node)

</details>

<a id="location-services"></a>

<details>
<summary><strong>Location Services</strong></summary>

Work with maps, weather, and location-based data for analytics and insights.

* [✅ @baidumap/mcp-server-baidu-map](https://github.com/baidu-maps/mcp) ⭐ 440 | 🐛 14 | 🌐 Python | 📅 2025-08-22: Integrates with Baidu Maps API for location-based operations including geocoding, route planning, and location search within the Baidu Maps ecosystem.  (10 tools) (node)
* [✅ @cablate/mcp-google-map](https://github.com/cablate/mcp-google-map) ⭐ 439 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-16: Integrates with Google Maps API to enable location-based operations like place searching, geocoding, and geographical information retrieval within conversations.  (7 tools) (node)
* [✅ @mapbox/mcp-server](https://github.com/mapbox/mcp-server) ⭐ 353 | 🐛 10 | 🌐 TypeScript | 📅 2026-08-27: Geospatial intelligence with Mapbox APIs like geocoding, POI search, directions, isochrones, etc.  (9 tools) (node)
* [✅ @modelcontextprotocol/server-google-maps](https://github.com/modelcontextprotocol/servers-archived/tree/HEAD/src/google-maps) ⚠️ Archived: Access location data, geocoding, and place details through Maps API.  (7 tools) (node)
* [✅ gis-dataconversion-mcp](https://github.com/ronantakizawa/gis-dataconversion-mcp) ⭐ 16 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-30: Provides a bridge between geographic data formats, enabling seamless conversion between WKT, GeoJSON, CSV, TopoJSON, and KML with support for reverse geocoding and topology preservation.  (9 tools) (node)
* [✅ hefeng-mcp-weather](https://github.com/shanggqm/hefeng-mcp-weather) ⭐ 9 | 🐛 6 | 🌐 TypeScript | 📅 2025-02-26: Integrates with the HeFeng Weather API to provide real-time weather information and forecasts for locations in China, supporting queries by coordinates and offering hourly and daily data.  (1 tools) (node)
* [✅ @zealgeo/mcp-geo-server](https://github.com/nodegis/geo-mcp-server) ⭐ 3 | 🐛 2 | 🌐 JavaScript | 📅 2025-04-18: Provides geospatial calculation capabilities for coordinate conversion and spatial analysis, enabling GIS applications and geometry processing without complex algorithm implementation.  (3 tools) (node)
* [✅ @swonixs/weatherapi-mcp](https://github.com/swonixs/weatherapi-mcp) ⭐ 2 | 🐛 2 | 🌐 JavaScript | 📅 2025-03-21: Provides current weather and air quality data for any city through WeatherAPI.com, requiring only an API key for temperature, humidity, wind speed, and optional air quality metrics.  (1 tools) (node)
* [✅ mcp-weather-demo](https://github.com/nick-telsan/mcp) ⭐ 0 | 🐛 0 | 🌐 TypeScript | 📅 2025-07-01: A demonstration server for ModelContextProtocol that provides weather tools and prompts for Claude AI, allowing LLMs to access external data without manual copying.  (2 tools) (node)

</details>

<a id="marketing"></a>

<details>
<summary><strong>Marketing</strong></summary>

Create and edit marketing content, manage metadata, and refine product positioning.

* [✅ tiktok-ads-mcp-server](https://github.com/AdsMCP/tiktok-ads-mcp-server) ⭐ 48 | 🐛 0 | 🌐 Python | 📅 2026-07-04: TikTok Ads MCP Server – Model Context Protocol Server for TikTok Ads Marketing API Integration  (11 tools) (python)
* [✅ instagram-engagement-mcp](https://github.com/bob-lance/instagram-engagement-mcp) ⭐ 46 | 🐛 3 | 🌐 JavaScript | 📅 2025-04-26: Enables detailed Instagram interaction analysis by processing comments, user profiles, and post metrics to extract demographic insights and identify potential marketing leads through a private API integration.  (5 tools) (node)
* [✅ linkedin-mcp-runner](https://github.com/ertiqah/linkedin-mcp-runner) ⭐ 30 | 🐛 0 | 🌐 JavaScript | 📅 2026-03-28: Integrates with LinkedIn to have post access, scheduling, and voice-tuned generation using LiGo's API.  (10 tools) (node)
* [✅ smartlead-mcp-by-leadmagic](https://github.com/leadmagic/smartlead-mcp-server) ⚠️ Archived: Integrates with SmartLead's cold email outreach platform to manage campaigns, leads, email accounts, and analytics with smart tool loading that categorizes 113+ tools into essential, advanced, and administrative tiers for streamlined email automation workflows.  (50 tools) (node)
* [✅ @ashdev/discourse-mcp-server](https://github.com/ashdevfr/discourse-mcp-server) ⭐ 5 | 🐛 4 | 🌐 JavaScript | 📅 2025-03-10: Enables searching and retrieving content from Discourse forums through a single tool that queries posts using the discourse2 npm package.  (1 tools) (node)
* [✅ @feedmob/jampp-reporting](https://github.com/feed-mob/fm-mcp-servers/tree/HEAD/src/jampp-reporting) ⭐ 3 | 🐛 2 | 🌐 JavaScript | 📅 2026-08-31: Integrates with Jampp Reporting API to retrieve advertising campaign performance metrics including spend, impressions, clicks, and installs across specified date ranges with automated authentication handling.  (2 tools) (node)
* [✅ @feedmob/kayzen-reporting](https://github.com/feed-mob/fm-mcp-servers/tree/HEAD/src/kayzen-reporting) ⭐ 3 | 🐛 2 | 🌐 JavaScript | 📅 2026-08-31: Enables marketers and analysts to retrieve and analyze Kayzen advertising campaign data through API access with optional date filtering  (2 tools) (node)
* [✅ @feedmob/singular-reporting](https://github.com/feed-mob/fm-mcp-servers/tree/HEAD/src/singular-reporting) ⭐ 3 | 🐛 2 | 🌐 JavaScript | 📅 2026-08-31: Enables marketers to create, monitor, and download customized Singular marketing analytics reports with campaign performance metrics across different apps and sources  (2 tools) (node)
* [✅ @toolsdk.ai/google-analytics-mcp](https://github.com/Seey215/google-analytics-mcp) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2026-01-12: MCP server for Google Analytics Data API  (5 tools) (node)
* [✅ @toolsdk.ai/mcp-server-google-analytics](https://github.com/smithery-ai/mcp-server-google-analytics): An MCP server implementation for accessing Google Analytics 4 (GA4) data, built using the Model Context Protocol TypeScript SDK.  (2 tools) (node)

</details>

<a id="monitoring"></a>

<details>
<summary><strong>Monitoring</strong></summary>

Analyze app performance and error reports with monitoring tools.

* [✅ mcp-server-aliyun-observability](https://github.com/aliyun/alibabacloud-observability-mcp-server) ⭐ 165 | 🐛 25 | 🌐 Python | 📅 2026-08-12: Integrates with Alibaba Cloud's monitoring and logging services, enabling log structure queries, log searches, application monitoring, and trace data analysis for troubleshooting cloud applications  (18 tools) (python)
* [✅ sonarqube-mcp-server](https://github.com/sapientpants/sonarqube-mcp-server) ⚠️ Archived: Integrates with SonarQube to provide code quality metrics, issue tracking, and quality gate status information for software development projects  (28 tools) (node)
* [✅ appsignal-mcp-server](https://github.com/pulsemcp/mcp-servers/tree/HEAD/experimental/appsignal) ⭐ 80 | 🐛 44 | 🌐 TypeScript | 📅 2026-08-30: Integrates with AppSignal's monitoring platform to provide incident tracking, anomaly detection, performance monitoring, and log analysis with severity filtering and time-based queries for debugging production applications.  (2 tools) (node)
* [✅ @raygun.io/mcp-server-raygun](https://github.com/MindscapeHQ/mcp-server-raygun) ⭐ 22 | 🐛 3 | 📅 2026-03-02: MCP server for interacting with Raygun's API for crash reporting and real user monitoring metrics  (32 tools) (node)
* [✅ agentops-mcp](https://github.com/agentops-ai/agentops-mcp) ⭐ 14 | 🐛 1 | 🌐 JavaScript | 📅 2025-07-30: Provides access to AgentOps observability and tracing data for debugging agent runs, enabling retrieval of project information, trace details, span metrics, and complete execution traces through authenticated API access.  (4 tools) (node)
* [✅ stdout-mcp-server](https://github.com/amitdeshmukh/stdout-mcp-server) ⭐ 7 | 🐛 3 | 🌐 TypeScript | 📅 2025-03-07: Lightweight server that captures and manages stdout logs from multiple processes through a named pipe system, maintaining a 100-entry log history and providing robust querying and filtering capabilities for debugging and real-time monitoring.  (1 tools) (node)
* [✅ @hiyorineko/mcp-rollbar-server](https://github.com/hiyorineko/mcp-rollbar-server) ⭐ 6 | 🐛 0 | 🌐 TypeScript | 📅 2025-04-18: Provides a bridge to Rollbar error tracking platform for monitoring and analyzing application errors, retrieving detailed information, managing projects, and tracking deployments.  (13 tools) (node)
* [✅ mcp-solarwinds](https://github.com/jakenuts/mcp-solarwinds) ⭐ 6 | 🐛 2 | 🌐 TypeScript | 📅 2025-03-06: Integrates with SolarWinds Observability logs, providing tools for searching, visualizing, and analyzing log data with advanced filtering options and customizable time ranges for DevOps and IT operations teams.  (2 tools) (node)
* [✅ sentry-issues-mcp](https://github.com/leee62/sentry-issues-mcp) ⭐ 5 | 🐛 2 | 🌐 JavaScript | 📅 2025-05-26: Integrates with Sentry error tracking to retrieve detailed event and issue data for analyzing application exceptions and errors in development workflows.  (2 tools) (node)
* [✅ @kajirita2002/honeycomb-mcp-server](https://github.com/kajirita2002/honeycomb-mcp-server) ⭐ 3 | 🐛 2 | 🌐 JavaScript | 📅 2025-03-28: Provides a direct TypeScript-based interface for interacting with Honeycomb's observability API, enabling developers to query, create, and manage datasets, events, and monitoring resources through natural language interaction.  (11 tools) (node)

</details>

<a id="search-data-extraction"></a>

<details>
<summary><strong>Search & Data Extraction</strong></summary>

Find and extract data from various sources quickly and efficiently.

* [✅ @modelcontextprotocol/server-aws-kb-retrieval](https://github.com/modelcontextprotocol/servers/blob/main/src/aws-kb-retrieval-server) ⭐ 90,001 | 🐛 515 | 🌐 TypeScript | 📅 2026-08-31: MCP server for AWS Knowledge Base retrieval using Bedrock Agent Runtime  (1 tools) (node)
* [✅ @upstash/context7-mcp](https://github.com/upstash/context7) ⭐ 61,479 | 🐛 68 | 🌐 TypeScript | 📅 2026-09-01: Connects to Context7.com's documentation database to provide up-to-date library and framework documentation with intelligent project ranking and customizable token limits.  (2 tools) (node)
* [✅ awslabs.aws-documentation-mcp-server](https://github.com/awslabs/mcp/tree/HEAD/src/aws-documentation-mcp-server) ⭐ 9,652 | 🐛 247 | 🌐 Python | 📅 2026-09-01: Provides tools to access AWS documentation, search for content, and get recommendations.  (3 tools) (python)
* [✅ @sourcebot/mcp](https://github.com/sourcebot-dev/sourcebot) ⭐ 3,924 | 🐛 107 | 🌐 TypeScript | 📅 2026-08-31: Enables code search across multiple repository hosts including GitHub, GitLab, Gitea, Gerrit, and Bitbucket with advanced filtering options for exploring large codebases through natural language queries.  (3 tools) (node)
* [✅ arxiv-mcp-server](https://github.com/blazickjp/arxiv-mcp-server) ⭐ 3,100 | 🐛 7 | 🌐 Python | 📅 2026-08-26: Search and analyze academic papers from the arXiv repository.  (4 tools) (python)
* [✅ @toolsdk.ai/tavily-mcp](https://github.com/tavily-ai/tavily-mcp) ⭐ 2,365 | 🐛 48 | 🌐 JavaScript | 📅 2026-08-20: An MCP server that implements web search, extract, mapping, and crawling through the Tavily API.  (4 tools) (node)
* [✅ mcp/tavily](https://github.com/tavily-ai/tavily-mcp) ⭐ 2,365 | 🐛 48 | 🌐 JavaScript | 📅 2026-08-20: Tavily is a search engine optimized for LLMs and RAG, aimed at efficient, quick and persistent search results. (Docker Runtime)  (4 tools) (docker)
* [✅ @brave/brave-search-mcp-server](https://github.com/brave/brave-search-mcp-server) ⭐ 1,411 | 🐛 34 | 🌐 TypeScript | 📅 2026-08-26: Provides a bridge to the Brave Search API for performing web, image, video, news, and local business searches with configurable parameters and robust error handling.  (6 tools) (node)
* [✅ mcp-deepwiki](https://github.com/regenrek/deepwiki-mcp) ⭐ 1,383 | 🐛 10 | 🌐 TypeScript | 📅 2026-03-20: Converts DeepWiki repositories into well-formatted Markdown, maintaining links between pages while removing headers, footers, navigation elements, and ads for clean documentation extraction.  (1 tools) (node)
* [✅ ref-tools-mcp](https://github.com/ref-tools/ref-tools-mcp) ⭐ 1,164 | 🐛 6 | 🌐 TypeScript | 📅 2026-06-26: Integrates with Ref.tools documentation search service to provide curated technical documentation access, web search fallback, and URL-to-markdown conversion for efficient developer reference during coding workflows.  (2 tools) (node)
* [✅ @kimtaeyoon83/mcp-server-youtube-transcript](https://github.com/kimtaeyoon83/mcp-server-youtube-transcript) ⭐ 590 | 🐛 14 | 🌐 TypeScript | 📅 2026-07-21: Extract and analyze video captions and subtitles in multiple languages.  (1 tools) (node)
* [✅ @anaisbetts/mcp-youtube](https://github.com/anaisbetts/mcp-youtube) ⭐ 544 | 🐛 11 | 🌐 TypeScript | 📅 2026-08-16: Extract and analyze video subtitle data for content understanding.  (1 tools) (node)
* [✅ docfork](https://github.com/docfork/docfork-mcp) ⚠️ Archived: Retrieves up-to-date documentation and code examples for any software library through the Docfork API, automatically selecting relevant libraries and providing topic-focused documentation with configurable response size limits.  (1 tools) (node)
* [✅ graphlit-mcp-server](https://github.com/graphlit/graphlit-mcp-server) ⭐ 379 | 🐛 6 | 🌐 TypeScript | 📅 2026-01-12: Graphlit MCP Server for AI, RAG, OpenAI, PDF parsing and preprocessing  (64 tools) (node)
* [✅ mcp-omnisearch](https://github.com/spences10/mcp-omnisearch) ⭐ 347 | 🐛 2 | 🌐 TypeScript | 📅 2026-09-01: Unifies search and content processing by dynamically selecting optimal providers like Tavily, Brave, and Perplexity to enable flexible information retrieval and enhancement across multiple domains.  (15 tools) (node)
* [✅ @mzxrai/mcp-webresearch](https://github.com/mzxrai/mcp-webresearch) ⚠️ Archived: Research topics using Google search and web scraping.  (3 tools) (node)
* [✅ @modelcontextprotocol/server-brave-search](https://github.com/modelcontextprotocol/servers-archived/tree/HEAD/src/brave-search) ⚠️ Archived: Retrieve web pages, news, and local business results via Brave API.  (2 tools) (node)
* [✅ g-search-mcp](https://github.com/jae-jae/g-search-mcp) ⭐ 272 | 🐛 5 | 🌐 TypeScript | 📅 2025-06-14: Automates parallel Google searches with intelligent CAPTCHA detection, browser state persistence, and user behavior simulation to bypass anti-bot measures while returning structured results.  (1 tools) (node)
* [✅ mcp-trends-hub](https://github.com/baranwang/mcp-trends-hub) ⭐ 269 | 🐛 5 | 🌐 TypeScript | 📅 2026-01-31: Provides real-time access to trending topics and content from major Chinese platforms including Weibo, Zhihu, Douyin, Bilibili, Douban, Toutiao, and 36kr through separate tools with temporary caching for improved performance.  (21 tools) (node)
* [✅ mcp-server-gsc](https://github.com/ahonn/mcp-server-gsc) ⭐ 261 | 🐛 2 | 🌐 TypeScript | 📅 2026-02-19: Analyze SEO metrics and search performance data.  (6 tools) (node)
* [✅ mcp-maigret](https://github.com/burtthecoder/mcp-maigret) ⭐ 259 | 🐛 6 | 🌐 JavaScript | 📅 2026-01-27: OSINT Maigret integration to gather user info across social networks.  (2 tools) (node)
* [✅ backlinks-mcp](https://github.com/cnych/seo-mcp) ⭐ 257 | 🐛 4 | 🌐 Python | 📅 2025-04-14: Retrieves detailed backlink data from Ahrefs including anchor text, domain rating, and URL information for SEO analysis and link building strategies.  (1 tools) (python)
* [✅ mcp-server-reddit](https://github.com/hawstein/mcp-server-reddit) ⭐ 184 | 🐛 7 | 🌐 Python | 📅 2026-06-10: Integrates with Reddit's API to fetch and retrieve diverse content including posts, comments, and subreddit information for data analysis and content curation.  (8 tools) (python)
* [✅ agentql-mcp](https://github.com/tinyfish-io/agentql-mcp) ⭐ 178 | 🐛 7 | 🌐 JavaScript | 📅 2026-08-24: Extracts structured data from web pages based on natural language descriptions, converting website content into JSON format without custom scraping code.  (1 tools) (node)
* [✅ search1api-mcp](https://github.com/fatwang2/search1api-mcp) ⭐ 173 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-28: Execute web searches, news queries, and content extraction.  (6 tools) (node)
* [✅ scrapeless-mcp-server](https://github.com/scrapeless-ai/scrapeless-mcp-server) ⭐ 168 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-28: Provides a bridge to the Scrapeless API for performing Google searches with customizable parameters including query text, country code, and language preferences.  (2 tools) (node)
* [✅ serper-search-scrape-mcp-server](https://github.com/marcopesani/mcp-server-serper) ⭐ 165 | 🐛 6 | 🌐 TypeScript | 📅 2025-03-13: Integrates with the Serper API to enable web searches and webpage content extraction, supporting research, content aggregation, and data mining tasks.  (2 tools) (node)
* [✅ @cyanheads/pubmed-mcp-server](https://github.com/cyanheads/pubmed-mcp-server) ⭐ 142 | 🐛 9 | 🌐 TypeScript | 📅 2026-08-21: Enables AI systems to search, retrieve, and analyze biomedical literature from PubMed for evidence-based research, citation generation, and data visualization  (5 tools) (node)
* [✅ one-search-mcp](https://github.com/yokingma/one-search-mcp) ⭐ 139 | 🐛 4 | 🌐 TypeScript | 📅 2026-07-31: Provides a unified search and web scraping platform that integrates multiple search providers like SearxNG and Tavily, along with Firecrawl for advanced web content extraction, enabling flexible web data retrieval and structured information gathering.  (4 tools) (node)
* [✅ @chanmeng666/google-news-server](https://github.com/ChanMeng666/server-google-news) ⭐ 126 | 🐛 5 | 🌐 TypeScript | 📅 2026-07-08: MCP server for Google News search via SerpAPI  (1 tools) (node)
* [✅ brave-search-mcp](https://github.com/mikechao/brave-search-mcp) ⭐ 125 | 🐛 1 | 🌐 TypeScript | 📅 2026-05-28: Provides a bridge to the Brave Search API for performing web, image, video, news, and local business searches with configurable parameters and robust error handling.  (5 tools) (node)
* [✅ mcp-package-version](https://github.com/sammcj/mcp-package-version) ⚠️ Archived: Get package version data from npm and PyPI registries to assist with dependency management.  (11 tools) (node)
* [✅ mcp-server-perplexity](https://github.com/tanigami/mcp-server-perplexity) ⭐ 94 | 🐛 3 | 🌐 Python | 📅 2024-12-25: Get chat completions with citations from Perplexity API.  (1 tools) (python)
* [✅ octagon-deep-research-mcp](https://github.com/octagonai/octagon-deep-research-mcp) ⭐ 94 | 🐛 1 | 🌐 JavaScript | 📅 2026-02-09: Integrates with Octagon API to provide multi-source data aggregation, web scraping, academic research synthesis, competitive analysis, market intelligence, technical analysis, policy research, and trend analysis for professional-grade research capabilities.  (1 tools) (node)
* [✅ clinicaltrialsgov-mcp-server](https://github.com/cyanheads/clinicaltrialsgov-mcp-server) ⭐ 91 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-21: Integrates with ClinicalTrials.gov REST API to search clinical trials by conditions, interventions, locations, and status, plus retrieve detailed study information by NCT ID with automatic data cleaning and local backup storage.  (2 tools) (node)
* [✅ cryo-mcp](https://github.com/z80dev/cryo-mcp) ⭐ 91 | 🐛 1 | 🌐 Python | 📅 2025-03-10: Provides a powerful Ethereum blockchain data extraction and analysis interface using Cryo and DuckDB, enabling efficient SQL-based querying of on-chain datasets with advanced filtering capabilities.  (10 tools) (python)
* [✅ @xiaohui-wang/mcpadvisor](https://github.com/istarwyh/mcpadvisor) ⭐ 89 | 🐛 7 | 🌐 TypeScript | 📅 2026-03-07: Discovery and recommendation service that helps find and understand available MCP services based on natural language queries, supporting multiple search backends for exploring servers by semantic similarity.  (2 tools) (node)
* [✅ anilist-mcp](https://github.com/yuna0x0/anilist-mcp) ⭐ 85 | 🐛 2 | 🌐 TypeScript | 📅 2026-07-13: MCP server that interfaces with the AniList API, allowing LLM clients to access and interact with anime, manga, character, staff, and user data from AniList  (44 tools) (node)
* [✅ mcp-gemini-google-search](https://github.com/yukukotani/mcp-gemini-google-search) ⭐ 82 | 🐛 1 | 🌐 TypeScript | 📅 2025-07-20: Provides Google Search functionality through Gemini's native grounding capabilities, delivering search results with automatic source citations and grounding metadata for reliable information retrieval.  (1 tools) (node)
* [✅ mcp-hn](https://github.com/erithwik/mcp-hn) ⭐ 76 | 🐛 5 | 🌐 Python | 📅 2025-07-14: Integrates with Hacker News to fetch stories, comments, and user data, enabling tech news aggregation, trend analysis, and community engagement tracking.  (4 tools) (python)
* [✅ mcp-ripgrep](https://github.com/mcollina/mcp-ripgrep) ⭐ 74 | 🐛 6 | 🌐 JavaScript | 📅 2025-04-29: Provides high-performance text search capabilities by wrapping ripgrep, enabling powerful file exploration, pattern matching, and content discovery across local filesystems.  (5 tools) (node)
* [✅ @adenot/mcp-google-search](https://github.com/adenot/mcp-google-search) ⭐ 68 | 🐛 5 | 🌐 JavaScript | 📅 2025-09-24: Integrates Google Custom Search API to enable web searches for fact-checking, research, and content generation tasks.  (1 tools) (node)
* [✅ @mcp-get-community/server-curl](https://github.com/mcp-get/community-servers/blob/main/src/server-curl) ⚠️ Archived: MCP server for making HTTP requests using a curl-like interface  (1 tools) (node)
* [✅ youtube-data-mcp-server](https://github.com/icraft2170/youtube-data-mcp-server) ⭐ 65 | 🐛 5 | 🌐 JavaScript | 📅 2025-10-17: Integrates with YouTube Data API to retrieve and analyze video content, transcripts, channel statistics, and engagement metrics across different regions and categories without leaving the conversation interface.  (9 tools) (node)
* [✅ puremd-mcp](https://github.com/puremd/puremd-mcp) ⭐ 60 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-01: Enables AI access to web content in clean markdown format through unblock-url extraction and search-web capabilities, bypassing anti-bot measures for reliable information retrieval.  (2 tools) (node)
* [✅ mcp-searxng-public](https://github.com/pwilkin/mcp-searxng-public) ⭐ 54 | 🐛 0 | 🌐 JavaScript | 📅 2026-05-23: Queries public SearXNG instances to extract structured search results with time-range filtering and fallback mechanisms for reliable web search capabilities  (1 tools) (node)
* [✅ mcp-google-cse](https://github.com/richard-weiss/mcp-google-cse) ⭐ 45 | 🐛 0 | 🌐 Python | 📅 2026-04-09: Integrates with Google Custom Search Engine API to enable web searches for fact-checking, research, and content generation with current data.  (1 tools) (python)
* [✅ @oevortex/ddg\_search](https://github.com/oevortex/ddg_search) ⭐ 42 | 🐛 0 | 🌐 JavaScript | 📅 2026-07-06: Provides web search capabilities through DuckDuckGo, enabling content retrieval, URL processing, and metadata extraction with customizable filtering options  (4 tools) (node)
* [✅ mcp-search-linkup](https://github.com/linkupplatform/python-mcp-server) ⚠️ Archived: Integrates with Linkup Technologies' API to enable web searches for information gathering, fact-checking, and research tasks.  (1 tools) (python)
* [✅ websearch-mcp](https://github.com/mnhlt/websearch-mcp) ⭐ 40 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-30: Provides real-time web search capabilities via a dedicated crawler service with configurable result limits, language filtering, and domain rules  (1 tools) (node)
* [✅ @kevinwatt/mcp-server-searxng](https://github.com/kevinwatt/mcp-server-searxng) ⭐ 36 | 🐛 0 | 🌐 TypeScript | 📅 2026-09-01: Integrates SearXNG's meta-search capabilities to provide privacy-focused, customizable search results from multiple engines for diverse information retrieval scenarios.  (1 tools) (node)
* [✅ @sinco-lab/mcp-youtube-transcript](https://github.com/sinco-lab/mcp-youtube-transcript) ⭐ 36 | 🐛 0 | 🌐 TypeScript | 📅 2026-05-20: Extracts and formats YouTube video transcripts with language selection, paragraph formatting, and metadata enrichment for content analysis and research workflows.  (1 tools) (node)
* [✅ mcp-server-giphy](https://github.com/magarcia/mcp-server-giphy) ⭐ 33 | 🐛 2 | 🌐 TypeScript | 📅 2025-05-22: Allows AI models to search, retrieve, and utilize GIFs from Giphy.  (3 tools) (node)
* [✅ newsnow-mcp-server](https://github.com/ourongxing/newsnow-mcp-server) ⭐ 32 | 🐛 2 | 🌐 JavaScript | 📅 2026-06-03: Provides a bridge to the NewsNow platform for retrieving trending and latest news from various sources with customizable result limits and markdown-formatted output.  (1 tools) (node)
* [✅ mcp-server-dumplingai](https://github.com/dumpling-ai/mcp-server-dumplingai) ⭐ 31 | 🐛 7 | 🌐 JavaScript | 📅 2025-07-10: Provides a bridge to Dumpling AI's data extraction API for performing web searches, scraping content, extracting structured data, and processing various document formats through 20+ specialized tools.  (27 tools) (node)
* [✅ vectara-mcp](https://github.com/vectara/vectara-mcp) ⭐ 29 | 🐛 1 | 🌐 Python | 📅 2026-07-23: Provides a bridge between conversational interfaces and Vectara's Retrieval-Augmented Generation capabilities, enabling powerful search queries that return both relevant results and generated responses with customizable parameters.  (2 tools) (python)
* [✅ jina-ai-mcp-server](https://github.com/joebuildsstuff/mcp-jina-ai) ⭐ 28 | 🐛 1 | 🌐 JavaScript | 📅 2025-05-15: Integrates with Jina AI's web services to enable web content extraction, search, and fact-checking through natural language interactions.  (3 tools) (node)
* [✅ @mcp-server/google-search-mcp](https://github.com/modelcontextprotocol-servers/google-search-mcp) ⭐ 23 | 🐛 3 | 🌐 TypeScript | 📅 2025-03-09: Model Context Protocol server for google search. A Playwright-based Model Context Protocol (MCP) tool that bypasses search engine anti-bot mechanisms, performs Google searches, and extracts results, providing real-time search capabilities for AI assistants like Claude and Cursor.  (1 tools) (node)
* [✅ @mcp-server/whois-mcp](https://github.com/modelcontextprotocol-servers/whois-mcp) ⭐ 21 | 🐛 1 | 🌐 JavaScript | 📅 2025-03-07: Provides a lightweight server for performing WHOIS lookups using the whoiser library, enabling retrieval of domain registration and ownership information through TypeScript-based type-safe queries.  (4 tools) (node)
* [✅ @melaodoidao/datagov-mcp-server](https://github.com/melaodoidao/datagov-mcp-server) ⭐ 19 | 🐛 2 | 🌐 JavaScript | 📅 2025-04-09: Integrates with Data.gov to enable searching, retrieving, and accessing U.S. government datasets for data-driven analysis and research projects.  (4 tools) (node)
* [✅ @mjpitz/mcp-rfc](https://github.com/mjpitz/mcp-rfc) ⭐ 19 | 🐛 0 | 🌐 TypeScript | 📅 2025-04-11: Provides a bridge to IETF RFC documents for retrieving, searching, and extracting specific sections from technical standards documentation with support for both HTML and TXT formats  (3 tools) (node)
* [✅ @deventerprisesoftware/scrapi-mcp](https://github.com/deventerprisesoftware/scrapi-mcp) ⭐ 18 | 🐛 2 | 🌐 JavaScript | 📅 2026-08-06: Enables web scraping from sites with bot detection, captchas, or geolocation restrictions through residential proxies and automated captcha solving for content extraction in HTML or Markdown formats.  (2 tools) (node)
* [✅ open-docs-mcp](https://github.com/askme765cs/open-docs-mcp) ⭐ 18 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-08: Crawls and indexes technical documentation sites with multi-language support using jieba-wasm and Lunr.js for full-text search capabilities  (7 tools) (node)
* [✅ server-moz-readability](https://github.com/emzimmer/server-moz-readability) ⭐ 17 | 🐛 3 | 🌐 Dockerfile | 📅 2025-01-28: Integrates Mozilla's Readability algorithm to extract and transform webpage content into clean, LLM-optimized Markdown.  (1 tools) (node)
* [✅ ddg-mcp](https://github.com/misanthropic-ai/ddg-mcp) ⭐ 16 | 🐛 6 | 🌐 Python | 📅 2025-03-13: Integrates with DuckDuckGo to provide web, image, news, and video search capabilities with configurable parameters for region, safesearch, and time limits.  (5 tools) (python)
* [✅ npm-search-mcp-server](https://github.com/btwiuse/npm-search-mcp-server) ⭐ 16 | 🐛 3 | 🌐 JavaScript | 📅 2026-02-20: Enables npm package searches via CLI, facilitating JavaScript library discovery and dependency management  (1 tools) (node)
* [✅ opengov-mcp-server](https://github.com/srobbin/opengov-mcp-server) ⭐ 14 | 🐛 1 | 🌐 TypeScript | 📅 2025-04-09: Enables access to public government datasets from Socrata-powered portals through a unified tool for searching, querying, and analyzing data like budgets, crime statistics, and transportation information without requiring an API key.  (1 tools) (node)
* [✅ @scrapezy/mcp](https://github.com/scrapezy/mcp) ⭐ 13 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-11: Integrates with the Scrapezy API to extract structured data from websites based on user-specified prompts, enabling flexible web scraping for data collection, content aggregation, and automated research tasks.  (1 tools) (node)
* [✅ mcp-copy-web-ui](https://github.com/maoxiaoke/mcp-copy-web-ui) ⭐ 13 | 🐛 1 | 🌐 HTML | 📅 2025-04-14: Transforms webpage content into a fully inlined, script-free HTML document with base64-encoded resources, enabling comprehensive web page analysis and extraction.  (1 tools) (node)
* [✅ mcp-pagespeed-server](https://github.com/phialsbasement/pagespeed-mcp-server) ⭐ 13 | 🐛 2 | 🌐 JavaScript | 📅 2025-09-13: Integrates Google's PageSpeed Insights API to provide detailed web performance metrics and optimization recommendations for websites.  (1 tools) (node)
* [✅ @mcptools/mcp-tavily](https://github.com/kshern/mcp-tavily) ⭐ 12 | 🐛 0 | 🌐 JavaScript | 📅 2026-06-09: Integrates with the Tavily API to enable advanced search and content extraction operations, facilitating web research and up-to-date information access for AI applications.  (4 tools) (node)
* [✅ youtube-dlp-server](https://github.com/agentx-ai/agentx-mcp-servers/tree/HEAD/youtube_dlp_server) ⭐ 12 | 🐛 1 | 📅 2025-07-22: Extracts YouTube video metadata, subtitles in multiple languages, and top comments using yt-dlp with proxy support for content analysis and accessibility workflows.  (3 tools) (python)
* [✅ @unifuncs/ufn-mcp-server](https://github.com/unifuncs/ufn-mcp-server) ⭐ 11 | 🐛 1 | 🌐 JavaScript | 📅 2026-07-02: Provides a bridge to the UniFuncs API for web search and web reading capabilities through TypeScript implementation with Express and NPX commands.  (2 tools) (node)
* [✅ google-pse-mcp](https://github.com/rendyfebry/google-pse-mcp) ⭐ 11 | 🐛 3 | 🌐 JavaScript | 📅 2025-09-28: Integrates with Google Programmable Search Engine to enable web search capabilities with support for pagination, result size, language restrictions, and safe search filtering  (1 tools) (node)
* [✅ mcp-server-pacman](https://github.com/oborchers/mcp-server-pacman) ⭐ 11 | 🐛 2 | 🌐 Python | 📅 2025-04-29: Integrates with package repositories including PyPI, npm, crates.io, Docker Hub, and Terraform Registry to search and retrieve detailed information about packages, versions, dependencies, and Docker images.  (5 tools) (python)
* [✅ searxng-simple-mcp](https://github.com/sacode/searxng-simple-mcp) ⭐ 11 | 🐛 1 | 🌐 Python | 📅 2025-08-25: Integrates with SearxNG privacy-focused search engine to provide web search capabilities with customizable parameters like result count, language, and time range.  (1 tools) (python)
* [✅ @kazuph/mcp-pocket](https://github.com/kazuph/mcp-pocket) ⭐ 9 | 🐛 0 | 🌐 JavaScript | 📅 2025-01-01: Integrates with the Pocket API to enable natural language-based retrieval and management of saved articles.  (2 tools) (node)
* [✅ better-fetch-mcp](https://github.com/flutterninja9/better-fetch) ⭐ 9 | 🐛 0 | 🌐 JavaScript | 📅 2026-03-05: Fetches and processes web content with nested URL crawling capabilities. Transform any documentation site or web resource into clean, structured markdown files perfect for AI consumption and analysis.  (2 tools) (node)
* [✅ mcp-data-extractor](https://github.com/sammcj/mcp-data-extractor) ⚠️ Archived: Extracts data from TypeScript/JavaScript code into JSON configuration files, facilitating code refactoring and improved maintainability.  (2 tools) (node)
* [✅ mcp-private-github-search](https://github.com/hint-services/obsidian-github-mcp) ⭐ 9 | 🐛 2 | 🌐 TypeScript | 📅 2025-11-28: Provides authenticated access to a specific private GitHub repository for file content retrieval, code search, issue search, and detailed commit history with file diffs.  (4 tools) (node)
* [✅ mcp-perplexity-search](https://github.com/spences10/mcp-perplexity-search) ⚠️ Archived: Integrates with Perplexity's web search API to enable real-time fact-checking, research, and content generation using up-to-date information.  (1 tools) (node)
* [✅ scraperis-mcp](https://github.com/ai-quill/scraperis-mcp) ⭐ 8 | 🐛 3 | 🌐 TypeScript | 📅 2026-06-02: Integrates with Scraper.is API to enable web content extraction, structured data parsing, and Markdown conversion for tasks like product research, news aggregation, and content analysis.  (1 tools) (node)
* [✅ mcp-tavily-search](https://github.com/spences10/mcp-tavily-search) ⚠️ Archived: Integrates with Tavily's semantic search API to enable web searches and retrieval of relevant results for fact-checking and research tasks.  (3 tools) (node)
* [✅ rapidocr-mcp](https://github.com/z4none/rapidocr-mcp) ⭐ 6 | 🐛 0 | 🌐 Python | 📅 2025-09-13: Extracts text from images using RapidOCR library through base64-encoded data or file paths for automated document processing workflows.  (2 tools) (python)
* [✅ @ashdev/discourse-mcp-server](https://github.com/ashdevfr/discourse-mcp-server) ⭐ 5 | 🐛 4 | 🌐 JavaScript | 📅 2025-03-10: Enables searching and retrieving content from Discourse forums through a single tool that queries posts using the discourse2 npm package.  (1 tools) (node)
* [✅ @tongxiao/common-search-mcp-server](https://github.com/aliyun/alibabacloud-iqs-tongxiao-mcp-server) ⭐ 5 | 🐛 3 | 🌐 JavaScript | 📅 2026-01-07: Integrates with TongXiao's IQS APIs to provide enhanced real-time web search capabilities that deliver clean, accurate, and diverse results through multiple data sources optimized with large language models.  (1 tools) (node)
* [✅ rss-reader-mcp](https://github.com/kwp-lab/rss-reader-mcp) ⭐ 5 | 🐛 0 | 🌐 JavaScript | 📅 2025-09-10: An MCP (Model Context Protocol) server for RSS feed aggregation and article content extraction. You can use it to subscribe to RSS feeds and fetch article lists, or extract the full content of an article from a URL and format it as Markdown.  (2 tools) (node)
* [✅ @grounddocs/grounddocs](https://github.com/grounddocs/grounddocs) ⭐ 4 | 🐛 1 | 🌐 TypeScript | 📅 2025-05-25: Provides source-verified documentation lookup for Python libraries and Kubernetes resources, retrieving accurate, version-specific information from authoritative sources rather than potentially hallucinated content.  (2 tools) (node)
* [✅ @kazuph/mcp-youtube](https://github.com/kazuph/mcp-youtube) ⭐ 4 | 🐛 0 | 🌐 TypeScript | 📅 2025-09-25: Integrates YouTube subtitle retrieval for natural language queries about video content.  (1 tools) (node)
* [✅ hermes-search-mcp](https://github.com/cognitive-stack/hermes-search-mcp) ⭐ 4 | 🐛 2 | 🌐 TypeScript | 📅 2025-04-19: Provides a bridge to Azure Cognitive Search for executing search queries, indexing documents, and managing search indexes with filtering options  (3 tools) (node)
* [✅ mcp-server-fetch-typescript](https://github.com/tatn/mcp-server-fetch-typescript) ⭐ 4 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-13: Integrates with web content sources to fetch, convert, and summarize online information for real-time data retrieval and analysis.  (4 tools) (node)
* [✅ pickapicon-mcp](https://github.com/leee62/pickapicon-mcp) ⭐ 4 | 🐛 1 | 🌐 JavaScript | 📅 2025-05-26: Provides a bridge to the Iconify API for searching and retrieving SVG icons from various collections, enabling quick integration of icons into projects.  (3 tools) (node)
* [✅ @humansean/mcp-bocha](https://github.com/intounknown/mcp-bocha) ⭐ 3 | 🐛 0 | 🌐 JavaScript | 📅 2025-05-22: Enables AI to perform web searches with customizable parameters including freshness filters, domain controls, and result summarization for retrieving up-to-date information from the internet.  (1 tools) (node)
* [✅ hackernews-mcp](https://github.com/georgenance/hackernews-mcp) ⭐ 3 | 🐛 0 | 🌐 JavaScript | 📅 2025-07-08: Integrates with Hacker News API to fetch top stories, retrieve detailed story information with markdown content extraction, get popular comments with filtering options, and search recent stories by keywords within specified time ranges.  (1 tools) (node)
* [✅ linkd-mcp](https://github.com/automcp-app/linkd-mcp) ⭐ 3 | 🐛 5 | 🌐 TypeScript | 📅 2025-05-28: Integrates with Linkd API to extract LinkedIn user and company profiles, search contacts, retrieve email addresses, and perform deep research workflows for sales prospecting and recruitment.  (7 tools) (node)
* [✅ mcp-jinaai-search](https://github.com/spences10/mcp-jinaai-search) ⚠️ Archived: Integrates JinaAI's search capabilities for web content discovery, information retrieval, and data extraction  (1 tools) (node)
* [✅ news-mcp-server](https://github.com/anurag-dhamala/news-mcp-server) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2025-04-11: Fetches the latest news and sources based on country and language using newsdata.io.  (3 tools) (node)
* [✅ @pinkpixel/prysm-mcp](https://github.com/pinkpixel-dev/prysm-mcp-server) ⭐ 2 | 🐛 0 | 🌐 TypeScript | 📅 2025-06-02: Provides web scraping capabilities with three specialized tools (scrapeFocused, scrapeBalanced, scrapeDeep) for efficient content extraction, image processing, and pagination handling with customizable parameters.  (4 tools) (node)
* [✅ @search-intent/mcp](https://github.com/captainchaozi/search-intent-mcp) ⭐ 2 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-31: Detects search intent for SEO-related workflows.  (1 tools) (node)
* [✅ agora-mcp](https://github.com/fewsats/agora-mcp) ⭐ 2 | 🐛 3 | 🌐 Python | 📅 2025-05-23: Connects AI systems to SearchAgora, a universal product search engine, enabling natural language interactions for discovering, viewing, and purchasing products from thousands of online stores without leaving the conversation interface.  (6 tools) (python)
* [✅ duck-duck-mcp](https://github.com/qwang07/duck-duck-mcp) ⭐ 2 | 🐛 3 | 🌐 JavaScript | 📅 2025-02-07: Integrates with DuckDuckGo's search engine to enable web searches for up-to-date information retrieval and fact-checking.  (1 tools) (node)
* [✅ orion-vision-mcp](https://github.com/cognitive-stack/orion-vision-mcp) ⭐ 2 | 🐛 0 | 🌐 TypeScript | 📅 2025-04-19: Integrates with Azure Form Recognizer to extract structured data from documents including receipts, invoices, ID documents, and business cards for automated document processing workflows.  (2 tools) (node)
* [✅ qanon\_mcp](https://github.com/jkingsman/qanon-mcp-server) ⭐ 2 | 🐛 0 | 🌐 Python | 📅 2025-04-05: Enables access to QAnon drops for sociological research.  (8 tools) (python)
* [✅ @jayarrowz/mcp-xpath](https://github.com/thirdstrandstudio/mcp-xpath) ⭐ 1 | 🐛 1 | 🌐 JavaScript | 📅 2025-08-04: Enables Claude to execute XPath queries on XML and HTML content, supporting both direct parsing and web scraping through Puppeteer for structured data extraction from documents and websites.  (2 tools) (node)
* [✅ @sean.lee/server-youtube-transcription](https://github.com/seanlee10/server-youtube-transcription) ⭐ 0 | 🐛 2 | 📅 2025-03-25: Extracts transcriptions from YouTube videos by accepting a video URL and returning the full transcript text using the youtube-transcript library  (1 tools) (node)
* [✅ jfk-mcp](https://github.com/westernconcrete/jfk-mcp) ⭐ 0 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-23: Provides access to declassified JFK assassination documents through search tools for text, vector, metadata filtering, and document retrieval of government files released between 2017-2025.  (5 tools) (node)
* [✅ mcp-apifox](https://github.com/sujianqingfeng/mcp-apifox) ⭐ 0 | 🐛 1 | 🌐 TypeScript | 📅 2025-03-14: Integrates with Apifox to provide access to API documentation, enabling developers to extract project details and fetch API information using authentication tokens via stdio transport.  (2 tools) (node)
* [✅ uniquity-mcp](https://github.com/kunihiros/uniquity-mcp) ⚠️ Archived: Analyzes GitHub repositories for code similarities, returning detailed reports in Markdown format  (1 tools) (node)
* [✅ @mcpfinder/server](https://github.com/mcpfinder/server): Enables AI assistants to discover and integrate new capabilities by providing tools for searching, retrieving details, and configuring MCP servers across various applications.  (5 tools) (node)
* [✅ fetchserp-mcp-server](https://github.com/fetchserp/fetchserp-mcp-server-node): Integrates with FetchSERP API to provide SEO analysis, SERP data retrieval, web scraping, keyword research, backlink analysis, and domain intelligence across Google, Bing, Yahoo, and DuckDuckGo search engines.  (23 tools) (node)

</details>

<a id="security"></a>

<details>
<summary><strong>Security</strong></summary>

Enhance security with tools for scanning, threat detection, and secure access.

* [✅ @clerk/agent-toolkit](https://github.com/clerk/javascript/tree/HEAD/packages/agent-toolkit) ⭐ 1,749 | 🐛 142 | 🌐 TypeScript | 📅 2026-09-01: Manage Clerk's authentication and user management organization management, session handling, and authorization features.  (19 tools) (node)
* [✅ @burtthecoder/mcp-shodan](https://github.com/burtthecoder/mcp-shodan) ⭐ 161 | 🐛 5 | 🌐 TypeScript | 📅 2026-03-31: Access Shodan API and CVEDB to query IoT device data and vulnerability information.  (7 tools) (node)
* [✅ @burtthecoder/mcp-virustotal](https://github.com/burtthecoder/mcp-virustotal) ⭐ 149 | 🐛 6 | 🌐 TypeScript | 📅 2026-05-24: This VirusTotal MCP server enables AI assistants to programmatically access VirusTotal's threat intelligence for security analysis and threat detection.  (7 tools) (node)
* [✅ mcp-security-audit](https://github.com/qianniuspace/mcp-security-audit) ⭐ 57 | 🐛 1 | 🌐 TypeScript | 📅 2025-07-18: Integrates with npm-audit-report and npm-registry-fetch to analyze and report potential vulnerabilities in Node.js project dependencies, offering actionable security insights for development teams.  (1 tools) (node)
* [✅ @burtthecoder/mcp-dnstwist](https://github.com/burtthecoder/mcp-dnstwist) ⭐ 51 | 🐛 7 | 🌐 JavaScript | 📅 2025-03-03: Integrates with dnstwist to automate DNS fuzzing for detecting typosquatting, phishing, and corporate espionage threats.  (1 tools) (node)
* [✅ mcp-nmap-server](https://github.com/phialsbasement/nmap-mcp-server) ⚠️ Archived: Integrates NMAP to enable network scanning, security assessments, and automated penetration testing on Windows systems.  (1 tools) (node)
* [✅ keycloak-model-context-protocol](https://github.com/christophenglisch/keycloak-model-context-protocol) ⭐ 46 | 🐛 3 | 🌐 TypeScript | 📅 2025-02-09: Integrates with Keycloak Admin to provide streamlined user and realm management operations for identity and access control automation.  (4 tools) (node)
* [✅ keycloak-mcp](https://github.com/haithamoumerzoug/keycloak-mcp) ⭐ 13 | 🐛 2 | 🌐 TypeScript | 📅 2025-12-01: Integrates with Keycloak identity management to enable user creation, role assignment, group management, and client listing across different realms  (9 tools) (node)
* [✅ @binalyze/air-mcp](https://github.com/binalyze/air-mcp) ⭐ 7 | 🐛 0 | 🌐 TypeScript | 📅 2025-12-02: Bridges to the Binalyze AIR digital forensics platform, enabling security teams to query endpoint data, monitor status, and manage investigations through a secure API connection.  (116 tools) (node)
* [✅ proofly-mcp](https://github.com/prooflie/mcp) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2025-05-14: Enables deepfake detection in images through Proofly API integration, providing detailed analysis results including real/fake probability scores and individual model results for each detected face.  (4 tools) (node)
* [✅ @mtane0412/perspective-mcp-server](https://github.com/mtane0412/perspective-mcp-server) ⭐ 0 | 🐛 0 | 🌐 JavaScript | 📅 2025-02-07: Integrates with Perspective API to analyze text toxicity and provide content moderation across multiple languages for enhanced online safety.  (2 tools) (node)
* [✅ @usegrant/mcp](https://github.com/usegranthq/mcp-server) ⭐ 0 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-24: Integrates with the UseGrant API to enable secure authentication, resource management, and structured access control for provider, client, and tenant management within the UseGrant ecosystem.  (26 tools) (node)
* [✅ @vj-presidio/specif-ai-mcp-server](https://github.com/vj-presidio/specif-ai-mcp-server-archive): Provides a bridge for interacting with specif-ai services over stdio, focusing on secure API key management, error handling, and flexible deployment options.  (9 tools) (node)

</details>

<a id="sports"></a>

<details>
<summary><strong>Sports</strong></summary>

Access sports data, results, and stats with ease.

* [✅ fpl-mcp](https://github.com/rishijatia/fantasy-pl-mcp) ⭐ 78 | 🐛 3 | 🌐 Python | 📅 2026-08-03: Integrates with Fantasy Premier League data to provide player statistics, team information, and analytical tools for making informed fantasy football management decisions.  (16 tools) (python)

</details>

<a id="support-service-management"></a>

<details>
<summary><strong>Support & Service Management</strong></summary>

Manage customer support and IT services with specialized tools.

* [✅ @makeplane/plane-mcp-server](https://github.com/makeplane/plane-mcp-server) ⭐ 305 | 🐛 29 | 🌐 Python | 📅 2026-09-01: Integrates with Plane's project management APIs to enable creation and management of projects, issues, cycles, modules, and work logs through over 30 specialized tools for automating workflow tasks.  (46 tools) (node)
* [✅ @tacticlaunch/mcp-linear](https://github.com/tacticlaunch/mcp-linear) ⭐ 146 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-29: Bridges Linear project management system with natural language interaction, enabling issue tracking, project workflows, and team management without context switching.  (32 tools) (node)
* [✅ @shortcut/mcp](https://github.com/useshortcut/mcp-server-shortcut) ⚠️ Archived: Integrates with Shortcut's project management platform to provide story management, epic and iteration operations, team workflows, objective tracking, and comprehensive search capabilities for development teams automating sprint planning and task management.  (32 tools) (node)
* [✅ clickup-mcp-server](https://github.com/nsxdavid/clickup-mcp-server) ⭐ 42 | 🐛 5 | 🌐 TypeScript | 📅 2025-04-14: Integrates with ClickUp API to enable workspace, task, document, and checklist management directly from conversations through Node.js-based tools and URI template resources.  (42 tools) (node)
* [✅ mcp-server-monday](https://github.com/sakce/mcp-server-monday) ⭐ 34 | 🐛 2 | 🌐 Python | 📅 2025-09-10: Integrates with Monday.com to enable creating items, retrieving board groups, adding comments, listing boards, and managing sub-items for project management and team collaboration workflows.  (21 tools) (python)
* [✅ @makingchatbots/genesys-cloud-mcp-server](https://github.com/makingchatbots/genesys-cloud-mcp-server) ⭐ 31 | 🐛 3 | 🌐 TypeScript | 📅 2026-04-06: Provides a bridge between contact center analytics and routing data in Genesys Cloud, enabling conversational business intelligence through queue searches, conversation volume queries, call sampling, and voice quality metrics analysis.  (8 tools) (node)
* [✅ advocu-mcp-server](https://github.com/carlosazaustre/advocu-mcp-server) ⭐ 31 | 🐛 11 | 🌐 TypeScript | 📅 2026-01-20: Integrates with the Advocu platform to streamline activity reporting for Google Developer Experts, enabling submission of content creation, speaking engagements, workshops, mentoring sessions, and community interactions with detailed metrics tracking.  (7 tools) (node)
* [✅ jira-mcp](https://github.com/camdenclark/jira-mcp) ⭐ 28 | 🐛 3 | 🌐 JavaScript | 📅 2025-02-27: Integrates Jira for natural language querying and manipulation of project management data.  (2 tools) (node)
* [✅ @mcp-devtools/jira](https://github.com/dxheroes/mcp-devtools) ⚠️ Archived: Integrates with Jira and Linear issue tracking systems to enable retrieval and interaction with tickets directly within conversations, eliminating context switching when accessing project management data.  (15 tools) (node)
* [✅ mcp-linear](https://github.com/shannonlal/mcp-linear) ⭐ 6 | 🐛 6 | 🌐 TypeScript | 📅 2025-03-31: Integrates Linear project management with MCP to enable task creation, updates, and queries for automated workflow management.  (5 tools) (node)
* [✅ @parassolanki/jira-mcp-server](https://github.com/parassolanki/jira-mcp-server) ⭐ 3 | 🐛 0 | 🌐 TypeScript | 📅 2025-04-09: Integrates with Jira API to enable issue tracking, sprint planning, and project management tasks directly within conversation interfaces using personal access tokens for authentication.  (5 tools) (node)
* [✅ devrev-mcp](https://github.com/kpsunil97/devrev-mcp-server) ⭐ 3 | 🐛 3 | 🌐 Python | 📅 2024-12-20: Integrates with DevRev APIs to enable search and retrieval of DevRev data for automated issue tracking, customer support analysis, and data-driven decision making.  (16 tools) (python)
* [✅ hh-jira-mcp-server](https://github.com/alexeydubinin/hh-jira-mcp-server) ⭐ 3 | 🐛 2 | 🌐 Python | 📅 2024-12-02: Integrates with the Jira API to enable querying, creating, and modifying issues and projects, simplifying project tracking and management tasks.  (3 tools) (python)
* [✅ @cristip73/mcp-server-asana](https://github.com/cristip73/mcp-server-asana) ⭐ 2 | 🐛 2 | 🌐 TypeScript | 📅 2025-06-14: Integrates with Asana's API to enable task management, project organization, and collaboration workflows through 30+ tools for searching, creating, and visualizing projects and tasks.  (41 tools) (node)
* [✅ @keegancsmith/linear-issues-mcp-server](https://github.com/keegancsmith/linear-issues-mcp-server) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-23: Integrates with Linear issue tracking to provide read-only access to issue details and comments without switching contexts.  (2 tools) (node)
* [✅ mcp-server-linearapp](https://github.com/magarcia/mcp-server-linearapp) ⭐ 1 | 🐛 4 | 🌐 TypeScript | 📅 2025-03-24: Integrates with Linear's issue tracking system to enable natural language interaction for creating, updating, and managing issues, comments, attachments, and project information without context switching.  (19 tools) (node)

</details>

<a id="translation-services"></a>

<details>
<summary><strong>Translation Services</strong></summary>

Translate content between languages with AI-powered tools.

* [✅ deepl-mcp-server](https://github.com/deeplcom/deepl-mcp-server) ⭐ 112 | 🐛 3 | 🌐 JavaScript | 📅 2026-09-01: Integrates with DeepL to provide high-quality text translation and rephrasing between numerous languages with formality controls for supported language pairs.  (5 tools) (node)
* [✅ @translated/lara-mcp](https://github.com/translated/lara-mcp) ⭐ 97 | 🐛 1 | 🌐 TypeScript | 📅 2026-06-11: Bridges to the Lara Translation API for accurate, context-aware text translations between languages with automatic language detection capabilities.  (10 tools) (node)
* [✅ @winterjung/mcp-korean-spell](https://github.com/winterjung/mcp-korean-spell) ⭐ 24 | 🐛 2 | 🌐 TypeScript | 📅 2025-04-15: Integrates with Naver's spelling service to automatically correct grammatical errors, typos, and linguistic issues in Korean text.  (1 tools) (node)

</details>

<a id="travel-transportation"></a>

<details>
<summary><strong>Travel & Transportation</strong></summary>

Get travel schedules, routes, and real-time transportation data.

* [✅ 12306-mcp](https://github.com/Joooook/12306-mcp) ⭐ 1,227 | 🐛 6 | 🌐 JavaScript | 📅 2026-07-31: A 12306 ticket search server based on the Model Context Protocol (MCP). The server provides a simple API interface that allows users to search for 12306 tickets.  (8 tools) (node)
* [✅ 12306-mcp](https://github.com/Joooook/12306-mcp) ⭐ 1,227 | 🐛 6 | 🌐 JavaScript | 📅 2026-07-31: A 12306 ticket search server based on the Model Context Protocol (MCP). The server provides a simple API interface that allows users to search for 12306 tickets.  (8 tools) (node)
* [✅ @openbnb/mcp-server-airbnb](https://github.com/openbnb-org/mcp-server-airbnb) ⭐ 520 | 🐛 35 | 🌐 JavaScript | 📅 2026-08-06: Integrates with Airbnb to enable vacation rental search and detailed property information retrieval without requiring API keys  (2 tools) (node)
* [✅ @gongrzhe/server-travelplanner-mcp](https://github.com/gongrzhe/travel-planner-mcp-server) ⚠️ Archived: Integrates with Google Maps to enable AI-driven travel planning, itinerary optimization, and location-based services for automated trip management.  (5 tools) (node)
* [✅ @variflight-ai/variflight-mcp](https://github.com/variflight/variflight-mcp) ⭐ 31 | 🐛 3 | 🌐 JavaScript | 📅 2026-04-20: Integrates with Variflight API to provide real-time flight information, schedules, aircraft tracking, airport weather forecasts, and comfort metrics for travel planning and aviation monitoring applications.  (8 tools) (node)
* [✅ @mfukushim/map-traveler-mcp](https://github.com/mfukushim/map-traveler-mcp) ⭐ 23 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-03: Integrates with Google Maps to create virtual travel experiences where users can navigate real-world routes with customizable avatars, discover nearby facilities, and share journeys on Bluesky.  (8 tools) (node)
* [✅ caltrain-mcp](https://github.com/davidyen1124/caltrain-mcp) ⭐ 10 | 🐛 1 | 🌐 Python | 📅 2026-08-31: Provides real-time Caltrain commuter rail information including schedules, station details, and trip planning for the San Francisco Bay Area  (2 tools) (python)
* [✅ train\_12306](https://github.com/ecmadao/Train-12306) ⭐ 7 | 🐛 1 | 🌐 Python | 📅 2022-07-12: A command line tool to show 12306 tickets  (4 tools) (python)
* [✅ lumbretravel-mcp](https://github.com/lumile/lumbretravel-mcp) ⭐ 1 | 🐛 2 | 🌐 TypeScript | 📅 2025-03-28: Integrates with the Argentinian LumbreTravel API to manage travel programs, activities, and bookings for efficient itinerary planning and service coordination.  (68 tools) (node)

</details>

<a id="version-control"></a>

<details>
<summary><strong>Version Control</strong></summary>

Manage Git repositories, pull requests, and issues with version control tools.

* [✅ @modelcontextprotocol/server-github](https://github.com/modelcontextprotocol/servers-archived/tree/HEAD/src/github) ⚠️ Archived: Manage repositories, issues, and search code via GitHub API.  (26 tools) (node)
* [✅ mcp-server-git](https://github.com/modelcontextprotocol/servers-archived/tree/HEAD/src/git) ⚠️ Archived: Interact with local Git repositories for version control tasks.  (13 tools) (python)
* [✅ @yoda.digital/gitlab-mcp-server](https://github.com/yoda-digital/mcp-gitlab-server) ⭐ 64 | 🐛 27 | 🌐 TypeScript | 📅 2026-08-12: GitLab MCP Server provides robust integration with the GitLab API, enabling developers to interact with repositories, issues, merge requests, and project events through natural language commands via stdio and Server-Sent Events (SSE) transports.  (30 tools) (node)
* [✅ github-repo-mcp](https://github.com/ryan0204/github-repo-mcp) ⭐ 29 | 🐛 1 | 🌐 JavaScript | 📅 2025-04-25: Provides a bridge between AI and GitHub repositories, enabling access to repository contents for code analysis and reference during conversations.  (3 tools) (node)
* [✅ @nexus2520/bitbucket-mcp-server](https://github.com/pdogra1299/bitbucket-mcp-server) ⭐ 27 | 🐛 13 | 🌐 TypeScript | 📅 2026-07-09: Integrates with Bitbucket Cloud and Server APIs to manage pull request workflows including creation, updates, merging, branch management, code review operations, and diff retrieval with configurable context lines.  (16 tools) (node)
* [✅ mcp-github-issue](https://github.com/sammcj/mcp-github-issue) ⚠️ Archived: Fetches GitHub issues and transforms them into structured task descriptions for software development workflows and project management.  (1 tools) (node)
* [✅ @missionsquad/mcp-github](https://github.com/missionsquad/mcp-github) ⭐ 10 | 🐛 2 | 🌐 TypeScript | 📅 2026-05-07: Integrates with GitHub's API to enable repository operations, file management, issue tracking, and code search directly within conversations through flexible authentication and over 30 specialized tools.  (45 tools) (node)
* [✅ @sunwood-ai-labs/github-kanban-mcp-server](https://github.com/sunwood-ai-labs/github-kanban-mcp-server) ⭐ 10 | 🐛 6 | 🌐 TypeScript | 📅 2026-06-22: Integrates with GitHub's API to enable Kanban-style project management and issue tracking for streamlined software development workflows.  (4 tools) (node)
* [✅ mcp-server-diff-python](https://github.com/tatn/mcp-server-diff-python) ⭐ 9 | 🐛 1 | 🌐 Python | 📅 2025-01-21: Integrates with Python's difflib to generate unified diffs for efficient text comparison and version control tasks.  (1 tools) (python)
* [✅ mcp-git-commit-aider](https://github.com/mrorz/mcp-git-commit-aider) ⭐ 8 | 🐛 2 | 🌐 JavaScript | 📅 2025-05-13: Enables AI to make Git commits on behalf of users, automatically tracking AI contributions by appending '(aider)' to committer names for codebase statistics and analysis.  (1 tools) (node)
* [✅ gitee-mcp-server](https://github.com/normal-coder/gitee-mcp-server) ⭐ 7 | 🐛 2 | 🌐 TypeScript | 📅 2025-03-17: Integrates with Gitee repositories to enable repository creation, code management, issue tracking, and pull request workflows using a simple token-based authentication system.  (20 tools) (node)
* [✅ @kazuph/mcp-github-pera1](https://github.com/kazuph/mcp-github-pera1) ⚠️ Archived: Connects to GitHub repositories, enabling natural language queries about code structure, dependencies, and development history.  (1 tools) (node)
* [✅ git-mcp](https://github.com/kjozsa/git-mcp) ⭐ 3 | 🐛 1 | 🌐 Python | 📅 2025-03-05: Provides Git operations for local repositories, enabling repository management, tag handling, and repository refreshing without direct shell access.  (6 tools) (python)
* [✅ mcp-server-diff-typescript](https://github.com/tatn/mcp-server-diff-typescript) ⭐ 3 | 🐛 1 | 🌐 JavaScript | 📅 2025-01-21: Generates unified diffs between text strings with 3 lines of context, enabling precise comparison and analysis for version control and code review tasks.  (1 tools) (node)
* [✅ git-commands-mcp](https://github.com/bsreeram08/git-commands-mcp) ⭐ 2 | 🐛 1 | 🌐 JavaScript | 📅 2025-05-02: Enables Git repository exploration and analysis through a Node.js server that executes commands for cloning repositories, browsing directories, reading files, comparing branches, and searching code patterns with structured JSON responses.  (27 tools) (node)
* [✅ test-mcp](https://github.com/sach999/git-spice-help-mcp) ⭐ 1 | 🐛 1 | 🌐 TypeScript | 📅 2025-04-10: Provides instant access to git-spice documentation within your IDE, enabling real-time searching and automatic assistance for developers working with git-spice commands and usage examples.  (1 tools) (node)

</details>

<a id="other-tools-and-integrations"></a>

<details>
<summary><strong>Other Tools and Integrations</strong></summary>

Miscellaneous tools and integrations that don’t fit into other categories.

* [✅ @modelcontextprotocol/server-everything](https://github.com/modelcontextprotocol/servers/tree/HEAD/src/everything) ⭐ 90,001 | 🐛 515 | 🌐 TypeScript | 📅 2026-08-31: Test protocol features and tools for client compatibility.  (8 tools) (node)
* [✅ @modelcontextprotocol/server-sequential-thinking](https://github.com/modelcontextprotocol/servers/tree/HEAD/src/sequentialthinking) ⭐ 90,001 | 🐛 515 | 🌐 TypeScript | 📅 2026-08-31: Implements a structured sequential thinking process for breaking down complex problems, iteratively refining solutions, and exploring multiple reasoning paths.  (1 tools) (node)
* [✅ mcp-server-time](https://github.com/modelcontextprotocol/servers/tree/HEAD/src/time) ⭐ 90,001 | 🐛 515 | 🌐 TypeScript | 📅 2026-08-31: MCP server providing time and timezone conversion tools for AI assistants to handle localized time data and calculations.  (2 tools) (python)
* [✅ awslabs.aws-documentation-mcp-server](https://github.com/awslabs/mcp/tree/HEAD/src/aws-documentation-mcp-server) ⭐ 9,652 | 🐛 247 | 🌐 Python | 📅 2026-09-01: Provides tools to access AWS documentation, search for content, and get recommendations.  (3 tools) (python)
* [✅ diff-mcp](https://github.com/benjamine/jsondiffpatch/tree/HEAD/packages/diff-mcp) ⭐ 5,339 | 🐛 55 | 🌐 TypeScript | 📅 2026-05-14: Compares and patches JSON objects with a compact delta format that captures additions, modifications, deletions, and array moves for efficient data synchronization and change visualization.  (1 tools) (node)
* [✅ @notionhq/notion-mcp-server](https://github.com/makenotion/notion-mcp-server) ⭐ 4,618 | 🐛 187 | 🌐 TypeScript | 📅 2026-07-25: Bridges to the Notion API for searching content, querying databases, and managing pages and comments without requiring complex API interaction code  (19 tools) (node)
* [✅ tianji-mcp-server](https://github.com/msgbyte/tianji) ⭐ 3,085 | 🐛 140 | 🌐 TypeScript | 📅 2026-08-31: Bridges AI assistants with the Tianji platform to enable survey management, including querying results, retrieving detailed information, and listing workspace surveys without navigating the Tianji interface.  (8 tools) (node)
* [✅ @anaisbetts/mcp-installer](https://github.com/anaisbetts/mcp-installer) ⭐ 1,532 | 🐛 22 | 🌐 JavaScript | 📅 2024-11-26: Install and configure additional MCP servers dynamically.  (2 tools) (node)
* [✅ mcp-sequentialthinking-tools](https://github.com/spences10/mcp-sequentialthinking-tools) ⭐ 584 | 🐛 6 | 🌐 TypeScript | 📅 2026-09-01: Provides structured problem-solving tools for step-by-step analysis, branching thoughts, and adaptive reasoning strategies in complex decision-making processes.  (1 tools) (node)
* [✅ @abhiz123/todoist-mcp-server](https://github.com/abhiz123/todoist-mcp-server) ⭐ 392 | 🐛 18 | 🌐 JavaScript | 📅 2025-04-20: Create and manage tasks in Todoist through natural language.  (5 tools) (node)
* [✅ interactive-mcp](https://github.com/ttommyth/interactive-mcp) ⭐ 351 | 🐛 8 | 🌐 TypeScript | 📅 2025-11-20: Interactive terminal interface for enhancing AI interactions with user input capabilities, notifications, and cross-platform support for complex tasks requiring confirmation or clarification.  (5 tools) (node)
* [✅ imagesorcery-mcp](https://github.com/sunriseapps/imagesorcery-mcp) ⭐ 329 | 🐛 2 | 🌐 Python | 📅 2026-05-19: Provides powerful image manipulation capabilities including resizing, cropping, object detection, OCR text extraction, and finding objects based on text descriptions using Python with OpenCV and Ultralytics  (16 tools) (python)
* [✅ @jinzcdev/markmap-mcp-server](https://github.com/jinzcdev/markmap-mcp-server) ⭐ 281 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-04: Transforms Markdown documents into interactive mind maps with zooming, node expansion/collapse, and multi-format export capabilities for visualizing hierarchical information.  (1 tools) (node)
* [✅ @greirson/mcp-todoist](https://github.com/greirson/mcp-todoist) ⭐ 243 | 🐛 19 | 🌐 TypeScript | 📅 2026-05-01: Integrates with Todoist API to manage tasks, projects, sections, and comments with support for bulk operations, natural language search, and comprehensive CRUD functionality.  (28 tools) (node)
* [✅ penpot-mcp](https://github.com/montevive/penpot-mcp) ⭐ 237 | 🐛 15 | 🌐 Python | 📅 2025-11-01: Integrates with Penpot's API to enable project browsing, file retrieval, object searching, and visual component export with automatic screenshot generation for converting UI designs into functional code.  (10 tools) (python)
* [✅ @kazuph/mcp-taskmanager](https://github.com/kazuph/mcp-taskmanager) ⭐ 216 | 🐛 5 | 🌐 JavaScript | 📅 2026-06-12: Implements a structured task management system resulting in step-by-step workflows requiring explicit user approval.  (10 tools) (node)
* [✅ lighthouse-mcp](https://github.com/priyankark/lighthouse-mcp) ⭐ 202 | 🐛 7 | 🌐 JavaScript | 📅 2026-05-23: Use Google's lighthouse tool to measure perf metrics for your webpage. You can then run an agentic loop and get the assistants to optimize those metrics.  (2 tools) (node)
* [✅ @taskade/mcp-server](https://github.com/taskade/mcp) ⭐ 163 | 🐛 11 | 🌐 TypeScript | 📅 2026-08-25: Official Taskade MCP server with 50+ tools for managing workspaces, projects, tasks, custom AI agents, knowledge bases, templates, and workflow automations. Includes OpenAPI-to-MCP codegen utility.  (21 tools) (node)
* [✅ mcp-server-calculator](https://github.com/githejie/mcp-server-calculator) ⭐ 156 | 🐛 7 | 🌐 Python | 📅 2026-08-08: Provides a secure mathematical expression evaluation service using Python's AST module for basic operations without relying on eval(), enabling quick calculations within conversations.  (1 tools) (python)
* [✅ @tacticlaunch/mcp-linear](https://github.com/tacticlaunch/mcp-linear) ⭐ 146 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-29: Bridges Linear project management system with natural language interaction, enabling issue tracking, project workflows, and team management without context switching.  (32 tools) (node)
* [✅ @baruchiro/paperless-mcp](https://github.com/baruchiro/paperless-mcp) ⭐ 139 | 🐛 10 | 🌐 TypeScript | 📅 2026-08-21: Enables AI to interact with Paperless-NGX document management systems for organizing, searching, and managing document collections through natural language commands.  (23 tools) (node)
* [✅ consult-llm-mcp](https://github.com/raine/consult-llm-mcp) ⭐ 132 | 🐛 2 | 🌐 Rust | 📅 2026-08-30: Escalates complex reasoning tasks to more powerful language models (OpenAI o3, Google Gemini 2.5 Pro, DeepSeek Reasoner) by forwarding markdown prompts with code context and git diffs, returning responses with detailed cost tracking.  (1 tools) (node)
* [✅ mcp-think-tool](https://github.com/dannymac180/mcp-think-tool) ⭐ 131 | 🐛 8 | 🌐 Python | 📅 2025-04-01: Provides a dedicated space for structured reasoning during complex problem-solving tasks, enabling models to maintain timestamped thought logs that can be reviewed, cleared, and analyzed for improved performance in tasks requiring long chains of reasoning.  (4 tools) (python)
* [✅ dart-mcp-server](https://github.com/its-dart/dart-mcp-server) ⭐ 128 | 🐛 3 | 🌐 TypeScript | 📅 2026-08-24: Integrates with Dart's project management platform, enabling direct task and document management through a set of tools for creating, retrieving, updating, and filtering work items by various attributes.  (16 tools) (node)
* [✅ @llmindset/mcp-webcam](https://github.com/evalstate/mcp-webcam) ⭐ 120 | 🐛 1 | 🌐 TypeScript | 📅 2025-10-22: Enables capturing and analyzing live webcam images and screenshots for real-time visual context in AI applications.  (2 tools) (node)
* [✅ replicate-flux-mcp](https://github.com/awkoy/replicate-flux-mcp) ⭐ 107 | 🐛 0 | 🌐 TypeScript | 📅 2026-06-14: Integrates with Replicate's Flux image generation model, enabling image creation capabilities within conversation interfaces through a simple API token setup and TypeScript implementation available as both an npm module and Docker container.  (7 tools) (node)
* [✅ think\_mcp](https://github.com/rai220/think-mcp) ⭐ 106 | 🐛 3 | 🌐 Python | 📅 2026-03-07: Provides a lightweight 'think' tool for structured reasoning, enabling LLMs to pause, log thoughts, and improve multi-step problem solving without obtaining new information.  (1 tools) (python)
* [✅ grasshopper-mcp](https://github.com/alfredatnycu/grasshopper-mcp) ⭐ 97 | 🐛 8 | 🌐 C# | 📅 2025-03-22: Connects Grasshopper parametric design software with Claude through a bidirectional TCP server and Python bridge, enabling natural language control of architectural and engineering modeling workflows.  (8 tools) (python)
* [✅ mcp-server-perplexity](https://github.com/tanigami/mcp-server-perplexity) ⭐ 94 | 🐛 3 | 🌐 Python | 📅 2024-12-25: Get chat completions with citations from Perplexity API.  (1 tools) (python)
* [✅ a11y-mcp-server](https://github.com/ronantakizawa/a11ymcp) ⭐ 91 | 🐛 0 | 🌐 JavaScript | 📅 2026-03-10: Provides accessibility testing capabilities for web content using Axe-core, enabling evaluation of websites and HTML snippets against WCAG standards with detailed violation reports and remediation guidance.  (6 tools) (node)
* [✅ clinicaltrialsgov-mcp-server](https://github.com/cyanheads/clinicaltrialsgov-mcp-server) ⭐ 91 | 🐛 1 | 🌐 TypeScript | 📅 2026-08-21: Integrates with ClinicalTrials.gov REST API to search clinical trials by conditions, interventions, locations, and status, plus retrieve detailed study information by NCT ID with automatic data cleaning and local backup storage.  (2 tools) (node)
* [✅ mcp-applemusic](https://github.com/kennethreitz/mcp-applemusic) ⭐ 91 | 🐛 5 | 🌐 Python | 📅 2025-09-11: Integrates with Apple Music on macOS using Python and AppleScript to enable music playback control, library searching, playlist creation, and metadata retrieval.  (10 tools) (python)
* [✅ frame0-mcp-server](https://github.com/niklauslee/frame0-mcp-server) ⭐ 89 | 🐛 7 | 🌐 JavaScript | 📅 2026-06-17: Provides a bridge to the Frame0 diagramming application for creating and manipulating visual elements like shapes, text, and connectors with customizable properties for diagram generation and mockup creation.  (28 tools) (node)
* [✅ @bschauer/strapi-mcp-server](https://github.com/misterboe/strapi-mcp-server) ⭐ 75 | 🐛 1 | 🌐 JavaScript | 📅 2026-01-07: Integrates Strapi CMS content into workflows, enabling manipulation of data for content management and querying in Strapi-powered applications.  (5 tools) (node)
* [✅ kroger-mcp](https://github.com/cupofowls/kroger-mcp) ⭐ 70 | 🐛 5 | 🌐 Python | 📅 2026-07-09: Integrates with Kroger's grocery shopping API to enable store location management, product search with pricing and aisle information, cart operations, and order history management through OAuth2 authentication and local cart tracking.  (29 tools) (python)
* [✅ @nutrient-sdk/dws-mcp-server](https://github.com/pspdfkit/nutrient-dws-mcp-server) ⭐ 69 | 🐛 3 | 🌐 TypeScript | 📅 2026-08-28: Integrates with Nutrient Document Web Services API to provide PDF manipulation, digital signing, and document processing capabilities across multiple file formats including Office documents and images.  (3 tools) (node)
* [✅ deepseek-thinker-mcp](https://github.com/ruixingshi/deepseek-thinker-mcp) ⭐ 69 | 🐛 5 | 🌐 JavaScript | 📅 2025-03-25: Integrates with DeepSeek Thinker model to enable chain-of-thought reasoning and complex problem-solving for applications requiring advanced cognitive capabilities.  (1 tools) (node)
* [✅ @mcp-get-community/server-llm-txt](https://github.com/mcp-get/community-servers/tree/HEAD/src/server-llm-txt) ⚠️ Archived: Access up-to-date API documentation efficiently.  (3 tools) (node)
* [✅ @pollinations/chucknorris](https://github.com/pollinations/chucknorris) ⭐ 67 | 🐛 2 | 🌐 JavaScript | 📅 2025-04-11: Enhances language models by fetching specialized prompts from the L1B3RT4S repository, supporting multiple LLMs including ChatGPT, Claude, and Gemini with fallback mechanisms for educational and research purposes.  (2 tools) (node)
* [✅ mcp-guide](https://github.com/qpd-v/mcp-guide) ⭐ 66 | 🐛 1 | 🌐 JavaScript | 📅 2024-12-19: Interactive tutorial and tools for understanding, implementing, and exploring MCP concepts and capabilities.  (3 tools) (node)
* [✅ @k-jarzyna/mcp-miro](https://github.com/k-jarzyna/mcp-miro) ⭐ 65 | 🐛 3 | 🌐 TypeScript | 📅 2026-08-18: Integrates with Miro's collaborative whiteboard platform, providing over 80 tools for managing boards, creating and manipulating various item types, and handling enterprise features for visual collaboration workflows.  (97 tools) (node)
* [✅ coda-mcp](https://github.com/orellazri/coda-mcp) ⭐ 64 | 🐛 0 | 🌐 TypeScript | 📅 2026-03-31: Provides a bridge between AI and Coda documents, enabling listing, creating, reading, updating, and duplicating pages for collaborative document management and content creation.  (9 tools) (node)
* [✅ todoist-mcp](https://github.com/stanislavlysenko0912/todoist-mcp-server) ⭐ 63 | 🐛 10 | 🌐 TypeScript | 📅 2026-06-30: Full implementation of the Todoist REST API.  (35 tools) (node)
* [✅ @inkdropapp/mcp-server](https://github.com/inkdropapp/mcp-server) ⭐ 57 | 🐛 0 | 🌐 JavaScript | 📅 2026-07-17: Integrates with Inkdrop's note-taking application to enable searching, reading, creating, and updating Markdown notes directly within conversations through seven specialized tools for managing notes, notebooks, and tags.  (7 tools) (node)
* [✅ meme-mcp](https://github.com/haltakov/meme-mcp) ⭐ 56 | 🐛 1 | 🌐 JavaScript | 📅 2025-03-12: Enables meme generation through the ImgFlip API with a single tool that accepts template ID and text placeholder parameters for creating custom memes directly within conversations.  (1 tools) (node)
* [✅ random-number-mcp](https://github.com/zazencodes/random-number-mcp) ⭐ 50 | 🐛 0 | 🌐 Python | 📅 2026-07-18: Provides random number generation utilities including pseudorandom and cryptographically secure operations for integers, floats, weighted selections, list shuffling, and secure token generation.  (7 tools) (python)
* [✅ mcp-timeserver](https://github.com/secretiveshell/mcp-timeserver) ⭐ 42 | 🐛 3 | 🌐 Python | 📅 2025-01-07: Provides access to current date and time information across timezones through a custom datetime:// URI scheme and local system time tool.  (1 tools) (python)
* [✅ @pinkpixel/mcpollinations](https://github.com/pinkpixel-dev/mcpollinations) ⚠️ Archived: Enables multimodal content generation through Pollinations APIs, providing image, text, and audio creation capabilities without requiring authentication.  (9 tools) (node)
* [✅ mcp-slicer](https://github.com/zhaoyouj/mcp-slicer) ⭐ 38 | 🐛 0 | 🌐 Python | 📅 2026-08-05: Bridges medical professionals with 3D Slicer's medical imaging platform, enabling MRML node listing and direct Python code execution for advanced image analysis and visualization tasks.  (2 tools) (python)
* [✅ aistudio-mcp-server](https://github.com/eternnoir/aistudio-mcp-server) ⭐ 36 | 🐛 0 | 🌐 JavaScript | 📅 2025-07-15: Integrates with Google AI Studio/Gemini API to process multimodal content including images, videos, audio, PDFs, and text files for content generation, analysis, and document conversion tasks.  (1 tools) (node)
* [✅ mcp-ollama](https://github.com/emgeee/mcp-ollama) ⭐ 34 | 🐛 4 | 🌐 Python | 📅 2025-02-05: Integrates with Ollama for local large language model inference, enabling text generation and model management without relying on cloud APIs.  (3 tools) (python)
* [✅ mcp-hacker-news](https://github.com/paablolc/mcp-hacker-news) ⭐ 33 | 🐛 1 | 🌐 TypeScript | 📅 2025-06-09: Integrates with Hacker News through the Firebase API to retrieve stories, comments, user profiles, and job postings with filtering options for top/new/Ask HN/Show HN content, comment thread exploration, and formatted responses including discussion links.  (11 tools) (node)
* [✅ mcp-server-giphy](https://github.com/magarcia/mcp-server-giphy) ⭐ 33 | 🐛 2 | 🌐 TypeScript | 📅 2025-05-22: Allows AI models to search, retrieve, and utilize GIFs from Giphy.  (3 tools) (node)
* [✅ mcp-server-actor-critic-thinking](https://github.com/aquarius-wing/actor-critic-thinking-mcp) ⭐ 32 | 🐛 4 | 🌐 JavaScript | 📅 2025-05-26: Provides dual-perspective analysis through alternating actor and critic viewpoints for evaluating creative works, strategic decisions, and complex scenarios requiring both empathetic understanding and objective assessment.  (1 tools) (node)
* [✅ advocu-mcp-server](https://github.com/carlosazaustre/advocu-mcp-server) ⭐ 31 | 🐛 11 | 🌐 TypeScript | 📅 2026-01-20: Integrates with the Advocu platform to streamline activity reporting for Google Developer Experts, enabling submission of content creation, speaking engagements, workshops, mentoring sessions, and community interactions with detailed metrics tracking.  (7 tools) (node)
* [✅ mcp-datetime](https://github.com/zeparhyfar/mcp-datetime) ⭐ 31 | 🐛 3 | 🌐 Python | 📅 2024-12-13: Provides flexible, timezone-aware date and time formatting across various locales.  (1 tools) (python)
* [✅ pulsemcp-server](https://github.com/orliesaurus/pulsemcp-server) ⭐ 29 | 🐛 4 | 🌐 JavaScript | 📅 2025-04-23: Integrates with PulseMCP to enable querying and retrieval of MCP server and integration data for ecosystem analysis and service recommendations.  (2 tools) (node)
* [✅ @pinkpixel/taskflow-mcp](https://github.com/pinkpixel-dev/taskflow-mcp) ⭐ 28 | 🐛 2 | 🌐 TypeScript | 📅 2026-03-13: Task management system that breaks down user requests into structured tasks with subtasks, dependencies, and notes, requiring explicit approval before proceeding to ensure systematic tracking and user control.  (17 tools) (node)
* [✅ structured-thinking](https://github.com/promptly-technologies-llc/mcp-structured-thinking) ⚠️ Archived: Structures reasoning processes through defined thought stages, managing a history of thoughts with metadata for transparent, step-by-step problem solving and decision making.  (5 tools) (node)
* [✅ @delorenj/mcp-server-ticketmaster](https://github.com/delorenj/mcp-server-ticketmaster) ⭐ 25 | 🐛 3 | 🌐 TypeScript | 📅 2025-07-01: Integrates with Ticketmaster's Discovery API to search and retrieve detailed event, venue, and attraction data for entertainment planning and ticketing assistance.  (1 tools) (node)
* [✅ grok-mcp](https://github.com/bob-lance/grok-mcp) ⭐ 25 | 🐛 6 | 🌐 JavaScript | 📅 2025-06-02: Provides direct integration with Grok AI's language and vision capabilities, exposing chat completion, image understanding, and function calling tools for developers to interact with Grok's latest models.  (3 tools) (node)
* [✅ strapi-mcp](https://github.com/l33tdawg/strapi-mcp) ⭐ 25 | 🐛 0 | 🌐 JavaScript | 📅 2025-07-25: Integrates with Strapi CMS to enable creating, reading, updating, and deleting content entries with support for filtering, pagination, sorting, and media uploads through URI-based resource patterns.  (19 tools) (node)
* [✅ mcp-chain-of-draft-server](https://github.com/bsmi021/mcp-chain-of-draft-server) ⭐ 24 | 🐛 3 | 🌐 TypeScript | 📅 2025-03-26: Enables iterative reasoning through structured drafts with explicit reasoning chains, allowing for focused critiques and targeted revisions to improve problem-solving quality through systematic refinement.  (1 tools) (node)
* [✅ @kazuph/mcp-screenshot](https://github.com/kazuph/mcp-screenshot) ⭐ 23 | 🐛 2 | 🌐 JavaScript | 📅 2026-06-12: An MCP server that captures screenshots and performs OCR text recognition.  (1 tools) (node)
* [✅ @thomaswawra/server-spotify](https://github.com/superseoworld/mcp-spotify) ⭐ 23 | 🐛 2 | 🌐 TypeScript | 📅 2025-03-03: Integrates with Spotify Web API to enable music search, playback control, and playlist management for applications needing music data and content access.  (26 tools) (node)
* [✅ mcp-ipfs](https://github.com/alexbakers/mcp-ipfs) ⭐ 21 | 🐛 4 | 🌐 TypeScript | 📅 2025-04-10: Enables AI access to the IPFS Storacha Network for decentralized file storage, content addressing, and persistent data management through the w3cli interface.  (35 tools) (node)
* [✅ guardian-mcp-server](https://github.com/jbenton/guardian-mcp-server) ⭐ 20 | 🐛 0 | 🌐 TypeScript | 📅 2025-06-29: Integrates with The Guardian's Open Platform API to search articles, retrieve full content, browse sections, and perform analytical operations like author profiling and topic trend analysis with 17 specialized tools including Long Read discovery and content timeline analysis.  (16 tools) (node)
* [✅ meta-mcp-server](https://github.com/dmontgomery40/meta-mcp-server) ⭐ 20 | 🐛 1 | 🌐 JavaScript | 📅 2026-03-23: Enables dynamic creation of customized MCP servers using the MCP SDK to automate setup and manage tools and resources for specialized AI applications.  (1 tools) (node)
* [✅ heybeauty-mcp](https://github.com/chatmcp/heybeauty-mcp) ⭐ 19 | 🐛 2 | 🌐 JavaScript | 📅 2025-07-07: Provides a bridge between virtual try-on technology and clothing visualization, enabling users to see how selected items would look on them through image processing and metadata-rich clothing resources.  (2 tools) (node)
* [✅ @cyanheads/toolkit-mcp-server](https://github.com/cyanheads/toolkit-mcp-server) ⭐ 18 | 🐛 3 | 🌐 TypeScript | 📅 2026-08-22: Provides system utilities and tools for network diagnostics, monitoring, cryptography, and QR code generation.  (16 tools) (node)
* [✅ @nekzus/mcp-server](https://github.com/nekzus/npm-sentinel-mcp) ⭐ 18 | 🐛 0 | 🌐 TypeScript | 📅 2026-08-24: Utility server implementation providing dynamically registered tools for datetime handling, card operations, and schema conversion through a modular TypeScript architecture with stdio transport compatibility.  (19 tools) (node)
* [✅ ai-expert-workflow-mcp](https://github.com/bacoco/ai-expert-workflow-mcp) ⭐ 18 | 🐛 1 | 🌐 TypeScript | 📅 2025-04-29: Facilitates structured product development through specialized AI roles (Product Manager, UX Designer, Software Architect) that generate documentation and convert requirements into actionable tasks.  (3 tools) (node)
* [✅ owl-mcp](https://github.com/ai4curation/owl-mcp) ⭐ 18 | 🐛 2 | 🌐 Python | 📅 2025-06-05: Enables AI systems to manipulate Web Ontology Language (OWL) ontologies by adding, removing, and finding axioms through string-based representations in OWL functional syntax  (19 tools) (python)
* [✅ folo-mcp](https://github.com/hyoban/folo-mcp) ⚠️ Archived: Integrates with Folo RSS reader to enable querying and managing feed subscriptions, retrieving filtered entries, checking unread counts, and marking entries as read for streamlined content discovery and organization.  (5 tools) (node)
* [✅ shopify-mcp-server](https://github.com/amir-bengherbi/shopify-mcp-server) ⭐ 17 | 🐛 2 | 🌐 TypeScript | 📅 2025-01-27: Integrates with Shopify's GraphQL API to enable comprehensive store management, including product, customer, order, and discount operations.  (15 tools) (node)
* [✅ @chargebee/mcp](https://github.com/chargebee/agentkit/tree/main/modelcontextprotocol) ⚠️ Archived: MCP Server that connects AI agents to Chargebee platform.  (2 tools) (node)
* [✅ mcp-server-flomo](https://github.com/xianminx/mcp-server-flomo) ⭐ 16 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-01: A MCP server for Flomo  (1 tools) (node)
* [✅ @surajadsul02/mcp-server-salesforce](https://github.com/surajadsul/mcp-server-salesforce) ⭐ 15 | 🐛 1 | 🌐 TypeScript | 📅 2025-03-12: Integrates Claude with Salesforce, enabling natural language interactions for object management, schema exploration, data querying, and manipulation across Salesforce objects.  (7 tools) (node)
* [✅ @wangshunnn/bilibili-mcp-server](https://github.com/wangshunnn/bilibili-mcp-server) ⭐ 15 | 🐛 1 | 🌐 TypeScript | 📅 2025-05-08: Integrates with Bilibili video platform to retrieve user information, video details, and perform searches without switching contexts.  (3 tools) (node)
* [✅ @pinkpixel/notification-mcp](https://github.com/pinkpixel-dev/notification-mcp) ⭐ 14 | 🐛 2 | 🌐 JavaScript | 📅 2026-07-12: Provides audio notification system that plays platform-specific chimes when tasks complete, supporting five built-in sounds and custom audio files for non-intrusive progress alerts.  (1 tools) (node)
* [✅ productboard-mcp](https://github.com/kenjihikmatullah/productboard-mcp) ⭐ 13 | 🐛 5 | 🌐 TypeScript | 📅 2025-02-26: Integrates with the Productboard API to enable access to companies, components, features, and products for enhanced product management and feature tracking workflows.  (11 tools) (node)
* [✅ server-koboldai](https://github.com/phialsbasement/koboldcpp-mcp-server) ⭐ 13 | 🐛 1 | 🌐 JavaScript | 📅 2025-05-06: Integrates KoboldAI's text generation, enabling local language model interactions for creative writing, chatbots, and content generation.  (20 tools) (node)
* [✅ haiguitang-mcp](https://github.com/wangyafu/haiguitangmcp) ⭐ 12 | 🐛 1 | 🌐 Python | 📅 2025-04-08: Hosts a collection of 'Haiguitang' lateral thinking puzzles where users ask yes/no questions to solve mysterious scenarios through interactive critical thinking exercises.  (3 tools) (python)
* [✅ @serendipityai/markdown2pdf-mcp](https://github.com/serendipity-ai/markdown2pdf-mcp) ⭐ 10 | 🐛 3 | 🌐 JavaScript | 📅 2026-08-10: Converts Markdown documents to PDF format using Lightning Network micropayments, handling payment flows with QR codes and returning downloadable PDF URLs for pay-per-use document conversion.  (1 tools) (node)
* [✅ orly-mcp](https://github.com/princefishthrower/orly-mcp) ⭐ 10 | 🐛 0 | 🌐 Python | 📅 2025-07-12: Generates parody O'Reilly-style book covers with customizable titles, subtitles, authors, and visual themes using 40 different animal/object images and 17 color schemes.  (1 tools) (python)
* [✅ together-mcp](https://github.com/manascb1344/together-mcp-server) ⭐ 10 | 🐛 3 | 🌐 JavaScript | 📅 2025-05-23: Integrates with Together AI's Flux.1 Schnell model to provide high-quality image generation with customizable dimensions, clear error handling, and optional image saving.  (1 tools) (node)
* [✅ cursor-rules-mcp](https://github.com/iannuttall/cursor-rules-mcp) ⭐ 9 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-24: Provides a bridge to Playbooks Rules API for listing, searching, and retrieving rules with tools for processing URL-based slugs and formatting rule files with metadata  (3 tools) (node)
* [✅ @kajirita2002/esa-mcp-server](https://github.com/kajirita2002/esa-mcp-server) ⭐ 8 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-27: Provides a bridge to esa's API for document management, enabling search, creation, updates, and deletion of posts and comments with member management and categorization capabilities  (9 tools) (node)
* [✅ @shinshin86/mcp-simple-aivisspeech](https://github.com/shinshin86/mcp-simple-aivisspeech) ⭐ 8 | 🐛 0 | 🌐 JavaScript | 📅 2025-07-01: Integrates with AivisSpeech engine to provide Japanese text-to-speech synthesis with customizable voice parameters including speed, pitch, intonation, and volume for building voice-enabled applications.  (1 tools) (node)
* [✅ mcp-server-ens](https://github.com/justaname-id/ens-mcp-server) ⭐ 8 | 🐛 1 | 🌐 TypeScript | 📅 2026-01-13: Integrates with the Ethereum Name Service to resolve ENS names to addresses, perform lookups, retrieve records, check availability, get prices, and explore name history through configurable Ethereum network providers.  (8 tools) (node)
* [✅ @kunihiros/patsnap-mcp](https://github.com/kunihiros/patsnap-mcp) ⭐ 7 | 🐛 0 | 🌐 TypeScript | 📅 2025-07-31: Integrates with PatSnap's patent analytics API to provide structured access to patent trends, competitive intelligence, and innovation research through specialized tools for technology scouting and landscape analysis.  (10 tools) (node)
* [✅ anki-mcp](https://github.com/ujisati/anki-mcp) ⭐ 7 | 🐛 0 | 🌐 Python | 📅 2025-05-16: Integrates with Anki flashcard software to create, manage, and study flashcards through natural language interactions with your local Anki desktop application  (8 tools) (python)
* [✅ mercadolibre-mcp](https://github.com/lumile/mercadolibre-mcp) ⭐ 7 | 🐛 1 | 🌐 TypeScript | 📅 2025-05-28: Integrates with MercadoLibre's e-commerce platform to simplify product and seller data retrieval, enabling functions like price monitoring, inventory management, and market analysis.  (3 tools) (node)
* [✅ opentrons-mcp](https://github.com/yerbymatey/opentrons-mcp) ⭐ 7 | 🐛 0 | 🌐 JavaScript | 📅 2025-06-24: Integrates with Opentrons laboratory robots to enable natural language control of protocol upload, run management, hardware operations, and system monitoring for both OT-2 and Flex platforms.  (14 tools) (node)
* [✅ @dasheck0/face-generator](https://github.com/dasheck0/face-generator) ⭐ 6 | 🐛 0 | 🌐 JavaScript | 📅 2025-07-06: Generates customizable human face images using thispersondoesnotexist.com, offering options for shape, dimensions, and batch processing for UI prototyping and dataset creation.  (1 tools) (node)
* [✅ @kydycode/todoist-mcp-server-ext](https://github.com/kydycode/todoist-mcp-server-ext) ⭐ 6 | 🐛 0 | 🌐 JavaScript | 📅 2026-02-15: Integrates with Todoist API to provide enhanced task management capabilities including task creation, updating, completion, project organization, label management, and natural language quick-add functionality with support for subtasks, priorities, due dates, and bulk operations.  (30 tools) (node)
* [✅ @luorivergoddess/mcp-geo](https://github.com/luorivergoddess/mcp-geo) ⭐ 6 | 🐛 0 | 🌐 JavaScript | 📅 2025-05-10: Renders precise geometric images using Asymptote vector graphics language, converting mathematical code into SVG or PNG formats for technical diagrams and visualizations.  (1 tools) (node)
* [✅ dubco-mcp-server](https://github.com/gitmaxd/dubco-mcp-server-npm) ⭐ 6 | 🐛 2 | 🌐 JavaScript | 📅 2025-04-15: Provides a streamlined interface for creating, updating, and deleting short links through the Dub.co URL shortening service with robust error handling and automatic domain selection.  (3 tools) (node)
* [✅ mcp-server-restart](https://github.com/non-dirty/mcp-server-restart) ⭐ 6 | 🐛 0 | 🌐 Python | 📅 2024-12-02: Enables automated restarts of Claude Desktop on macOS by leveraging psutil to safely terminate and relaunch the application process.  (1 tools) (python)
* [✅ @automation-ai-labs/mcp-wait](https://github.com/automation-ai-labs/mcp-wait) ⭐ 5 | 🐛 2 | 🌐 JavaScript | 📅 2025-10-16: Provides a simple waiting functionality that pauses execution for specified durations (0-300 seconds) with progress reporting in 10% increments for synchronizing processes between tasks.  (1 tools) (node)
* [✅ @flipt-io/mcp-server-flipt](https://github.com/flipt-io/mcp-server-flipt) ⭐ 5 | 🐛 11 | 🌐 TypeScript | 📅 2026-07-23: Integrates with Flipt feature flag management to enable listing, creating, updating, and deleting namespaces, flags, segments, and rules for controlling feature rollouts with constraints, variants, and distributions.  (28 tools) (node)
* [✅ @jwalsh/mcp-server-qrcode](https://github.com/jwalsh/mcp-server-qrcode) ⭐ 5 | 🐛 12 | 🌐 TypeScript | 📅 2026-08-05: Integrates with the qrencode utility to generate QR codes dynamically, supporting various output formats and configuration options for flexible use in applications and workflows.  (1 tools) (node)
* [✅ @stackzero-labs/mcp](https://github.com/stackzero-labs/mcp) ⭐ 5 | 🐛 0 | 🌐 JavaScript | 📅 2025-07-01: Integrates with Stackzero Labs' commerce UI component library to browse, retrieve, and install React components optimized for e-commerce applications including ratings, image viewers, product controls, and banner blocks with installation instructions and usage examples.  (12 tools) (node)
* [✅ tv-recommender-mcp-server](https://github.com/terryso/tv-recommender-mcp-server) ⭐ 5 | 🐛 1 | 🌐 TypeScript | 📅 2025-05-05: Integrates with The Movie Database (TMDb) API to provide TV show discovery, recommendations, and streaming information based on genres, trends, and viewer preferences.  (12 tools) (node)
* [✅ @promptz/mcp](https://github.com/cremich/promptz-mcp) ⭐ 4 | 🐛 5 | 🌐 TypeScript | 📅 2025-06-06: Provides a bridge to the promptz.dev platform, enabling direct access and seamless integration of prompts into developer workflows without manual copy-pasting.  (4 tools) (node)
* [✅ pickapicon-mcp](https://github.com/leee62/pickapicon-mcp) ⭐ 4 | 🐛 1 | 🌐 JavaScript | 📅 2025-05-26: Provides a bridge to the Iconify API for searching and retrieving SVG icons from various collections, enabling quick integration of icons into projects.  (3 tools) (node)
* [✅ @fishes/mcp-easy-copy](https://github.com/f-is-h/mcp-easy-copy) ⭐ 3 | 🐛 0 | 🌐 JavaScript | 📅 2025-05-19: Utility server that automatically reads Claude Desktop configuration files and presents all available MCP services in an easy-to-copy format for quick access and troubleshooting.  (1 tools) (node)
* [✅ @folderr/folderr-mcp-server](https://github.com/folderr-tech/folderr-mcp-server) ⭐ 3 | 🐛 2 | 🌐 JavaScript | 📅 2024-12-23: Integrates with Folderr's API to enable management and communication with Folderr Assistants, facilitating tasks like listing assistants and sending questions.  (7 tools) (node)
* [✅ @missionsquad/mcp-helper-tools](https://github.com/missionsquad/mcp-helper-tools) ⭐ 3 | 🐛 1 | 🌐 TypeScript | 📅 2025-03-21: Provides utility functions for encoding/decoding, geolocation, cryptography, QR code generation, and timezone conversions with intelligent caching and rate limiting for optimized external API access.  (14 tools) (node)
* [✅ @wenbopan/things-mcp](https://github.com/wbopan/things-mcp) ⭐ 3 | 🐛 1 | 🌐 TypeScript | 📅 2025-12-20: Integrates with Things.app task management for macOS, enabling task and project creation with full metadata support, update operations including completion status, database export functionality, and summary generation through URL scheme and direct database access.  (6 tools) (node)
* [✅ mcp-rand](https://github.com/turlockmike/mcp-rand) ⭐ 3 | 🐛 5 | 🌐 TypeScript | 📅 2025-01-16: Provides diverse random generation utilities including UUIDs, numbers, strings, passwords, and more.  (7 tools) (node)
* [✅ mcp-server-novacv](https://github.com/hiretechupup/mcp-server-novacv) ⭐ 3 | 🐛 1 | 🌐 JavaScript | 📅 2025-04-03: Integrates with NovaCV API to generate, analyze, and convert resumes in various formats without dealing with complex document formatting  (4 tools) (node)
* [✅ rqbit-mcp](https://github.com/philogicae/rqbit-mcp) ⭐ 3 | 🐛 0 | 🌐 Python | 📅 2026-01-15: Integrates with the rqbit BitTorrent client to enable torrent management operations including adding torrents via magnet links, monitoring download progress, and controlling torrent lifecycle with pause, start, delete, and forget commands.  (8 tools) (python)
* [✅ linear-mcp-server](https://github.com/mckaywrigley/takeoff-linear-mcp-server) ⭐ 2 | 🐛 0 | 🌐 TypeScript | 📅 2025-03-06: Provides a bridge to the Linear project management platform, enabling task creation, tracking, and workflow management through robust API interactions and comprehensive error handling.  (5 tools) (node)
* [✅ mcp-starter](https://github.com/akrasia0/s-mcp) ⭐ 2 | 🐛 1 | 🌐 TypeScript | 📅 2025-04-30: Provides a philosophical mentor named Stern who combines rationalist thinking, stoic philosophy, and psychological insights with smart contract-based accountability for personal growth commitments.  (1 tools) (node)
* [✅ my-mcp-server](https://github.com/iamjzx/dida) ⭐ 2 | 🐛 3 | 🌐 JavaScript | 📅 2025-04-13: Integrates with Dida365 (TickTick) task management API to create, update, retrieve, and delete tasks and projects after user authorization.  (5 tools) (node)
* [✅ promptopia-mcp](https://github.com/lumile/promptopia-mcp) ⭐ 2 | 🐛 3 | 🌐 TypeScript | 📅 2025-05-23: Manage, organize, and reuse prompt templates with variable substitution and multi-message conversation structures.  (7 tools) (node)
* [✅ qanon\_mcp](https://github.com/jkingsman/qanon-mcp-server) ⭐ 2 | 🐛 0 | 🌐 Python | 📅 2025-04-05: Enables access to QAnon drops for sociological research.  (8 tools) (python)
* [✅ todoist-mcp-server](https://github.com/stevengonsalvez/todoist-mcp) ⭐ 2 | 🐛 5 | 🌐 JavaScript | 📅 2026-02-19: Provides a bridge to the Todoist task management platform, enabling advanced project and task management capabilities like creating tasks, organizing projects, managing deadlines, and team collaboration.  (33 tools) (node)
* [✅ @keegancsmith/linear-issues-mcp-server](https://github.com/keegancsmith/linear-issues-mcp-server) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2025-04-23: Integrates with Linear issue tracking to provide read-only access to issue details and comments without switching contexts.  (2 tools) (node)
* [✅ findmine-mcp](https://github.com/findmine/findmine-mcp) ⭐ 1 | 🐛 2 | 🌐 TypeScript | 📅 2025-12-03: Integrates with FindMine's product styling API to enable fashion recommendations, outfit creation, product browsing, and style guide access for e-commerce platforms.  (3 tools) (node)
* [✅ holaspirit-mcp-server](https://github.com/syucream/holaspirit-mcp-server) ⭐ 1 | 🐛 1 | 🌐 JavaScript | 📅 2025-07-01: Integrates with Holaspirit's API to provide access to organizational data for tasks like structure analysis, role management, and policy review.  (13 tools) (node)
* [✅ mcp-server-diceroll](https://github.com/shimapon/mcp-server-diceroll) ⭐ 1 | 🐛 0 | 🌐 TypeScript | 📅 2025-04-05: Provides a simple dice rolling tool that generates random rolls with customizable dice faces and quantities, returning both individual results and their sum.  (1 tools) (node)
* [✅ mcp-sleep](https://github.com/agentsworkingtogether/mcp-sleep) ⭐ 1 | 🐛 0 | 🌐 Python | 📅 2025-03-26: Enables AI systems to introduce timed pauses in execution flow, supporting both stdio and SSE transport methods with configurable timeout limits for time-based coordination and rate limiting.  (1 tools) (python)
* [✅ mcp-wait-timer](https://github.com/199-mcp/mcp-wait-timer) ⭐ 1 | 🐛 2 | 🌐 JavaScript | 📅 2025-03-29: Introduces deliberate pauses into workflows, ensuring time-dependent operations like web page rendering, background processes, or API calls have sufficient time to complete before proceeding to subsequent steps.  (1 tools) (node)
* [✅ sherlock-mcp](https://github.com/fewsats/sherlock-mcp) ⭐ 1 | 🐛 0 | 🌐 Python | 📅 2026-02-11: Enables AI to search, purchase, and manage internet domains through Sherlock Domains API, handling authentication, ICANN requirements, and DNS configuration behind the scenes.  (10 tools) (python)
* [✅ terminal-mcp-server](https://github.com/pashaydev/terminal.shop.mcp) ⭐ 1 | 🐛 0 | 🌐 JavaScript | 📅 2025-05-27: Integrates with Terminal.shop's e-commerce platform to enable browsing coffee products, managing shopping carts, and handling subscriptions with secure Stripe payment processing  (1 tools) (node)
* [✅ uuid-mcp](https://github.com/tanker327/uuid-mcp) ⭐ 1 | 🐛 1 | 🌐 TypeScript | 📅 2025-04-02: Generates timestamp-based UUID v7 identifiers that are chronologically sortable and collision-resistant, following RFC standards for reliable unique identifier creation.  (1 tools) (node)
* [✅ @mtane0412/perspective-mcp-server](https://github.com/mtane0412/perspective-mcp-server) ⭐ 0 | 🐛 0 | 🌐 JavaScript | 📅 2025-02-07: Integrates with Perspective API to analyze text toxicity and provide content moderation across multiple languages for enhanced online safety.  (2 tools) (node)
* [✅ @pylogmonmcp/time](https://github.com/pylogmon/time-mcp) ⭐ 0 | 🐛 0 | 🌐 JavaScript | 📅 2025-03-17: Provides a lightweight time server that retrieves the current time as an ISO 8601 timestamp via stdio, built by Pylogmon using the Model Context Protocol SDK.  (1 tools) (node)
* [✅ casual-mcp-server-words](https://github.com/casualgenius/mcp-servers/tree/HEAD/servers/words) ⭐ 0 | 🐛 1 | 🌐 Python | 📅 2025-06-17: Provides English word definitions, synonyms, and example usage through the Free Dictionary API for writing assistance and vocabulary building applications.  (3 tools) (python)
* [✅ mcp-apifox](https://github.com/sujianqingfeng/mcp-apifox) ⭐ 0 | 🐛 1 | 🌐 TypeScript | 📅 2025-03-14: Integrates with Apifox to provide access to API documentation, enabling developers to extract project details and fetch API information using authentication tokens via stdio transport.  (2 tools) (node)
* [✅ strateegia-mcp-server](https://github.com/filipecalegario/mcp-server-strateegia) ⭐ 0 | 🐛 1 | 🌐 TypeScript | 📅 2025-03-09: Provides a bridge to the Strateegia API, enabling quick retrieval of project workspace details through a single tool for listing projects and integrating collaborative data directly into workflows.  (1 tools) (node)
* [✅ webflow-mcp-server](https://github.com/timkjones/mcp-webflow) ⭐ 0 | 🐛 1 | 🌐 TypeScript | 📅 2025-03-22: Enables direct management of Webflow sites through API access to retrieve site information, handle custom domains, configure localization settings, and manage collections without switching contexts.  (32 tools) (node)
* [✅ @0xbeedao/mcp-taskwarrior](https://github.com/0xbeedao/mcp-taskwarrior): Integrates with Taskwarrior to enable task management through adding, updating, deleting, and listing tasks with project organization and priority level support.  (4 tools) (node)
* [✅ @ibraheem4/eventbrite-mcp](https://github.com/lucitra/eventbrite-mcp): Integrates with Eventbrite's API for event searching, detail retrieval, venue information access, and category exploration, enabling streamlined event discovery, planning, and analysis.  (4 tools) (node)
* [✅ @moralisweb3/api-mcp-server](https://github.com/moralisweb3/moralis-mcp-server): Integrates with Moralis Web3 API to enable blockchain data access, token analysis, and smart contract interactions without requiring deep Web3 development knowledge  (93 tools) (node)
* [✅ @wavestreamer/mcp](https://www.npmjs.com/package/@wavestreamer/mcp): AI forecasting platform where agents predict the future of AI — place predictions with confidence scores, evidence-based reasoning, debate, and climb the leaderboard.  (17 tools) (node)
* [✅ interactive-feedback](https://github.com/nhatpm3124/interact-mcp): Launches a cross-platform desktop feedback window that displays custom messages and collects user text input, enabling human-in-the-loop interactions during AI workflows.  (2 tools) (python)
* [✅ time-mcp-local](https://github.com/punkpeye/time-mcp-local): Provides time and timezone conversion capabilities, enabling current time retrieval and timezone conversions using IANA names for accurate and localized time-based functionalities.  (2 tools) (python)
* [✅ webdev-mcp](https://github.com/zueai/webdev-mcp): Enables AI to capture screenshots from multiple displays on macOS, Windows, and Linux for analyzing visual content, debugging UI issues, or assisting with design tasks  (2 tools) (node)
* [✅ ygg-torrent-mcp](https://github.com/philogicae/ygg-torrent-mcp): Provides secure access to YggTorrent through an unofficial API wrapper, enabling torrent searching with category filtering, detailed metadata retrieval, and magnet link generation with automatic passkey injection for authenticated downloads.  (5 tools) (python)

</details>

***

> _Enhansomed by [enhansome](https://github.com/enhansome) on 2026-09-01._
