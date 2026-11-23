# ITS - DevOps & AI Resources

## Introduction

Resources for ITS students learning to build, operate, and improve software with AI. Start with practical projects, then use debugging and experiments to understand the systems underneath them.

## Resources

- [DevOps Resources](./DEVOPS.md): A document on discussed DevOps topics (Git, Actions, Docker, K8S, Deployments).
- [AI & ML Resources](./AI_ML.md): A document on discussed AI & ML topics (Models, LLMs, ML.NET, Neural networks).

## Projects

### 2025
- [JParser](./projects/2025/jparser) uses TypeScript and Deno to explore how a lexer, parser, and formatter handle JSON.
- [PokeBattle](./projects/2025/pokebattle): API and UI in docker with example GitHub actions workflows and ML.NET examples.
- [WalkGame](./projects/2025/walk-game) is a PyTorch game for exploring how an agent learns to navigate an environment.

### 2024

- [ITS SQL](./projects/2024/its-sql): A distributed No-SQL database engine similar to Cosmos DB with SQL-like query language.

### 2023

- [GitMess](./projects/2023/gitmess): A GitHub backend clone running in Docker for managing repositories.

## Systems labs

Use AI to implement changes in these labs. Before each run, predict the result. Afterward, trace the code or inspect a measurement that explains what happened.

| Area | What to understand | Small experiment |
| --- | --- | --- |
| Computer architecture | CPU execution, caches, RAM, storage, and the cost of moving data | Compare sequential and random access over a large array; measure time and explain the difference. |
| Operating systems | Processes, threads, virtual memory, files, permissions, and resource limits | Run a service with a memory limit, observe its failure, and inspect the logs. |
| Networks | DNS, TCP, TLS, HTTP, latency, timeouts, and connection reuse | Trace a request and simulate a slow dependency; distinguish a timeout from an application error. |
| Data systems | Indexes, transactions, isolation, consistency, and recovery | Compare query plans before and after an index; test concurrent updates and restore a backup. |
| System architecture | Boundaries, queues, caching, retries, idempotency, and backpressure | Deliver the same event twice and prove that an operation is applied only once. |

### Example projects

| Project | Practice | Evidence to show |
| --- | --- | --- |
| Operate a small API and database | Architecture, data modeling, deployment, and observability | Trace a request, enforce access control, diagnose a slow query, and demonstrate backup restoration. |
| Build a background job system | Queues, timeouts, retries, and idempotency | Restart a worker during a job and verify the intended result after redelivery. |
| Improve an unfamiliar application | Code reading, debugging, and performance | Reproduce a defect, trace its cause, fix it, and show a regression check or a measured improvement. |
| Create an agent development harness | Task specification, repeatable environments, and verification | Give an agent a bounded issue, record its attempts, and demonstrate that the checks reject a deliberately broken solution. |
| Compare two system designs | Tradeoffs and measurement | Test the same workload against a simple design and one alternative; explain latency, cost, and failure behavior. |

In the project review, walk through one design decision and the code that implements it. Reproduce a failure, show how you verified the fix, and identify which part you delegated. The [AI guide](./AI_ML.md#ai-assisted-software-development) has a sample task brief.

### Optional internals exercises

- **Language internals.** Start in [JParser](./projects/2025/jparser/main.ts). Predict the tokens and parsed value for `[1,[2]]`, then step through the parser. Try `[1 2]` and compare the result with `JSON.parse`. Find the branch responsible for the difference and add a check that catches it. A full compiler is an optional next project if language implementation interests you.
- **Storage**: Build a tiny key-value log, interrupt a write, and investigate recovery. Compare lookup behavior with and without an index.
- **HTTP**: Inspect raw requests and implement a small parser in a local lab. Explain how malformed input is handled before comparing with a production server.
- **Version control**: Store content by hash and represent parent commits. Explain how to detect changed content and where merge conflicts come from.

### Courses and demonstrations

| Resource | Use it for |
| --- | --- |
| [Nand to Tetris](https://www.nand2tetris.org/course) | Project 5 covers how a CPU, memory, and instruction ROM fit together. |
| [MIT Missing Semester](https://missing.csail.mit.edu/2026/) | Shell tools, Git, debugging, profiling, shipping software, and agentic coding. |
| [Hussein Nasser](https://www.husseinnasser.com/p/software-engineering.html) | Backend engineering, networking, and database behavior. |
| [Salvatore Sanfilippo](https://www.youtube.com/@antirez) | C programming and systems design discussions. |
| [MorroLinux](https://www.youtube.com/@morrolinux) | Linux demonstrations in Italian. |
| [ThePrimeagen](https://www.youtube.com/@ThePrimeagen) | Vim motions and editor workflows. |
