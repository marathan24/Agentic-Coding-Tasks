# Terminal Bench Tasks (Python and Javascript)

This repository contains coding tasks designed to test agents on their ability to solve realistic software engineering problems.
This follows the Directory structure of Terminal Bench 2.0 and we have used harbor to evaluate these agents.

(The readme is yet to be updated by screenshots of tasks failed. We haven't pushed trials folder to this repo.)

## Prerequisites

### 1. Start Docker

Before running any tasks, ensure Docker is running on your machine:

```bash
# Start Docker Desktop (macOS/Windows) or Docker daemon (Linux)
docker info  # Verify Docker is running
```

> **Note:** Docker must be running for the task environments to work properly.

### 2. Install Dependencies

Install the required dependencies:

```bash
uv sync
```

---

## Task 1: DB Migration Local Storage
**Location:** `datasets/agentic_coding_tasks/db-migration-local-storage`

### Overview
Your production SQLite database is being decommissioned and will no longer be available. The FastAPI service currently loads a 400-record CSV into SQLite on startup and serves queries via `/query?type1=X&type2=Y`. Your task is to refactor the code in `/app/service.py` to work without SQLite while maintaining identical API behavior.

### Testing the Oracle
Before running the task with any agent, it is important to test the oracle agent to ensure the task setup is correct. This command should return a reward of 1.0:

```bash
uv run harbor trials start -p datasets/agentic_coding_tasks/db-migration-local-storage/
```

### Running the Task

**With Aider agent and GPT-5.1:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/db-migration-local-storage \
  -a aider \
  -m openai/gpt-5.1
```

**With Codex agent and GPT-5.1-Codex:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/db-migration-local-storage \
  -a codex \
  -m openai/gpt-5.1-codex
```

**With Claude Code agent and Claude Sonnet 4.5:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/db-migration-local-storage \
  -a claude-code \
  -m anthropic/claude-sonnet-4-5-20250929 \
  --force-build \
  --no-delete
```

---

## Task 2: Fix CPU-Bound Blocking
**Location:** `datasets/agentic_coding_tasks/fix-cpu-bound-blocking`

### Overview
The application needs to handle intensive calculations without blocking other async operations. The `/calculate` endpoint performs heavy CPU work that blocks the event loop, preventing the heartbeat from updating. Fix the code in `/app/cpu_task.py` to ensure intensive work does not block other coroutines and the `/heartbeat` endpoint continues updating every second.

### Testing the Oracle
Before running the task with any agent, it is important to test the oracle agent to ensure the task setup is correct. This command should return a reward of 1.0:

```bash
uv run harbor trials start -p datasets/agentic_coding_tasks/fix-cpu-bound-blocking/
```

### Running the Task

**With Aider agent and GPT-5.1:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-cpu-bound-blocking \
  -a aider \
  -m openai/gpt-5.1
```

**With Codex agent and GPT-5.1-Codex:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-cpu-bound-blocking \
  -a codex \
  -m openai/gpt-5.1-codex
```

**With Claude Code agent and Claude Sonnet 4.5:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-cpu-bound-blocking \
  -a claude-code \
  -m anthropic/claude-sonnet-4-5-20250929 \
  --force-build \
  --no-delete
```

---

## Task 3: Fix Document Index Sync
**Location:** `datasets/agentic_coding_tasks/fix-document-index-sync`

### Overview
A document management system processes documents asynchronously in parallel batches. Index Documents maintain references to child documents through a `childDocumentIds` array. Due to parallel processing, documents complete at unpredictable times, creating race conditions. The current implementation at `/app/document_service.py` has a critical bug where the Index Document's `childDocumentIds` array often contains null or missing `jsonId` references. Implement a robust three-case synchronization strategy to guarantee eventual consistency regardless of processing order.

### Testing the Oracle
Before running the task with any agent, it is important to test the oracle agent to ensure the task setup is correct. This command should return a reward of 1.0:

```bash
uv run harbor trials start -p datasets/agentic_coding_tasks/fix-document-index-sync/
```

### Running the Task

**With Aider agent and GPT-5.1:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-document-index-sync \
  -a aider \
  -m openai/gpt-5.1
```




**With Codex agent and GPT-5.1-Codex:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-document-index-sync \
  -a codex \
  -m openai/gpt-5.1-codex
```

**With Claude Code agent and Claude Sonnet 4.5:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-document-index-sync \
  -a claude-code \
  -m anthropic/claude-sonnet-4-5-20250929 \
  --force-build \
  --no-delete
```

---

## Task 4: Fix Fire-and-Forget Analytics
**Location:** `datasets/agentic_coding_tasks/fix-fire-and-forget-analytics`

### Overview
A streaming API service generates responses chunk-by-chunk while recording analytics data. The current implementation at `/app/service.py` has a critical performance bug where analytics operations execute synchronously within the streaming response generator, causing the stream to block and stutter while waiting for database writes. Implement a true fire-and-forget analytics system using asyncio queues and background worker tasks to decouple analytics operations from the streaming response path entirely.

### Testing the Oracle
Before running the task with any agent, it is important to test the oracle agent to ensure the task setup is correct. This command should return a reward of 1.0:

```bash
uv run harbor trials start -p datasets/agentic_coding_tasks/fix-fire-and-forget-analytics/
```

### Running the Task

**With Aider agent and GPT-5.1:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-fire-and-forget-analytics \
  -a aider \
  -m openai/gpt-5.1
```

**With Codex agent and GPT-5.1-Codex:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-fire-and-forget-analytics \
  -a codex \
  -m openai/gpt-5.1-codex
```

**With Claude Code agent and Claude Sonnet 4.5:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-fire-and-forget-analytics \
  -a claude-code \
  -m anthropic/claude-sonnet-4-5-20250929 \
  --force-build \
  --no-delete
```

---

## Task 5: Fix Streaming Heartbeat
**Location:** `datasets/agentic_coding_tasks/fix-streaming-heartbeat`

### Overview
A Flask streaming API service generates large responses chunk-by-chunk. The current implementation at `/app/service.py` has a critical streaming timeout bug where long-running processing operations (35 seconds) cause the HTTP connection to be terminated before the stream completes. Clients have a 30-second read timeout, and when they don't receive any data within 30 seconds, they terminate the connection. Fix the streaming timeout issue to ensure responses complete successfully even with long processing operations.

### Testing the Oracle
Before running the task with any agent, it is important to test the oracle agent to ensure the task setup is correct. This command should return a reward of 1.0:

```bash
uv run harbor trials start -p datasets/agentic_coding_tasks/fix-streaming-heartbeat/
```

### Running the Task

**With Aider agent and GPT-5.1:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-streaming-heartbeat \
  -a aider \
  -m openai/gpt-5.1
```

**With Codex agent and GPT-5.1-Codex:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-streaming-heartbeat \
  -a codex \
  -m openai/gpt-5.1-codex
```

**With Claude Code agent and Claude Sonnet 4.5:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/fix-streaming-heartbeat \
  -a claude-code \
  -m anthropic/claude-sonnet-4-5-20250929 \
  --force-build \
  --no-delete
```

---

## Task 6: Optimize Product Catalog
**Location:** `datasets/agentic_coding_tasks/optimize-product-catalog`

### Overview
The product catalog API is experiencing severe performance issues. The `/products/search` endpoint is taking multiple seconds to return results, causing timeouts and poor user experience. Optimize the code in `/app/catalog_service.py` to dramatically improve query performance while maintaining identical API behavior. The optimized solution must handle all search queries in under 100ms (currently taking 2-4 seconds).

### Testing the Oracle
Before running the task with any agent, it is important to test the oracle agent to ensure the task setup is correct. This command should return a reward of 1.0:

```bash
uv run harbor trials start -p datasets/agentic_coding_tasks/optimize-product-catalog/
```

### Running the Task

**With Aider agent and GPT-5.1:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/optimize-product-catalog \
  -a aider \
  -m openai/gpt-5.1
```

**With Codex agent and GPT-5.1-Codex:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/optimize-product-catalog \
  -a codex \
  -m openai/gpt-5.1-codex
```

**With Claude Code agent and Claude Sonnet 4.5:**

```bash
uv run harbor trials start \
  -p datasets/agentic_coding_tasks/optimize-product-catalog \
  -a claude-code \
  -m anthropic/claude-sonnet-4-5-20250929 \
  --force-build \
  --no-delete
```



