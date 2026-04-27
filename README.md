# @avclabs.ai/enhance-mcp (Node.js)

[![npm version](https://badge.fury.io/js/@avclabs%2Fenhance-mcp.svg)](https://www.npmjs.com/package/@avclabs.ai/enhance-mcp)
[![Node.js >=18](https://img.shields.io/badge/node-%3E%3D18-brightgreen.svg)](https://nodejs.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[中文文档](README_CN.md) | English

A video enhancement service based on the MCP protocol, acting as an MCP Client-Server to interact with a FastAPI HTTP Server.

## Features

The following MCP Tools are provided:
- `create_task` - Create a video enhancement task (supports URL or local file upload)
- `get_task_status` - Query task status
- `enhance_video_sync` - Synchronously enhance a video (blocking wait)

## Installation

### Install from npm (Recommended)

```bash
npm install -g @avclabs.ai/enhance-mcp
```

Or use yarn/pnpm:
```bash
yarn global add @avclabs.ai/enhance-mcp
pnpm add -g @avclabs.ai/enhance-mcp
```

### Install from Source

```bash
git clone https://github.com/avclabs/enhance-mcp.git
cd js_client
npm install
npm run build
```

## Usage

### 1. Command Line

Use directly after global installation:
```bash
avclabs-enhance-mcp --base-url https://mcp.avc.ai --api-key your-api-key
```

Or use environment variables:
```bash
# Windows PowerShell
$env:HTTP_API_BASE_URL="https://mcp.avc.ai"
$env:HTTP_API_KEY="your-api-key"
avclabs-enhance-mcp

# Windows CMD
set HTTP_API_BASE_URL=https://mcp.avc.ai
set HTTP_API_KEY=your-api-key
avclabs-enhance-mcp

# macOS/Linux
export HTTP_API_BASE_URL=https://mcp.avc.ai
export HTTP_API_KEY=your-api-key
avclabs-enhance-mcp
```

### 2. Configure in Claude Desktop

Edit the Claude Desktop configuration file:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`

**Windows**: `%APPDATA%/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "video-enhancement": {
      "command": "avclabs-enhance-mcp",
      "args": [
        "--base-url",
        "https://mcp.avc.ai",
        "--api-key",
        "your-api-key"
      ]
    }
  }
}
```

### 3. Use with npx (No Global Installation Required)

```bash
npx @avclabs.ai/enhance-mcp --base-url https://mcp.avc.ai --api-key your-api-key
```

Claude Desktop configuration:
```json
{
  "mcpServers": {
    "video-enhancement": {
      "command": "npx",
      "args": [
        "@avclabs.ai/enhance-mcp",
        "--base-url",
        "https://mcp.avc.ai",
        "--api-key",
        "your-api-key"
      ]
    }
  }
}
```

## Provided Tools

### create_task

Create a video enhancement task (asynchronous).

**Parameters:**
- `video_source` (string, required): Video URL or local file path
- `type` (string, optional): Upload type, defaults to "url"
  - Options: `"url"` - Network video URL, `"local"` - Local file path
- `resolution` (string, optional): Target resolution, defaults to 720p
  - Options: 480p, 540p, 720p, 1080p, 2k

**Example:**
```json
// URL mode
{
  "video_source": "https://example.com/video.mp4",
  "type": "url",
  "resolution": "1080p"
}

// Local file mode
{
  "video_source": "/path/to/local/video.mp4",
  "type": "local",
  "resolution": "1080p"
}
```

**Returns:**
```json
{
  "success": true,
  "task_id": "xxx",
  "status": "wait"
}
```

### get_task_status

Query task status.

**Parameters:**
- `task_id` (string, required): Task ID

**Example:**
```json
{
  "task_id": "task-123-abc"
}
```

**Returns:**
```json
{
  "success": true,
  "task_id": "xxx",
  "status": "completed",
  "progress": 100,
  "video_url": "https://...",
  "error_message": null,
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:01:00Z"
}
```

### enhance_video_sync

Synchronously enhance a video (blocking wait until completion).

**Parameters:**
- `video_source` (string, required): Video URL or local file path
- `type` (string, optional): Upload type, defaults to "url"
  - Options: `"url"` - Network video URL, `"local"` - Local file path
- `resolution` (string, optional): Target resolution, defaults to 720p
- `poll_interval` (number, optional): Polling interval in seconds, defaults to 5
- `timeout` (number, optional): Timeout in seconds, defaults to 600

**Example:**
```json
{
  "video_source": "https://example.com/video.mp4",
  "type": "url",
  "resolution": "1080p",
  "poll_interval": 5,
  "timeout": 600
}
```

**Returns:**
```json
{
  "success": true,
  "task_id": "xxx",
  "status": "completed",
  "progress": 100,
  "video_url": "https://..."
}
```

## File Upload Notes

When `type` is set to `"local"`, the MCP Server will:
1. Read the local file
2. Encode the file as base64
3. Upload it to the video enhancement service

**Limitations:**
- Maximum file size: 100MB

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `HTTP_API_BASE_URL` | FastAPI HTTP Server address | `https://mcp.avc.ai` |
| `HTTP_API_KEY` | API authentication key | None |

## Development

```bash
# Clone the repository
git clone https://github.com/avclabs/enhance-mcp.git
cd js_client

# Install dependencies
npm install

# Development mode (auto-compile)
npm run dev

# Build
npm run build
```

## License

MIT License - See [LICENSE](LICENSE) file for details.
